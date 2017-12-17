# jQuery.post
jQuery.post(url,data,success(data, textStatus, jqXHR),dataType)

success
Type: Function( PlainObject data, String textStatus, jqXHR jqXHR )
A callback function that is executed if the request succeeds. Required if dataType is provided, but can be null in that case.

If a request with jQuery.post() returns an error code, it will fail silently unless the script has also called the global .ajaxError() method. Alternatively, as of jQuery 1.5, the .error() method of the jqXHR object returned by jQuery.post() is also available for error handling.

success 回调函数只会在 request 成功的时候被调用（即，response 中的 status 为 200 系列）。如果要在调用 jQuery.post 时，处理 status 为其他系列的 response，有两种方式：

1. 使用 .ajaxError() 设置一个全局的回调函数
2. 通过 .post() 返回的 jqXHR 对象设置 fail 回调函数。
    
        // Assign handlers immediately after making the request,
        // and remember the jqxhr object for this request
        var jqxhr = $.post( "example.php", function() {
        alert( "success" );
        })
        .done(function() {
          alert( "second success" );
        })
        .fail(function() {
          alert( "error" );
        })
        .always(function() {
          alert( "finished" );
        });

## 总结
关于 jQuery.post 需要注意如下点：

1. success 回调函数在什么情况下会被调用
2. jQuery.post 的返回值为一个 jqXHR 对象，可以通过该对象设置处理其他情况的回调函数。

[1]: http://api.jquery.com/jQuery.post/ "jQuery.post()"