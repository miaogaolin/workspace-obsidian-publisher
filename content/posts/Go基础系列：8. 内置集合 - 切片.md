---  
title: Go基础系列：8. 内置集合 - 切片  
date: 2021-07-15T16:18:56+08:00  
share: true  
categories:  
  - Golang  
keywords:  
  - go  
  - 切片  
  - 数组  
description: 一文搞定 Go 语言切片，并对比了数组与切片的区别  
series:  
  - Go基础系列  
slug: golang-slice  
cover:  
  image: https://images.unsplash.com/photo-1607292798226-fee8a80425f6?crop=entropy&cs=tinysrgb&fit=max&fm=webp&ixid=M3wzNjAwOTd8MHwxfHNlYXJjaHw2MHx8c2V0fGVufDB8MHx8fDE3MDMzMDIxNDZ8MA&ixlib=rb-4.0.3&q=80&w=720  
---  
  
  
  
## 学到什么  
  
1. 什么是切片？  
2. 如何创建切片？  
3. 如何获取切片长度和容量？  
4. 切片和数组的关系？  
5. 操作切片具体元素？  
6. 切片元素如何追加和移除？  
7. 切片是引用类型还是值类型？  
8. 如何拷贝切片？  
9. 如何创建多维切片？  
10. 切片字符串是啥？  
  
## 概念  
  
在学习切片之前请先将上篇文章《[内置集合 - 数组]({{< relref "Go%E5%9F%BA%E7%A1%80%E7%B3%BB%E5%88%97%EF%BC%9A7.%20%E5%86%85%E7%BD%AE%E9%9B%86%E5%90%88%20-%20%E6%95%B0%E7%BB%84.md" >}})》搞明白。  
  
切片使用起来类似长度可变的数组，不像数组长度是固定的。但切片的底层使用的还是数组，切片只是保存了对数组的引用，帮着管理数组，实现可变的效果。  
  
## 声明  
  
格式： `var 切片名称 []数据类型`   
  
和数组声明的区别是，是否指明了长度，没有长度则为切片。  
  
```go  
var nums []int  
```  
  
注：切片未初始化默认为 nil ，长度为 0 。如果清空切片可以赋值 nil，例: `nums = nil` 。  
  
## 初始化  
  
### 1. make 函数  
  
使用 make 函数初始化切片，容量（马上有讲）参数可以省略，省略后长度和容量相等，格式如下：  
  
```go  
切片名称 := make([]数据类型，长度，容量)  
```  
  
示例：  
  
```go  
// 长度为 2，容量为 5  
nums := make([]int, 2, 5)  
```  
  
下图是示例代码的原理图，下来深层了解切片。  
  
![20231207161282.webp](/images/20231207161282.webp)  
  
**蓝色区域为切片的结构**，它包含数组的指针（ptr）、切片长度（len）和切片容量（cap）。  
  
- ptr：数组指针，保存数组的内存地址，指向数组的具体索引。  
- len：切片的长度，可以使用 `len(nums)` 函数获取，表示从指针对应的索引位置开始所使用的长度。  
- cap：切片的容量，可以使用 `cap(nums)` 函数获取，表示引用数组的长度。  
  
**黄色区域为切片底层引用的数组**，数组的长度就是切片的容量。  
  
### 2. 初始化具体值  
  
```go  
nums := []int{1, 2, 3}  
```  
  
初始化了一个长度为 3 的切片，此时容量也为 3。  
  
## 操作具体元素  
  
切片中元素的具体操作和数组的方式是一样的。如果获取元素时超出切片长度，即使没有超出容量，编译器也会报错。  
  
```go  
nums := []int{1, 2, 3}  
  
// 设置索引 1 的元素为 4  
nums[1] = 4  
  
fmt.Println(nums[1])  
  
// 输出  
4  
```  
  
## 获取子集  
  
定义了一个切片或数组后，可以获取其中的一部分，即子集。  
  
格式： `切片或数组[开始索引:结束索引]`    
  
获取从“开始索引”到“结束索引”的子集，**包含开始索引，但不包含结束索引**。如果是数组获取子集后，类型会转化为切片类型。  
  
```go  
// 切片  
nums := []int{1, 2, 3, 4, 5}  
// 获取切片子集  
nums1 := nums[2:4]   // []int{3, 4}  
  
// 数组  
arr := [5]int{1, 2, 3, 4, 5}  
// nums2 为切片类型  
nums2 := arr[2:4]    // []int{3, 4}  
```  
  
### 省略索引  
  
“开始索引”和“结束索引”都可以省略。  
  
- 开始索引省略，表示子集从索引 0 开始到结束索引。  
- 结束索引省略，表示子集从开始索引到最后结束。  
- 都省略，如果是切片两者一样，如果是数组会转化为切片类型。  
  
```go  
nums := []int{1, 2, 3, 4, 5}  
  
// 开始索引省略  
nums1 := nums[:3] // []int{1, 2, 3}  
  
// 结束索引省略  
nums2 := nums[2:] // []int{3, 4, 5}  
  
// nums3 和 nums 相同  
// 等价于：nums3 := nums  
nums3 := nums[:]  
```  
  
如果代码中 `nums` 为数组，那 `nums[:]` 会将数组转化为切片。下来看看以上代码的原理图，这样会加深理解。  
  
![20231207161293.webp](/images/20231207161293.webp)  
  
从图中可以看出所有的切片都指向同一个数组，这也说明了**切片是一个引用类型**，它在传递时不会进行拷贝。  
  
## 追加和移除元素  
  
往切片中追加元素，使用到 `append` 函数，此函数只能追加到切片末尾。  
  
```go  
// collection/slice-append.go  
  
nums := []int{1, 2, 3}  
nums = append(nums, 2)  
nums = append(nums, 4, 5)  
  
fmt.Println(nums)  
  
// 输出  
[1 2 3 2 4 5]  
```  
  
如果想追加到切片开头，没有原生的函数，使用 `append` 变向的实现，这种方式其实就是合并两个切片。  
  
```go  
// collection/slice-append-front.go  
  
nums := []int{1, 2, 3}  
// ... 三个点表示将切片元素展开传递给函数  
nums = append([]int{4}, nums...)  
  
fmt.Println(nums)  
  
// 输出  
[4 1 2 3]  
```  
  
如何移除某个元素呢，使用切片子集和 `append` 函数变向实现。  
  
```go  
// collection/slice-append-remove.go  
  
nums := []int{1, 2, 3, 4, 5}  
// 移除索引为 2 的元素值 3  
nums = append(nums[:2], nums[3:]...)  
  
fmt.Println(nums)  
  
// 输出  
[1 2 4 5]  
```  
  
## 切片拷贝  
  
### 1. copy 函数  
  
从上面了解到切片是一个引用类型，因此不能像数组一样直接赋值给一个新变量就会产生拷贝。下来使用 `copy` 函数完成切片的拷贝。  
  
```go  
// collection/slice-copy-1.go  
  
// 将 nums 拷贝到 numsCopy   
nums := []int{1, 2, 3}  
numsCopy := make([]int, 3)  
  
copy(numsCopy, nums)  
  
// 修改了 numsCopy，不会对 nums 产生影响  
numsCopy[0] = 2  
  
fmt.Println("nums:", nums)  
fmt.Println("numsCopy:", numsCopy)  
  
// 输出  
nums: [1 2 3]  
numsCopy: [2 2 3]  
```  
  
`numsCopy` 长度可以小于或大于 `nums` 的长度，如果小于就会拷贝 `nums` 前面一部分，大于会保留 `numsCopy` 后面一部分。  
  
```go  
// collection/slice-copy-2.go  
  
// numsCopy 长度小于 nums  
nums := []int{1, 2, 3}  
numsCopy := make([]int, 2)  
// 前面两个元素 1 和 2 被复制  
copy(numsCopy, nums)  
fmt.Println("numsCopy(小于):", numsCopy)  
  
// numsCopy 长度大于 nums  
numsCopy = []int{4, 5, 6, 7}  
// 4,5,6 会被覆盖，保留 7  
copy(numsCopy, nums)  
fmt.Println("numsCopy(大于):", numsCopy)  
  
// 输出  
numsCopy(小于): [1 2]  
numsCopy(大于): [1 2 3 7]  
```  
  
### 2. “长度 > 容量”触发拷贝  
  
使用 `append` 函数给切片追加元素时，如果追加的长度大于切片的容量时，切片的底层数组空间则重新开辟一块比原来大的地方，并把原来的数组值拷贝一份。  
  
![20231207161299.webp](/images/20231207161299.webp)  
  
注解：  
  
- 图中”新数组“两个位置就是切片长度大于容量的时刻，这两个时刻会自动开辟新数组，与原来的数组没有任何关联，只是把值拷贝了一份。  
- 图中创建”新数组“时，容量的大小是原来的 2 倍，但这不是一成不变的，不同情况算法也会不一样，想要了解清楚我推荐一篇文章《[深度解密Go语言之Slice](https://mp.weixin.qq.com/s/MTZ0C9zYsNrb8wyIm2D8BA)》。  
  
## 多维切片  
  
这块和多维数组是类似的，唯一的不同点是切片没有指明长度，举个例子：  
  
```go  
// 声明二维切片  
var mult [][]int  
  
// 初始化二维切片  
students := [][]int{  
	{2, 2, 0},  
	{2, 2, 2},  
	{2, 1, 2},  
	{2, 2, 2},  
}  
```  
  
注：如果想创建三维切片、四维切片，只要和多维数组类比就行。  
  
## 切片字符串  
  
这个是啥呢？是字符串可以使用上面的子集用法，来获取字符串中的一部分。  
  
```go  
str := "I'm laomiao."  
fmt.Println(str[4:7])  
  
// 输出  
lao  
```  
  
## 总结  
  
本篇围绕”切片“进行了重点讲解，在实际开发中也常常被使用，所以一定要掌握清楚。文中没有写明如何遍历切片，是因为它和数组的使用是一样的，如果不懂，请翻阅上篇文章。  
