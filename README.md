# Sublime下配置X86汇编编译环境
配置教程参考了piratf的[64位Windows环境下通过"Sublime Text 3"搭建汇编语言IDE](http://piratf.ml/2015/05/28/asm-in-windows-x64/)，实际配置环境为`Windows 7 32位`。
## 准备工作
1. 安装[`Sublime Text 3`](http://www.sublimetext.com/3)和[`Package Control`](https://packagecontrol.io/installation)，使用`Package Control`安装`MasmAssembly`  
这一步若有问题请参见[这里](http://piratf.ml/2015/04/28/sublime-text-3-pulgin/)。
2. 安装[`masm32`](http://www.masm32.com/download.htm)  
masm32是我们编译汇编语言的工具。
3. 下载[`DosBox`](http://www.dosbox.com/download.php?main=1)  
DosBox的作用是运行16位程序。  

## 环境变量配置
在Windows变量中分别添加`include`、`lib`、`path`三个变量，我的配置如下：
![](https://raw.githubusercontent.com/vancymoon/Image/master/MASMPATH.png)  
请根据你的`masm32`路径进行修改。
## 16位汇编编译配置
下载[`asm.bat`](https://github.com/vancymoon/sublime-masm32-X86-ASM-build/blob/master/asm.bat)，并存储到`masm32`的`bin`文件夹下。  
在`Sublime Text 3`中，选择`Tools`->`Build System` -> `New Build System…`，将以下内容粘贴进去（覆盖原内容）：

```JSON
{
    "cmd": ["c:\\masm32\\bin\\asm.bat", "$file_base_name"],
    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
    "selector": "source.asm",
    "encoding":"cp936",
    "variants":  
     [   
          {
              "name": "Run", 
              "cmd": ["start","CMD", "/U", "/C","debug $file_base_name.exe"],
              "shell":true
          }
     ]  
}
```
保存文件名为`Asm16`。
## 32位汇编编译配置
在`Sublime Text 3`中，选择`Tools`->`Build System` -> `New Build System…`，将以下内容粘贴进去（覆盖原内容）：
```JSON
{
  "cmd":["ml","/coff","/Cp","/Ic:/masm32/include","$file_name","/link","/subsystem:windows",
                "/libpath:c:/masm32/lib"],
  "selector":"source.asm"
}
```
保存文件名为`Asm32`。


