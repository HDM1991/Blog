# 自定义错误显示

有如下几个参数

errorClass              String      指定错误提示的 css 类名，可以自定义错误提示的样式。    "error"
errorElement            String      用什么标签标记错误，默认是 label，可以改成 em。    "label"
errorContainer          Selector    显示或者隐藏验证信息，可以自动实现有错误信息出现时把容器属性变为显示，无错误时隐藏，用处不大。errorContainer: "#messageBox1, #messageBox2"    
errorLabelContainer     Selector    把错误信息统一放在一个容器里面。    
wrapper                 String      用什么标签再把上边的 errorELement 包起来。    


每个错误信息会被 errorElement 设置的标签包括，并且标签的 class 为 errorClass 设置的类。假如
errorElement 设置的标签是 em，errorClass 设置的类是 h_error，则每一条错误信息是像下面这样的

    <em id="user-error" class="h_error" style="display: inline;"> (不能少于 3 个字母)</em>

wrapper 怎么理解，设置 wrapper 后，会用 wrapper 设置的标签把上面错误信息在包括一次，接上面的示例，
假如 wrapper 的值为 div，则每一条错误信息是像下面这样的

    <div><em id="user-error" class="h_error" style="display: inline;"> (不能少于 3 个字母)</em></div>


设置 errorLabelContainer 后，会把所有错误信息都放到errorLabelContainer 设置的容器中，
假如 errorContainer: $("#error")，则所有错误信息是像下面这样的：

    <div id="error">
        <div><em id="user-error" class="h_error" style="display: inline;"> (不能少于 3 个字母)</em>
        </div><div><em id="password-error" class="h_error" style="display: inline;"> (字母不能少于 5 个且不能大于 12 个)</em></div>
    </div>


errorContainer，这样理解，不管给他设置一个啥Selector，这个Selector就会在有错误信息的时候是显示，没错误信息的时候不显示。
一般情况下，还是要结合上面提到的几个参数用。