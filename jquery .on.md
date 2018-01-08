# .on()
    .on( events [, selector ] [, data ], handler )

 
TSZXype: String
A selector string to filter the descendants of the selected elements that trigger the event. If the selector is null or omitted, the event is always triggered when it reaches the selected element.

selector 这个参数，刚开始我的理解是，如果设置了，还是为当前元素绑定事件处理函数，只是只有这个事件是从 selector 指定的子元素中触发时，才会执行事件处理函数；但并不是这样，以下面这个实例为例，
    
    <!DOCTYPE html>
    <html>
    <head>
        <title>测试</title>
    </head>
    <body>

    <div id="container" style="padding:10px;background-color: #0b93d5;">
        <button id="test">Test</button>
        <button id="test2">test2</button>
        <a href="#">xxxx</a>
    </div>

    <script type="text/javascript" src="/assets/jquery/dist/jquery.min.js"></script>
    <script type="text/javascript">
        $('#container').on('click', 'button', {}, (e) => {
        // $('#container').on('click',  (e) => {
            console.log(e);
        });
    </script>
    </body>
    </html>

只有在点击两个按钮时才会触发事件处理函数，而且 e.currentTarget 是所点击的按钮，比如现在点击了 id 为 test 的按钮，则 e 如下：

    r.Event {originalEvent: MouseEvent, type: "click", isDefaultPrevented: ƒ, target: button#test, currentTarget: button#test, …}

这刚开始让我有点误解，我觉得 currentTarget 应该是 div#container，但这就是 jquery 的做法，上述示例中，我们依然是给 div 绑定了一个事件处理函数，并没有给两个 button 绑定事件处理函数，但它的行为就是这样，jquery 称这种行为叫 delegated.

## Event names and namespaces
An event name can be qualified by event namespaces that simplify removing or triggering the event. For example, "click.myPlugin.simple" defines both the myPlugin and simple namespaces for this particular click event. A click event handler attached via that string could be removed with .off("click.myPlugin") or .off("click.simple") without disturbing other click handlers attached to the elements. Namespaces are similar to CSS classes in that they are not hierarchical; only one name needs to match. Namespaces beginning with an underscore are reserved for jQuery's use.

event namespaces 可以理解成 CSS classes，通过给元素设置 class，我们可以通过 jquery 选择拥有特定 class 的所有元素，然后统一对它们进行操作；通过给 event 添加 event namespaces，我们就可以对特定 event namespaces 下的 event 进行操作，比如 removing or triggering the event。

    <!DOCTYPE html>
    <html>
    <head>
        <title>测试</title>
    </head>
    <body>

    <div id="container" style="padding:10px;background-color: #0b93d5;">
        <button id="test">Test</button>
        <button id="test2">test2</button>
        <a href="#">a1</a>
        <a href="#">a2</a>
    </div>


    <script type="text/javascript" src="/assets/jquery/dist/jquery.min.js"></script>
    <script type="text/javascript">
        let container = $('#container')
        container.on('click.no1', 'button', {}, (e) => {
            // $('#container').on('click.no1',  (e) => {
            console.log('no1');
            console.log(e);
        });

        container.on('click.no2', 'button', {}, (e) => {
            console.log('no2');
            console.log(e);
        });

        let ee = (e) => {
            console.log('no1');
            console.log(e);
            return false;
        };
        container.on('click.no1', 'a', {}, ee);

        //
        container.off('click.no1');
        //
        // container.off('click.no1', 'a');
        // //
        // container.off('click.no1', 'button');
        // //
        // container.off('click.no1', 'a', ee);

    </script>
    </body>
    </html>

# Direct and delegated events
If selector is omitted or is null, the event handler is referred to as *direct* or *directly-bound*. The handler is called every time an event occurs on the selected elements, whether it occurs directly on the element or bubbles from a descendant (inner) element.


When a selector is provided, the event handler is referred to as *delegated*. The handler is not called when the event occurs directly on the bound element, but only for descendants (inner elements) that match the selector. jQuery bubbles the event from the event target up to the element where the handler is attached (i.e., innermost to outermost element) and runs the handler for any elements along that path matching the selector.

上面这两段话，分别定义了 *directly-bound* 和 *delegated*。

In addition to their ability to handle events on descendant elements not yet created, another advantage of delegated events is their potential for much lower overhead when many elements must be monitored. On a data table with 1,000 rows in its tbody, this example attaches a handler to 1,000 elements:

    $( "#dataTable tbody tr" ).on( "click", function() {
      console.log( $( this ).text() );
    });

An event-delegation approach attaches an event handler to only one element, the tbody, and the event only needs to bubble up one level (from the clicked tr to tbody):

    $( "#dataTable tbody" ).on( "click", "tr", function() {
      console.log( $( this ).text() );
    });

这里很不理解的就是，既然说 An event-delegation approach attaches an event handler to only one element，那为什么调试时，event.currentTarget 不是 tbody。

没有为什么，因为这就是 jquery 对 delegated 的处理方式。然后 e 中其实还有一个 delegateTarget 成员变量，指 $( "#dataTable tbody" )。

#The event handler and its environment
When the browser triggers an event or other JavaScript calls jQuery's .trigger() method, jQuery passes the handler an Event object it can use to analyze and change the status of the event. This object is a *normalized subset* of data provided by the browser; the browser's unmodified native event object is available in event.originalEvent. 

jquery 的 event object 并不是 the browser's unmodified native event object。

When jQuery calls a handler, the *this* keyword is a reference to the element where the event is being delivered; for directly bound events this is the element where the event was attached and for delegated events this is an element matching selector. (Note that this may not be equal to event.target if the event has bubbled from a descendant element.) To create a jQuery object from the element so that it can be used with jQuery methods, use *$( this )*.

# .off( events [, selector ] [, handler ] )
    .off( events [, selector ] [, handler ] )
# .one()
    .one( events [, data ], handler )

[1]: https://api.jquery.com/on/ ".on()"