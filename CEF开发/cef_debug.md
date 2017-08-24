cef 是多进程模型。在 Visual Studio 中调试时，默认情况下只能调试主进程，所以怎样调试子进程就是一个问题。

# Debugging Chromium on Windows

## Getting started
You can use Visual Studio's built-in debugger or WinDBG to debug Chromium. You don't need to use the IDE to build in order to use the debugger: Ninja is used to build Chromium and most developers invoke it from a command prompt, and then open the IDE for debugging as necessary. To start debugging an executable from the command line:

    devenv /debugexe out\Debug\chrome.exe <options to Chromium can go here>

This assumes you have Visual Studio installed and have devenv.exe on your path. Note that the path to the binary must use backslashes and must include the ".exe" suffix or Visual Studio will open and do nothing.

Goma (the internal Google distributed build) defaults to symbol\_level = 1 which means source-level debugging will not work. If you want full debugging with goma then you need to explicitly set symbol\_level = 2, and is\_win\_fastlink = true, however this does tend to expose bugs in debuggers, so caveat debugger, and be sure to use the very latest versions of the debuggers.

TODO: 对 Goma 这个东西我不太了解，不过应该和 Ninja 一样，都可以用来 build Chromium，只不过 Goma 是 distributed build，而且是 Google 内部用过的，所以上面提到的关于 Goma 的东西，我们应该用不到。


### Profiles

It's a good idea to use a different profile for your debugging. If you are debugging Google Chrome branded builds, or use a Chromium build as your primary browser, the profiles can collide so you can't run both at once, and your stable browser might see profile versions from the future (Google Chrome and Chromium use different profile directories by default so won't collide). Use the command-line option:

    --user-data-dir=c:\tmp\my_debug_profile (replace the path as necessary)

Using the IDE, go to the Debugging tab of the properties of the chrome project, and set the Command Arguments.


[User Data Directory][https://chromium.googlesource.com/chromium/src/+/master/docs/user_data_dir.md]
#### Introduction
The user data directory contains profile data such as history, bookmarks, and cookies, as well as other per-installation local state.
Each profile is a subdirectory (often Default) within the user data directory.

To determine the user data directory for a running Chrome instance:

* Navigate to chrome://version
* Look for the Profile Path field. This gives the path to the profile directory.
* The user data directory is the parent of the profile directory.

CEF 好像没有 user data directory，所以这点我们就不用考虑了。

### Chrome debug log
Enable Chrome debug logging to a file by passing --enable-logging --v=1 command-line flags at startup. Debug builds place the chrome_debug.log file in the out\Debug directory. Release builds place the file in the top level of the user data Chromium app directory, which is OS-version-dependent. For more information, see logging and user data directory details.


在 CEF 的调试中，在命令行传入 --enable-logging --v=1 后，有如下两点变化：

1. 调试时会弹出一个命令行窗口，显示 log 信息。
2. 在所调试程序的目录下，deubg.log 文件中也会写入命令行窗口中的信息。这个 deubg.log 文件即使在命令行中没有传入上述选项，也会创建，只不过在其中写入的信息没有传入上述选项后详细。

这点还是挺有用的。

### Symbol server

If you are debugging official Google Chrome release builds, use the symbol server:

https://chromium-browser-symsrv.commondatastorage.googleapis.com.

In Visual Studio, this goes in Tools > Options under Debugging > Symbols. It will be faster if you set up a local cache in a empty directory on your computer.

这点暂时用不到。

## Multi-process issues
Chromium can be challenging to debug because of its multi-process architecture. When you select Run in the debugger, only the main browser process will be debugged. The code that actually renders web pages (the Renderer) and the plugins will be in separate processes that's not (yet!) being debugged. The ProcessExplorer tool has a process tree view where you can see how these processes are related. You can also get the process IDs associated with each tab from the Chrome Task Manager (right-click on an empty area of the window title bar to open).

### Single-process mode
The easiest way to debug issues is to run Chromium in single-process mode. This will allow you to see the entire state of the program without extra work (although it will still have many threads). To use single-process mode, add the command-line flag

    --single-process

This approach isn't perfect because some problems won't manifest themselves in this mode and some features don't work and worker threads are still spawned into new processes.

这个不太好，就拿 message_router 这个例子来讲，使用 Single-process mode，直接就不能工作了，这应该是跟 message\_router 的代码逻辑相关，所以这个方法还是不要用。

### Manually attaching to a child process
You can attach to the running child processes with the debugger. Select Tools > Attach to Process and click the chrome.exe process you want to attach to. Before attaching, make sure you have selected only Native code when attaching to the process This is done by clicking Select... in the Attach to Process window and only checking Native. If you forget this, it may attempt to attach in "WebKit" mode to debug JavaScript, and you'll get an error message "An operation is not legal in the current state."

You can now debug the two processes as if they were one. When you are debugging multiple processes, open the Debug > Windows > Processes window to switch between them. 

Sometimes you are debugging something that only happens on startup, and want to see the child process as soon as it starts. Use:

    --renderer-startup-dialog --no-sandbox

You have to disable the sandbox or the dialog box will be prohibited from showing. When the dialog appears, visit Tools > Attach to Process and attach to the process showing the Renderer startup dialog. Now you're debugging in the renderer and can continue execution by pressing OK in the dialog.

Startup dialogs also exist for other child process types: --gpu-startup-dialog, --ppapi-startup-dialog, --plugin-startup-dialog (for NPAPI).

You can also try the vs-chromium plug-in to attach to the right processes.

使用  --renderer-startup-dialog --no-sandbox 选项的这个方法很好，在命令行传入这个选项后，当子进程创建时，会弹出一个对话框，显示当前创建的子进程的 ID，类型，然后你就可以再这个时候 attach 这个子进程。

然后这个 vs-chromium plug-in，对调试 CEF 没什么用。

### Semi-automatically attaching the debugger to child processes

The following flags cause child processes to wait for 60 seconds in a busy loop for a debugger to attach to the process. Once either condition is true, it continues on; no exception is thrown.

    --wait-for-debugger-children[=filter]

The filter, if provided, will fire only if it matches the --type parameter to the process. Values include renderer, plugin (for NPAPI), ppapi, gpu-process, and utility.

When using this option, it may be helpful to limit the number of renderer processes spawned, using:

    --renderer-process-limit=1

不好用。


## Visual Studio hints

### Debug visualizers
Chrome's custom debug visualizers should be added to the pdb files and automatically picked up by Visual Studio. The definitions are in //tools/win/DebugVisualizers if you need to modify them (the BUILD.gn file there has additional instructions).

### Don't step into trivial functions
The debugger can be configured to automatically not step into functions based on regular expression. Edit default.natstepfilter in the following directory:
For Visual Studio 2015: C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Packages\Debugger\Visualizers (for all users)
or %USERPROFILE%\My Documents\Visual Studio 2015\Visualizers (for the current user only)
For Visual Studio 2017 Pro: C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\Common7\Packages\Debugger\Visualizers (for all users)
or %USERPROFILE%\My Documents\Visual Studio 2017\Visualizers (for the current user only)
Add regular expressions of functions to not step into. Remember to regex-escape and XML-escape them, e.g. &lt; for < and \. for a literal dot. Example:

    <Function><Name>operator new</Name><Action>NoStepInto</Action></Function>
    <Function><Name>operator delete</Name><Action>NoStepInto</Action></Function>
    <!-- Skip everything in std -->
    <Function><Name>std::.*</Name><Action>NoStepInto</Action></Function>
    <!-- all methods on WebKit OwnPtr and variants, ... WTF::*Ptr<*>::* -->
    <Function><Name>WTF::.*Ptr&lt;.*&gt;::.*</Name><Action>NoStepInto</Action></Function>

This file is read at start of a debugging session (F5), so you don't need to restart Visual Studio after changing it.
More info: Andy Pennel's Blog, microsoft email thread

