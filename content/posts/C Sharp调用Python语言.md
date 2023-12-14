---  
title: C#调用Python语言  
date: 2015-10-23 20:57:06  
tags:   
canonicalURL: https://blog.csdn.net/sinat_32124195/article/details/49366131  
share: true  
slug: csharp-python  
---  
  
本次实验是 vs2010 版本以下（包括 vs2010）,我记得如果是高版本的话好像已经内置了，所以比较简单。  
  
1.去官网下载 IronPython,链接 http://ironpython.codeplex.com/，它是 IronPython 是一种在 NET 和 Mono 上实现的 Python 语言  
2.打开 vs,添加两个引用，在 IronPython 的安装根目录下面选择 IronPython.dll 和 Microsoft.Scripting.dll  
```C#  
using Microsoft.Scripting.Hosting;  
using IronPython.Hosting;  
//前面要导入两个名称空间  
 private void button1_Click(object sender, EventArgs e)  
        {  
            ScriptEngine pyEngine = Python.CreateEngine();  //建立python引擎  
            pyEngine.CreateScriptSourceFromFile("demo1.py").Execute();//执行.py脚本   
        }  
```  
3.上面这个 "demo1.py" 是你的 Python 脚本文件，如果想通过上面两句执行.py 文件必须，前提是你还要导入别的模块，就**必须加入**在最前面写入以下代码,还有要设置脚本属性中（vs 中右键）“复制到输出目录中”这一项，选择始终复制（所有脚本一样）  
```C#  
import sys  
sys.path.appedn("C:\IronPython 2.7\Lib")#看自己的Lib路径  
  
4.调用python的方法//配置python的环境,另写一个脚本demo2.py  
ScriptRuntime pyRuntime = Python.CreateRuntime();  
dynamic obj = pyRuntime.UseFile("demo2.py");  
var a = obj.ShutDown(1800);       //调用脚本的ShutDown方法，1800时间单位为秒  
if (a == 1)  
{  
   Console.WriteLine("倒计时设置成功");  
}  
```  
  
下面是我调用的 python 文件 "demo2.py"  
```C#  
import sys  
sys.path.append("C:\IronPython 2.7\Lib")  
import os  
def ShutDown(delayTime):  
os.system('shutdown -s -t %d'%(delayTime))  
return 1  
```  
  
  
然后成功！  
