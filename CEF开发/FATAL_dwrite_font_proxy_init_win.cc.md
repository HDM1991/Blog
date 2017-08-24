# 错误信息
> [18376] [0719/113132.274:FATAL:dwrite_font_proxy_init_win.cc(88)] Check failed: fallback_available == base::win::GetVersion() > base::win::VERSION_WIN8 (1 vs. 0)

# 原因与解决方案
导致这个错误的原因是 In Windows 8.1 and Windows 10, the GetVersion and GetVersionEx functions have been deprecated. While you can still call the deprecated functions, if your application does not specifically target Windows 8.1 or Windows 10, you will get Windows 8 version (6.2.0.0).

解决方案：
https://msdn.microsoft.com/en-us/library/windows/desktop/dn481241(v=vs.85).aspx

现在还没搞清楚的是 Visual Studio 2015 中 app manifest file 的添加，还有一些设置。

在官方给出的一些示例代码中，比如 cefsimple，cefclient，对这个 app manifest file 的处理，是使用了自己编写的而一些脚本。没有使用 Visual Stuido 提供的工具。

> setlocal
mt.exe -nologo -manifest "E:/Documents/Bitbucket/cef-project/third_party/cef/cef_binary_3.3029.1619.geeeb5d7_windows32/tests/cefsimple/cefsimple.exe.manifest" "E:/Documents/Bitbucket/cef-project/third_party/cef/cef_binary_3.3029.1619.geeeb5d7_windows32/tests/cefsimple/compatibility.manifest" -outputresource:"E:/Documents/Bitbucket/cef-project/build/third_party/cef/cef_binary_3.3029.1619.geeeb5d7_windows32/tests/cefsimple/Debug/cefsimple.exe";#1


# how to create an application manifest for a C or C++ project type
https://msdn.microsoft.com/en-us/library/bb756929.aspx

## C or C++ Project
The following procedure details how to create an application manifest for a C or C++ project type in Visual Studio 2005.
## To create a manifest for a C or C++ project in Microsoft Visual Studio 2005

1. Open your project in Microsoft Visual Studio 2005.
2. Under Project, select Properties.
3. In Properties, select Manifest Tool, and then select Input and Output.
4. Add in the name of your application manifest file under Additional manifest files.
5. Rebuild your application.

Note   The updated manifests that include explicit version tags will permit the application to run correctly on both Windows Vista and Windows XP.

## Managed Code (C#, J# and Visual Basic)
Visual Studio does not currently embed a default application manifest into managed code. For managed code, the developer should insert a default application manifest into the target executable using mt.exe. The following procedure details this process.

## To insert a default manifest file into the target executable with mt.exe
1. Use a text editor, such as Windows Notepad, to create a default manifest file, temp.manifest.
2. Use mt.exe to insert the manifest.  The command would be: mt.exe –manifest temp.manifest –outputresource:YourApp.exe;#1.

## Adding the Application Manifest as a Step in Visual Studio Post-Build
Adding the application manifest can be automated as a post-build step as well. This option is available for C/C++ and for the two managed code languages of C# and J#.
NoteNote
The IDE does not currently include a post-build option for a Visual Basic application.
To automate the addition of the application manifest as a post-build step, place the following line as a post build task in your Visual Studio project's Project Properties:
mt.exe -manifest "$(ProjectDir)$(TargetName).exe.manifest" -updateresource:"$(TargetDir)$(TargetName).exe;#1"

