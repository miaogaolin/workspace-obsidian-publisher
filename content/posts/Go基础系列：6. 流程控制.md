---  
title: Go基础系列：6. 流程控制  
date: 2021-07-12T18:18:56+08:00  
share: true  
keywords:  
  - go  
  - 流程控制  
  - for-range  
description: go语言if、for、switch语句使用，break、continue、goto、fallthrough关键字使用  
series:  
  - Go基础系列  
tags:  
  - program_golang  
slug: golang-if-for-switch  
cover:  
  image: https://images.unsplash.com/photo-1520601212700-3a8ae628fab0?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHwyNXx8c3dpdGNofGVufDB8MHx8fDE3MDMzMDE5NzB8MA&ixlib=rb-4.0.3&q=80&w=720  
---  
  
  
  
## 学到什么  
  
1. if 条件语句的用法？  
2. switch 条件语句的用法？  
3. type-switch 用法?  
4. for 循环语句的用法？  
5. fallthrough 、break、continue、goto 用法？  
  
## if 条件语句  
  
### 1. 使用格式  
  
当“条件判断”为 true 时，则进入分支。如下，当第一个 if 的条件判断为 true 时则进入，反之则继续进行 else if 判断，如果还是不为 true, 则最终进入 else 分支。  
  
```go  
if [赋值语句;]条件判断 {  
	// 分支1  
} else if [赋值语句;]条件判断 {  
	// 分支2  
} else {  
	// 分支3  
}  
```  
  
if 语句有如下特点：  
  
- if 后面不需要小括号包裹，后面 switch 和 for 语句也是一样  
- 可以在条件判断前增加赋值语句，用赋值的结果进行条件判断  
  
### 2. 没有赋值语句  
  
省略了”使用格式“中的 `[赋值语句:]` ，if 后只写”条件判断“，这也是其他语言常常使用的格式。  
  
```go  
num := 10  
if num > 12 {  
	fmt.Println("分支1")  
} else if num > 9 {  
	fmt.Println("分支2") // 10 > 9 为 true, 进入此分支  
} else {  
	fmt.Println("分支3")  
}  
```  
  
条件判断不限于我上面的代码，在上篇文章中讲解的”比较运算符“和”非逻辑运算符“都可以参与判断，目的只要是条件判断语句返回 bool 类型就可以。  
  
### 3. 有赋值语句  
  
如果“赋值语句”的结果只在当前 if 语句中使用，那可以使用如下简写方式。  
  
```go  
// 判断函数错误并打印  
if err := fun1(); err != nil {  
	// 程序退出，并打印出错误  
	panic(err)  
}  
```  
  
注：代码中 err 中的作用域只在 if 语句内。  
  
## switch 条件语句  
  
### 1. 使用格式  
  
switch 语句 和 if 语句功能上很相似，基本都可以替代写，除了 `type-switch` 语法，继续往下看会说明。  
  
```go  
switch [var1] {  
	case: var2  
		// todo  
	case: var3,var4  
		// todo  
	default:  
		// todo  
}  
```  
  
switch 语句有以下特点：  
  
- var1 可以是任意类型，也可以是“赋值语句”，也可以省略。  
- case 后可以是变量（数量不限）、也可以是判断语句。  
- switch 进入 case 后，默认只执行当前 case 分支，不用写 break。  
- 如果 case 分支没有一个满足的，最终则执行 default 语句 ，类似 if 语句中的 else 分支。  
- 使用 fallthrough 关键字，执行下一个 case 分支。  
- case 分支也可以为空， 什么都不写。  
  
### 2. 上代码  
  
例 1：比较 num 值  
  
```go  
num := 1  
switch num {  
case 1,2: // 如果 num 的值为 1 或者 2 时进入分支  
	fmt.Println("1或2")  
case 3:  
	fmt.Println("3")  
}  
  
// 输出  
1或2  
```  
  
例 2：fallthrough 使用，即使下一个 case 不满足，也会执行  
  
```go  
num := 1  
switch num {  
case 1,2: // 如果 num 的值为 1 或者 2 时进入分支  
	fmt.Println("1或2")  
	fallthrough  
case 3:  
	fmt.Println("3")  
}  
  
// 输出  
1或2  
3  
```  
  
例 3:  switch 后是赋值语句，单个赋值和并行多个赋值都可以  
  
```go  
// flowcontrol/switch.go  
  
switch num1, num2 := 1, 2; {  
case num1 >= num2:  
	fmt.Println("num1 > num2")  
case num1 < num2:  
	fmt.Println("num1 < num2")  
}  
  
// 输出  
num1 < num2  
```  
  
例 4: 省略 switch 后的语句，如下代码不满足所有 case 分支，进入 default 分支  
  
```go  
num := 3  
switch {  
case num > 5:  
	fmt.Println("num > 5")  
case num > 4:  
	fmt.Println("num > 4")  
case num > 3:  
	fmt.Println("num > 3")  
default:  
	fmt.Println("num <= 3")  
}  
  
// 输出  
num <= 3  
```  
  
例 5：case 分支内容不写  
  
```go  
num := 0  
switch num {  
case 0:  
case 1:  
	fmt.Println(1)  
}  
  
// 进入了 case 0  
// 因为没有内容，所有啥都没有输出  
```  
  
### 3. type-switch  
  
用于获取接口（后面会重点讲解接口）实际类型，对于不同的类型通过分支判断，没明白就看下面代码。  
  
```go  
var data interface{}  
data = "111"  
  
// data 是接口类型， .(type) 获取实际类型  
// 将实际类型的值赋给 d 变量  
switch d := data.(type) {  
case string:  
	// 进入分支后，d 是 string 类型  
	fmt.Println(d + "str")  
case int:  
	// 进入分支后， d 是 int 类型  
	fmt.Println(d + 1)  
}  
  
// 输出  
111str  
```  
  
注：通过 `.(type)` 获取接口的实际类型，记住这种方式只能用于 switch 语句中，这也是我为什么单独在这块讲解。  
  
## for 循环语句  
  
for 循环语句从大的分类讲有两种格式，第一种是“基于计数器迭代”，第二种是“for-range”结构迭代，下来对这两种格式分别讲解。  
  
### 1. 基于计数器迭代  
  
这种也是很多语言常用的格式，如下：  
  
```go  
for [初始化语句];[条件语句];[赋值语句] {  
		...  
}  
  
// 示例：输出 0 - 5  
for i := 0; i < 6; i++ {  
		fmt.Println(i)  
}  
```  
  
for 循环语句有以下特点：  
  
- “[初始化语句]”、"[条件语句]"、"[赋值语句]" 都可以随意省略。  
- ”初始化语句”可以是并行赋值，例： `i, j := 0, 0`  
- ”赋值语句“可以并行赋值，例： `i, j = i + 1, j + 1`  
  
### 2. 语句省略  
  
上面说到“初始化语句”、" 条件语句 "、" 赋值语句 " 都可以省略。那通过不同的省略完成一个简单的例子：”通过循环语句输出 0-5“。  
  
方法 1: 无限循环形式  
  
```go  
i := 0  
for {  
	fmt.Println(i)  
	if i > 4 {  
		// 跳出 for 循环  
		break  
	}  
	i++  
}  
```  
  
方法 2：省略赋值语句  
  
```go  
for i := 0; i < 6; {  
	fmt.Println(i)  
	i++  
}  
```  
  
方法 3：只保留条件语句  
  
```go  
i := 0  
for i < 6 {  
	fmt.Println(i)  
	i++  
}  
```  
  
注：当然不局限以上三种省略，你可以随意省略其中 1 个、2 个、3 个语句。  
  
### 3. for-range  
  
`for-range` 可以迭代任何一个集合（数组、切片、map）、通道，也可以遍历字符串，为了知识点的系统性，把这些类型的格式都列举出来，如果迭代集合和通道没有看懂，后面章节会重点讲解。  
  
**迭代数组或切片**：这两种类型迭代时一样， `i` 为下标索引， `v` 为数组或切片的值，也可以省略 `v` 。  
  
```go  
for i, v := range collection {  
	...  
}  
  
// 省略  
for i := range collection {  
	...  
}  
```  
  
**迭代 map**： `map` 是无序的键值对集合（后面会讲）， `k` 是键名， `v` 是值，也可以省略 `v` 。  
  
```go  
for k, v := range collection {  
	...  
}  
  
// 省略  
for k := range collection {  
	...  
}  
```  
  
**迭代通道**：从通道里获取值，后面会重点讲解， `v` 是值，也可以省略 `v` 。  
  
```go  
for v := range channel {  
	...  
}  
  
// 省略  
for range channel {  
	...  
}  
```  
  
**迭代字符串**：遍历出字符串中的每个字符， `i` 是字符串中字符的位置，从 0 开始， `c` 字符串中每个字符，也可以省略 `c` 。   
  
```go  
for i, c := range str {  
   ...  
}  
  
// 省略  
for i := range str {  
	...  
}  
```  
  
遍历字符串时支持 utf-8 字符，代码如下：  
  
```go  
// flowcontrol/for-range-str.go  
  
str := "我爱china"  
for i, c := range str {  
	fmt.Printf("位置：%d, 字符：%c\n", i, c)  
}  
  
// 输出  
位置：0, 字符：我  
位置：3, 字符：爱  
位置：6, 字符：c  
位置：7, 字符：h  
位置：8, 字符：i  
位置：9, 字符：n  
位置：10, 字符：a  
```  
  
## 几个关键字  
  
下来对 `break` 、 `continue` 、 `goto` 这三个关键字进行讲解。  
  
### 1. break  
  
break 用于 for 语句 和 select 语句（后面会将），表示跳出 for 循环，默认跳出一层循环（不写位置），也可以指定跳出多层循环（写具体跳转位置）, ”位置“的命名随意，只要不和关键字冲突，前后相同。  
  
```go  
[位置]  
...  
break [位置]  
```  
  
示例代码：  
  
```go  
// flowcontrol/break.go  
  
// 默认跳出一层：当 i 为 5 时，跳出循环  
for i := 0; i < 10; i++ {  
	if i == 5 {  
		break  
	}  
	fmt.Println(i)  
}  
  
	// 指定位置跳出：当 j 为 5 时，跳出两层循环  
LABEL:  
	for i := 0; i < 10; i++ {  
		for j := 0; j < 10; j++ {  
			if j == 5 {  
				// 跳到 上面的 LABEL 位置   
				break LABEL  
			}  
			fmt.Println(i)  
		}  
}  
```  
  
### 2. contine  
  
contine 用于 for 循环语句中，表示不执行 continue 之后的逻辑，直接进入下一次循环，如果有多层 for 循环语句时，也可以指定哪个循环，位置的命名随意。  
  
```go  
[位置]  
...  
continue [位置]  
```  
  
示例代码：  
  
```go  
// flowcontrol/continue.go  
  
// 不指定位置，跳过 j 为 1 的内层循环  
for i := 0; i < 3; i++ {  
	for j := 0; j < 3; j++ {  
		if j == 1 {  
			fmt.Printf("%d+%d我没算\n", i, j)  
			continue  
			fmt.Println("没我的份")  
		}  
		fmt.Printf("%d+%d=%d\n", i, j, i+j)  
	}  
}  
  
// 输出  
0+0=0  
0+1我没算  
0+2=2  
1+0=1  
1+1我没算  
1+2=3  
2+0=2  
2+1我没算  
2+2=4  
  
// 指定位置，跳过 i 为 1 的外层循环  
LABEL:  
	for i := 0; i < 3; i++ {  
		for j := 0; j < 3; j++ {  
			if i == 1 {  
				fmt.Printf("%d+%d我没算\n", i, j)  
				continue LABEL  
				fmt.Println("没我的份")  
			}  
			fmt.Printf("%d+%d=%d\n", i, j, i+j)  
  
// 输出  
0+0=0  
0+1=1  
0+2=2  
1+0我没算  
2+0=2  
2+1=3  
2+2=4  
```  
  
### 3. goto  
  
这个关键字其实和”条件语句“、”循环语句“都没有关系，意思是直接跳到指定位置，继续执行逻辑，位置的命名随意。  
  
```go  
// 往回跳  
位置1  
...  
goto 位置1  
  
// 往后跳  
goto 位置2  
...  
位置2  
  
```  
  
示例代码：  
  
```go  
// flowcontrol/goto.go   
  
	 // 往回跳，当 i 不小于 2 时，结束跳转  
   i := 0  
UP:  
   fmt.Println(i)  
   if i < 2 {  
      i++  
      goto UP  
   }  
  
// 输出  
0  
1  
2  
  
   // 往后跳，跳过 goto 与 DOWN: 之间的逻辑  
   fmt.Println("你")  
   goto DOWN  
   fmt.Println("好")  
DOWN:  
   fmt.Println("呀")  
  
// 输出  
你  
呀  
```  
  
## 总结  
  
本篇讲解了 ”if 条件语句“、”switch 条件语句“、”for 循环语句“和关键字”fallthrough 、break、continue、goto“的用法，应该都能看明白。在讲解过程中也牵连了一些后面才要讲的知识点，如果迫切想知道可以自己先去查查（谁让我文章更新慢），或者在下方留言。