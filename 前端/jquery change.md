This event is limited to <input> elements, <textarea> boxes and <select> elements.

For select boxes, checkboxes, and radio buttons, the event is fired immediately when the user makes a selection with the mouse, but for the other element types the event is deferred until the element loses focus.

NOTE: 有一个混淆点，下面的代码：

    <form>
    <input name="sex" type="radio" value="male" checked>男
    <input name="sex" type="radio" value="female">女
    </form>

    <script type="text/javascript">
      $("[name='sex']").change(function() {
        console.log( $(this).val() );
      });

      // 这里我以为只会为选中的 input 元素触发 change 事件，然而并不是，
      // 两个 input 都触发了 change 事件
      $("[name='sex']").change();
    </script>
