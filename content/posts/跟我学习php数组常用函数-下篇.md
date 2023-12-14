---  
date: 2016-06-05 00:05:03+08:00  
tags:   
title: 跟我学习php数组常用函数-下篇  
slug: php-func-down  
share: true  
canonicalURL: https://www.jianshu.com/p/82659d6c53e7  
keywords:   
description:   
series:   
---  
  
1. `mixed array_rand ( array $input [, int $num_req = 1 ] )`  
  
> 从 input 所给的数组中随机 选取一个或多个键  
  
*  num_req，指明了你想取出多少个单元。如果指定的数目超过了数组里的数量将会产生一个 E_WARNING 级别的错误。  
* 返回值,如果你只取出一个，array_rand() 返回一个随机单元的键名，否则就返回一个包含随机键名的数组。这样你就可以随机从数组中取出键名和值。   
  
  
  
  
2.  `array array_replace ( array $array1 , array $array2 [, array $... ] )`  
  
>  array_replace() 函数使用后面数组元素相同 key 的值替换 array1 数组的值。  
  
* 如果一个键存在于第一个数组同时也存在于第二个数组，它的值将被第二个数组中的值替换。  
```php  
$arr1 = array('hobby' => 'basketball');  
$arr2 = array('hobby' => 'football');  
$arr = array_replace($arr1, $arr2);  
//$arr结果,array('hobby' => 'football');  
```  
* 如果一个键存在于第二个数组，但是不存在于第一个数组，则会在第一个数组中创建这个元素。  
```php  
$arr1 = array('hobby' => 'basketball');  
$arr2 = array('sex' => 'male');  
$arr = array_replace($arr1, $arr2);  
//$arr结果,array('hobby' => 'basketball', 'sex' => 'male');  
```  
* 如果一个键仅存在于第一个数组，它将保持不变。  
* 如果传递了多个替换数组，它们将被按顺序依次处理，后面的数组将覆盖之前的值。  
* 是**非递归的**：它将第一个数组的值进行替换而不管第二个数组中是什么类型。  
```php  
$arr1 = array('hobby' => array('a' => 'football', 'b' => 'basketball'));  
$arr2 = array('hobby' => array('a' = > 'ping-pong'));  
$arr = array_replace($arr1, $arr2);  
//结果:array('hobby' => array('a' = > 'ping-pong'));  
```  
**如果是递归的,结果:array('hobby' => array('a' => 'ping-pong', 'b' => 'basketball'));**  
  
  
3. `array array_reverse ( array $array [, bool $preserve_keys = false ]`   
> 将数组中的内容倒转  
  
* preserve_keys: 如果设置为 TRUE 会保留数字的键。 非数字的键则不受这个设置的影响，总是会被保留。  
  
* 解释:preserve_keys  
  
```php  
<?php  
  
$array = array('a'=>'a',1,2);  
/*  
结果:  
Array  
(  
    [a] => a  
    [0] => 1  
    [1] => 2  
)  
*/  
```  
* preserve_keys = false(默认)  
  
```php  
$revArray = array_reverse($araay);  
print_r($revArray);  
/*  
结果:  
Array  
(  
    [0] => 2  
    [1] => 1  
    [a] => a  
)  
*/  
```  
*  preserve_keys = true  
  
```php  
$revArray = array_reverse($araay, true);  
print_r($revArray);  
/*  
结果:  
Array  
(  
    [1] => 2  
    [0] => 1  
    [a] => a  
)  
*/  
```  
  
4. `number array_sum ( array $array )`  
  
> 计算,参数数组值的总和  
* 如果值是不包含数字不参与运算  
* 即使值是字符串,也将会字符串开始为数字的字符参与运算  
```php  
$arr = array(1, 2, 3, '4a', 2);  
$sum = array_sum($arr);//结果: 12  
```  
  
5. `bool sort ( array &$array [, int $sort_flags = SORT_REGULAR ] )`  
  
> 将值排序完成后，会是一个索引数组,即便原来是一个关联数组  
  
* $array,需要排序的数组  
* sort_flags(可选),以何种方式排序  
   * SORT_NUMERIC,按照数字的从小到达排序  
   * SORT_NATURAL,自然排序  
   * [更多](http://php.net/manual/zh/function.sort.php)   
  
* 如果 sort_flags 胜率,会按照 ascii 从小到达排序   
  
```php  
<?php  
$fruits = array("lemon", "orange", "banana", "apple");  
sort($arr);  
//$arr: Array ( [0] => apple [1] => banana [2] => lemon [3] => orange )   
```  
  
 * sort_flags = SORT_NUMERIC  
  
```php  
$num = array('2', '11', '1', '21');  
sort($num, SORT_NUMERIC);  
//$num: Array ( [0] => 1 [1] => 2 [2] => 11 [3] => 21 )  
```  
  
* sort_flags = SORT_STRING  
> 假设实现以下排序  
> 1---- 第一章  
> 11--- 第一章第一节  
> 2---- 第二章  
> 21--- 第二章第一节  
  
```php  
$num = array('2', '11', '1', '21');  
sort($num, SORT_STRING);  
//$num: Array ( [0] => 1 [1] => 11 [2] => 2 [3] => 21 )  
```  
  
6. `bool in_array ( mixed $needle , array $haystack [, bool $strict = FALSE ] )`  
> 判断指定值是否在一个数组里  
  
* $needle, 指定的值  
* hanystack, 要搜索的数组  
* $trict, true 区分大小写  
* 返回,存在 true,不存在 false  
  
7. `string join ( string $glue , array $pieces)`  
> 别名 [implode()](http://php.net/manual/zh/function.implode.php)  
> 将一个数组转化字符串  
  
* $glue,要连接数组值的字符串  
* $pieces,需要转化的数组  
> 如果是关联数组键是不会保留的  
  
```php  
<?php  
$arr = array('a'=>1, 'b'=>2);  
echo join(',', $arr);  
// 输出: 1,2  
```  
