# .on( events [, selector ] [, data ], handler )

selector
Type: String
A selector string to filter the descendants of the selected elements that trigger the event. If the selector is null or omitted, the event is always triggered when it reaches the selected element.

selector 这个参数，刚开始我的理解是，如果设置了，还是为当前元素绑定事件处理函数，只是只有这个事件是从 selector 指定的子元素中触发时，才会执行事件处理函数；但并不是这样，以下面这个实例为例，
    
    $('#container').on('click', 'button', {}, (e) => {
    // $('#container').on('click',  (e) => {
        console.log(e);
    });

等价于

        $('#container button').click((e) => {
            console.log(e);
        });

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
        $('#container').on('click.no1', 'button', {}, (e) => {
        // $('#container').on('click.no1',  (e) => {
            console.log('no1: ' + e);
        });

        $('#container').on('click.no2', 'button', {}, (e) => {
            console.log('no2: ' + e);
        });

        let ee = (e) => {
            console.log('no1: ' + e);
            return false;
        };
        $('#container').on('click.no1', 'a', {}, ee);

        // 
        $('#container').off('click.no1');
        //
        $('#container').off('click.no1', 'a');
        //
        $('#container').off('click.no1', 'button');
        //
        $('#container').off('click.no1', 'a', ee);
       
    </script>
    </body>
    </html>

# .off( events [, selector ] [, handler ] )

[1]: https://api.jquery.com/on/ ".on()"