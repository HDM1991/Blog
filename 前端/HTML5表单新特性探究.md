# 前言
在 HTML5 之前，当页面需要用户在一个表单控件中输入 email 地址、数字或者日期时，我们只能写下如下的代码：

    <input type="text" name="email">

然后，如果是我们不在上面代码的基础上添加一些额外功能代码的情况下，我们是无法验证用户是否输入了数据、用户输入的数据是否符合格式、当用户没有输入数据或者输入的数据不符合格式时，对用户做出提示的；而且我们也无法给用户提供一个很好的操作 UI，比如我们想让用户输入一个日期，但我们能提供给用户的操作界面只能是这样的:
![这里写图片描述](http://img.blog.csdn.net/20171016161433627?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQ4MjYxOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
如果我们想给用户提供这样一个操作 UI：
![这里写图片描述](http://img.blog.csdn.net/20171016161607787?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzQ4MjYxOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
那我们只能再次写很多很多代码实现这个功能。


可以说，这些都是我们在使用表单控件时的基本需求。那为什么这些基本需求就不能有一个通用的标准，并且有浏览器原生支持呢？其实，标准已经有了，就是 HTML5 新添加的关于表单的一些东西；而且对于这个标准，现在的浏览器基本都提供了很好支持。

简单来说，HTML5 提供的这些新的表单支持，就是为了提供更好的输入控制和验证。通俗讲，就是上面我们提到的那些，判断数据是否已经输入、验证数据是否符合格式、在用户没有输入数据或者输入的数据不符合格式时，对用户做出提示、给用户提供一个更好的操作 UI。

# HTML5 表单新特性
HTML5 从三个方面来解决上述问题。

1. 提供新的输入类型
2. 提供新的表单元素
3. 提供新的表单属性

目前，主要还是 1、3 方面；2 方面提供的新的表单元素就 3 个，然后目前 chrome 61.0.3163.100 也就支持一个，可有可无，暂不考虑。

## 新的输入类型
新的输入类型，具体如下：

* email
* url
* number
* range
* Date pickers (date, month, week, time, datetime, datetime-local)
* search
* color

看名字就很容易理解，这些输入类型就用于上面我们提到的要求用户输入 email、数字的这类情况。新的输入类型一方面会在用户输入完成后，检查用户输入的数据是否符合格式，并在数据不符合格式的时候对用户进行提示，比如 email 类型；另一方就是给用户提供更好的操作UI，比如 Date pickers 类型。

##新的表单属性
新的表单属性也有很多，这里不在一一列举，但可以重点关注和使用如下属性:

1. required
2. pattern
3. placeholder
4. novalidate

required 属性要求指定了该属性的表单必须输入值；在值为空的情况下，会对用户进行提示。

pattern 属性规定用于验证 input 域的模式（pattern）。模式（pattern） 是正则表达式。使用该属性可以进一步对用户输入的数据的格式化进行定制。

placeholder 属性提供一种提示（hint），描述输入域所期待的值。提示（hint）会在输入域为空时显示出现，会在输入域获得焦点时消失。

novalidate 属性规定在提交表单时不应该验证 form 或 input 域。

# 其他
## An invalid form control with name='ssdfsad' is not focusable
然后这里讲一下遇到的一个小问题，在一项工作任务中，我给页面中的某个 input 控件指定了 required 属性，这个 input  和 submit 按钮分别属于不同的上级 div；并且在 submit 按钮所属的 div 显示时，input 所属的 div 会隐藏；然后在给这个 input 控件不赋值的情况下，点击 submit 按钮，chrome 61.0.3163.100 会在开发者工具的 console 下输出如下 log：

> An invalid form control with name='ssdfsad' is not focusable.

这个 log 信息可以这样理解，就是在这种情况下，浏览器本该显示一个提示信息，提示用户该 input 需要填写一个值，但是由于这个 input 现在没有显示，就无法将焦点定位到这个 input，然后显示提示信息。

虽然在这种情况下浏览器会输出这个 log 信息且无法显示提示信息，但依然会终止表单的提交。但这样就很不友好，会让用户一头雾水。

# 总结-如何适当地使用这些新特性
综上所述，虽然 HTML5 目前提供的这些表单新特性确实很方便，但如何使用、使用哪些，还是要根据实际需求而定。比如在上面提到的我的工作任务中，对于 input 数据的验证就不能使用 HTML5 提供的这些新特性，还是需要自己完成。然后在另外一些任务中，考虑到页面兼容性的问题，可能也无法使用这些新特性；或者至少提供一个备用方案。根据用户浏览器的情况，使用合适的方案。