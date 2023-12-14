---  
date: 2016-06-03 23:54:14+08:00  
tags:   
title: 跟我学习php数组常用函数-上篇  
slug: php-func-up  
share: true  
canonicalURL: https://www.jianshu.com/p/9a7d2d784549  
keywords:   
description:   
series:   
---  
  
对于 php 的初学者,也许会对它大量的函数不清楚该学习哪些。我在这列举了一些大家实际当中可能会使用到的,供您参考  
  
  
***  
  
1. `array_map  ( callable  $callback  , array $arr1  [, array $...  ] ) `  
  
> callback(回调函数),接受一个函数  
> 函数作用: 将 arr1 的参数顺序的传递给前面的回调函数  
  
* 回调函数有参时:  
  
```php  
/*例1*/  
  
<?php  
 function  cube ( $n )  
{  
    return( $n  *  $n  *  $n );  
}  
 $a  = array( 1 ,  2 ,  3 ,  4 ,  5 );  
 $b  =  array_map ( "cube" ,  $a );  
 print_r ( $b );  
/**  
 输出  
 Array  
 (  
     [0] => 1  
     [1] => 8  
     [2] => 27  
     [3] => 64  
     [4] => 125  
 )  
**/  
 ?>   
```  
* 回调函数无参时:  
  
```php  
<?php  
/*例2*/  
/*将多个数组进行合并*/  
$a  = array( 1 ,  2 ,  3 ,  4 ,  5 );  
 $b  = array( "one" ,  "two" ,  "three" ,  "four" ,  "five" );  
 $c  = array( "uno" ,  "dos" ,  "tres" ,  "cuatro" ,  "cinco" );  
 $d  =  array_map ( null ,  $a ,  $b ,  $c );  
 print_r ( $d );  
/*输出  
Array  
(  
    [0] => Array  
        (  
            [0] => 1  
            [1] => one  
            [2] => uno  
        )  
    [1] => Array  
        (  
            [0] => 2  
            [1] => two  
            [2] => dos  
        )  
    [2] => Array  
        (  
            [0] => 3  
            [1] => three  
            [2] => tres  
        )  
    [3] => Array  
        (  
            [0] => 4  
            [1] => four  
            [2] => cuatro  
        )  
    [4] => Array  
        (  
            [0] => 5  
            [1] => five  
            [2] => cinco  
        )  
)  
*/  
 ?>   
```  
***  
2.  `range ( mixed $start , mixed $limit [, number $step = 1 ] )`  
  
**step 表示间隔值，不写默认为 1**  
  
```php  
/*  
例1，产生一组数字  
*/  
$nums = range(1, 5);  
print_r($nums);  
/*  
输出:  
Array ( [0] => 1 [1] => 2 [2] => 3 [3] => 4 [4] => 5 )  
*/  
$nums = range(1, 5, 2);  
print_r($nums);  
/*  
输出:  
Array ( [0] => 1 [1] => 3 [2] => 5 )  
*/  
```  
  
```php  
/*  
例2，产生一组字母数组  
*/  
$array = range('a','f');  
print_r($array);  
/*  
输出:  
Array ( [0] => a [1] => b [2] => c [3] => d [4] => e [5] => f )  
*/  
$array = array('a', 'f', 2);  
print_r($array);  
/*  
输出:  
print_r($array);  
/*  
输出:  
Array ( [0] => a [1] => c [2] => e )  
*/  
```  
  
  
  
***  
  
  
  
3. `array_merge ( array $array1 [, array $... ] )`  
  
  
  
> array_merge() 将一个或多个数组的单元合并起来，一个数组中的值附加在前一个数组的后面。返回作为结果的数组。  
  
  
  
> 如果输入的数组中有相同的字符串键名，则该键名后面的值将覆盖前一个值。然而，如果数组包含数字键名，后面的值将不会覆盖原来的值，而是附加到后面。  
  
  
  
> 如果只给了一个数组并且该数组是数字索引的，则键名会以连续方式重新索引。  
  
  
  
```php  
/*  
解释:如果只给了一个数组并且该数组是数字索引的，则键名会以连续方式重新索引。  
*/  
$array1 = array(1, 2, 3, 4, 5);  
$array2 = array(1, 2, 8, 9);  
$array3 = array_merge($array1, $array2);  
print_r($array3);  
/*  
输出:  
Array  
(  
    [0] => 1  
    [1] => 2  
    [2] => 3  
    [3] => 4  
    [4] => 5  
    [5] => 1  
    [6] => 2  
    [7] => 8  
    [8] => 9  
)  
*/  
```  
  
  
  
***  
  
  
  
4.  `array_merge_recursive ( array $array1 [, array $... ] )` **递归地合并一个或多个数组**  
  
  
  
> 如果输入的数组中有相同的字符串键名，则这些值会被合并到一个数组中去，这将递归下去，因此如果一个值本身是一个数组，本函数将按照相应的条目把它合并为另一个数组。然而，如果数组具有相同的数组键名，后一个值将不会覆盖原来的值，而是附加到后面。  
  
  
  
> 会根据键名相同一层一层的将值进行合并  
  
  
  
```php  
/*  
例1  
*/  
$ar1 = array("color" => array("favorite" => "red"), 5);  
$ar2 = array(10, "color" => array("favorite" => array('a'=>"red"), "blue"));  
$result = array_merge_recursive($ar1, $ar2);  
print_r($result);  
/*  
输出:  
Array  
(  
    [color] => Array  
        (  
            [favorite] => Array  
                (  
                    [0] => red  
                    [a] => red     重点  
                )  
            [0] => blue  
        )  
    [0] => 5  
    [1] => 10  
)  
*/  
```  
  
  
  
```php  
/*  
例2  
*/  
$ar1 = array("color" => array("favorite" => "red"), 5);  
$ar2 = array(10, "color" => array("favorite" =>"red", "blue"));  
$result = array_merge_recursive($ar1, $ar2);  
print_r($result);  
/*  
输出:  
Array  
(  
    [color] => Array  
        (  
            [favorite] => Array  
                (  
                    [0] => red  
                    [1] => red  重点  
                )  
            [0] => blue  
        )  
    [0] => 5  
    [1] => 10  
)  
*/  
```  
  
  
  
***  
  
  
  
5. `array_pad ( array $input , int $pad_size , mixed $pad_value )` **给数组增加值到指定的长度,原数组不会改变**  
  
  
  
```php  
<?php  
$input = array(12, 10, 9);  
$result = array_pad($input, 5, 0);  
// result is array(12, 10, 9, 0, 0)  
$result = array_pad($input, -7, -1);  
// result is array(-1, -1, -1, -1, 12, 10, 9)  
$result = array_pad($input, 2, "noop");  
// not padded  
/*  
如果size<数组的长度，将不会有变化  
*/  
?>  
  
```  
  
***  
  
  
  
6. `array_pop ( array &$array )` **移出最后一个元素，原数组会改变**  
  
  
  
> array_pop() 弹出并返回 array 数组的最后一个单元，并将数组 array 的长度减一。 **如果 array 为空（或者不是数组）将返回 NULL** 。 此外如果被调用不是一个数则会产生一个 Warning。  
  
  
```php  
<?php  
$stack = array("orange", "banana", "apple", "raspberry");  
$fruit = array_pop($stack);  
print_r($stack);  
?>  
/*  
输出  
Array  
(  
    [0] => orange  
    [1] => banana  
    [2] => apple  
)  
*/  
```  
  
  
  
***  
  
  
  
  
  
  
  
7. `array_shift()` **将数组开头的单元移出数组,原数组会改变, 使用此函数后会重置（reset()）array 指针。**  
  
  
  
> array_shift() 将 array 的第一个单元移出并作为结果返回，将 array 的长度减一并将所有其它单元向前移动一位。所有的数字键名将改为从零开始计数，文字键名将不变。 **如果 array 为空（或者不是数组）将返回 NULL**   
  
  
  
8. `int array_push ( array &$array , mixed $var [, mixed $... ] )`  
  
*  array_push() 将 array 当成一个栈，并将传入的变量压入 array 的末尾。array 的长度将根据入栈变量的数目增加  
* 返回处理后数组的元素个数  
  
  
9)  reset ( array &$array )**将数组的内部指针指向第一个单元**  
* reset() 将 array 的内部指针倒回到第一个单元并返回第一个数组单元的值。   
* 数组为空返回 false  
  
  
10)  end ( array &$array )  
> 参数 array,该数组是通过**引用 (&)**传递的，因为它会被这个函数修改。 这意味着你必须传入一个**真正的变量**，而**不是函数返回的数组**，因为只有真正的变量才能以引用传递。   
  
* end() 将 array 的内部指针移动到最后一个单元并返回其值。  
* 数组为空返回 false  
  
```php  
<?php  
funcation back(){  
    $arr = array(1, 2, 3);  
    return $arr;  
}  
/*上面引用传递的意思请注意看*/  
$num = end(back()); //错误！,因为该参数是引用传递  
  
$array = back();  
$num = end($array); //正确  
```  
  
> 对于引用传递,如果不将一个函数的返回值赋给一个变量的话,在内存中是不会真正的开辟以个单独的空间内存的。而引用传递的参数则必须需要一个有真实存在的内存的,因此,$array = back(),这样会开辟一片内存给$array 变量  
  
[下篇](http://www.jianshu.com/p/82659d6c53e7)