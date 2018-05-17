# == 与 ===
== operaotr 会在把两个值自动转换成同类型后再比较；而 === operator 在比较前不转换，因为它不仅判断值还对类型进行判断。 

    $a = 2;
    $b = '2';

    echo $a == $b ? 'true' : 'false';   // true
    echo '<br>';   // true
    echo $a === $b ? 'true' : 'false';  // false