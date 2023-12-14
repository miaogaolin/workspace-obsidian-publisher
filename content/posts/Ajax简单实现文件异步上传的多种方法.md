---  
date: 2016-05-31 19:33:05+08:00  
tags:   
title: Ajax简单实现文件异步上传的多种方法  
slug: ajax-upload  
share: true  
canonicalURL: https://www.jianshu.com/p/d90d2e6bb0d5  
keywords:   
description:   
series:   
---  
  
## 1. 认识 FormData 对象  
FormData 是 Html5 新加进来的一个类,可以模拟表单数据  
  
  
构造函数 |解释  
---|---  
FormData (optional HTMLFormElement form) | (可选) 一个 HTML 表单元素,可以包含任何形式的表单控件,包括文件输入框.  
  
### 方法  
  
void append(DOMString name, DOMString value)  
  
* name 表单元素名称  
* value 表单元素要传递的值  
  
```html  
<form name="myForm"  enctype="multipart/form-data">  
    <input type="text" name="userName">  
    <input type="file" name="img">  
    <input type="button" id="btn" value="submit">  
</form>  
  
```  
## 2. 使用 javascript 简单实现  
```javascript  
function upload() {  
    var userName = document.myForm.userName.value;  
    var img = document.myForm.img.files[0];  
    var fm = new FormData();  
    fm.append('userName', userName);  
    fm.append('img', img);  
  
    var request = new XMLHttpRequest();  
    request.open('POST', 'submitform.php');  
    request.send(fm);  
}  
```  
## 3.  使用 Ajax 实现  
  
  
```javascript  
$('#btn').click(function () {  
    var userName = document.myForm.userName.value;  
    var img = document.myForm.img.files[0];  
      
    var fm = new FormData();  
    fm.append('userName', userName);  
    fm.append('img', img);  
    $.ajax(  
        {  
            url: 'submitform.php',  
            type: 'POST',  
            data: fm,  
            contentType: false, //禁止设置请求类型  
            processData: false, //禁止jquery对DAta数据的处理,默认会处理  
            //禁止的原因是,FormData已经帮我们做了处理  
            success: function (result) {  
                //测试是否成功  
                //但需要你后端有返回值  
                alert(result);  
            }  
        }  
    );  
});  
```  
  
## 4. ajaxfileupload.js 插件实现 Ajax 文件上传  
```javascript  
function upload(){  
$.ajaxFileUpload({  
        url: 'a.php', //用于文件上传的服务器端请求地址  
        secureuri: false, //一般设置为false  
        fileElementId: 'file', //文件上传空间的id属性    
        dataType: 'HTML', //返回值类型 一般设置为json  
        success: function (data, status)  //服务器成功响应处理函数  
        {                  
        	$("#img1").attr("src", data);  
        	addI(data);  
        },  
        error: function (data, status, e)//服务器响应失败处理函数  
        {  
            alert(e);  
        }  
    }     
);  
}   
```  
对于 PHP 就可以使用 Files 全局数组拿到文件属性,POST 全局数组拿到 userName 的值  
  
