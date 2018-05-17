# .ready()
The .ready() method offers a way to run JavaScript code as soon as the page's Document Object Model (DOM) becomes safe to manipulate. 


Browsers also provide the load event on the window object. When this event fires it indicates that all assets on the page have loaded, including images. This event can be watched in jQuery using $( window ).on( "load", handler ). In cases where code relies on loaded assets (for example, if the dimensions of an image are required), the code should be placed in a handler for the load event instead.

经过测试， *.ready()* 先于 *$( window ).on( "load", handler )*

so. what's DOM? DOM is fully loaded means what?


另外需要注意的是，以后直接使用如下语法就行了。

$(function() {
  // Handler for .ready() called.
});


Note that although the DOM always becomes ready before the page is fully loaded, it is usually not safe to attach a load event listener in code executed during a .ready() handler. For example, scripts can be loaded dynamically long after the page has loaded using methods such as $.getScript(). Although handlers added by .ready() will always be executed in a dynamically loaded script, the window's load event has already occurred and those listeners will never run.

不理解上面这段话的意思。


# .load()
The load event is sent to an element when it and all sub-elements have been completely loaded. This event can be sent to any element associated with a URL: images, scripts, frames, iframes, and the window object.


# .ready() 与 $(window).load() 的先后顺序
做了一个小实验，在 *.ready()* 执行一个 50000 次的循环，发现这个 *$(window).load()* 总是在 .ready 执行后才触发。

# DOM
The Document Object Model (DOM) is a programming interface for HTML and XML documents.

It represents the page so that programs can change the document structure, style, and content. The DOM represents the document as nodes and objects.

[1]: http://api.jquery.com/ready/ ".ready()"
[2]: http://api.jquery.com/load-event/ ".load()"
[3]: https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction "Introduction to the DOM
"