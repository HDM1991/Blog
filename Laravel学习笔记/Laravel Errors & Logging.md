Laravel 给我们提供了统一处理异常的机制。

All exceptions are handled by the App\Exceptions\Handler class.

The report method is used to log exceptions or send them to an external service like Bugsnag or Sentry. By default, the report method simply passes the exception to the base class where the exception is logged.

The render method is responsible for converting a given exception into an HTTP response that should be sent back to the browser. By default, the exception is passed to the base class which generates a response for you. However, you are free to check the exception type or return your own custom response.

# Reportable & Renderable Exceptions
Instead of type-checking exceptions in the exception handler's report and render methods, you may define report and render methods directly on your custom exception. When these methods exist, they will be called automatically by the framework

上面这段话的意思是，当我们自定义了一个异常类时，没有必要在 App\Exceptions\Handler 的 report and render methods 中添加对这个异常的处理代码，只要在这个定义这个异常类时，定义 report and render methods，然后将相应的代码放到其中就可以了。

# HTTP Exceptions
The abort helper will immediately raise an exception which will be rendered by the exception handler.

这个很有用。

# Custom HTTP Error Pages
也有用

[1]: https://laravel.com/docs/5.5/errors#the-exception-handler "Errors & Logging"

