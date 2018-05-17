# JavaScriptIntegration
JS APIs that communicate between the browser and renderer processes should be designed using asynchronous callbacks. See the "Asynchronous JavaScript Bindings" section of the GeneralUsage wiki page for more information.

## ExecuteJavaScript

This function is available in both the browser process and the renderer process and can safely be used from outside of a JS context.

The ExecuteJavaScript() function can be used to interact with functions and variables in the frame's JS context.

## Window Binding
Window binding allows the client application to attach values to a frame's window object.


## Extensions


# JS Functions

# Working with Contexts


# Asynchronous JavaScript Bindings
JavaScriptIntegration is implemented in the render process but frequently need to communicate with the browser process. The JavaScript APIs themselves should be designed to work asynchronously using closures and promises.

An application interacts with the router by passing it data from
standard CEF C++ callbacks (OnBeforeBrowse, OnProcessMessageRecieved,
OnContextCreated, etc). The renderer-side router supports generic JavaScript
callback registration and execution while the browser-side router supports
application-specific logic via one or more application-provided Handler
instances.

## Generic Message Router
CEF provides a generic implementation for routing asynchronous messages between JavaScript running in the renderer process and C++ running in the browser process.

这个东西的基本用法目前大致是搞清楚了，但高级一点用法暂时还不明白，可以在研究下。