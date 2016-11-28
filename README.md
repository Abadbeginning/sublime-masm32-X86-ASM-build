# Sublime下使用masm32配置X86汇编编译环境
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
## `DosBox`挂载设置
由于16位程序只能在DosBox的虚拟环境中运行，因此我们要对DosBox进行一些设置。
首先，新建一个文件夹，如我在C盘下新建了一个DEBUG文件夹（当然，你也可以根据自己的需要在任意盘符下新建一个文件夹，文件夹名随意），里面存放你的16位汇编程序以及一些DosBox工具，我的DEBUG下有[这些工具](https://github.com/vancymoon/sublime-masm32-X86-ASM-build/blob/master/DEBUG.zip)。  
双击打开DosBox文件夹下的`DOSBox 0.74 Options`，在末尾加入：

```
mount C C:\DEBUG
C:
```
其中，`C:\DEBUG`改为你自己新建的文件夹的路径。
## 16位汇编编译及运行
到这里，我们的配置工作就基本完成了。现在，我们尝试来对16位汇编程序以及32位汇编程序进行编译与运行。
首先，在`Sublime Text 3`中新建空白文件，写入以下内容：

```ASM
data segment
abc db "hello, world!", 0Dh, 0Ah, "$"
data ends
code segment
assume cs:code, ds:data
main:
   mov ax, data
   mov ds, ax
   mov ah, 9
   mov dx, offset abc
   int 21h
   mov ah, 4Ch
   int 21h
code ends
end main
```
保存为`hello.asm`到`C:\DEBUG`中（或你自己新建的文件夹的路径中）。然后在`Sublime Text 3`中，选择`Tools` -> `Build System` -> `Asm16`，再使用快捷键`ctrl`+`shift`+`B`，上下键选择`Asm16`回车，即可完成编译。
然后打开Dosbox，直接输入hello即可运行16位程序。
## 32位汇编编译及运行
在`Sublime Text 3`中新建空白文件，写入以下内容：

```ASM
.386
.model flat, stdcall
option casemap :none

include include\windows.inc
include include\kernel32.inc
include include\user32.inc

includelib lib\kernel32.lib
includelib lib\user32.lib

.data
result db 100 dup(0); dup:duplicate重复
;char result[100]={0};
format db "%d",0; db:define byte字节类型
; char format[3]="%d";
prompt db "The result",0
above db "Above or equal", 0
below db "Below", 0

.code
main:         ; 标号
   mov eax, 0; eax:extended ax
   mov ebx, 1
again:
   add eax, ebx; eax=0+1+2+3
   add ebx, 1  ; ebx=4
   cmp ebx, 100; cmp:compare
   jbe again   ; jbe:jump if below or equal
   mov ebx, eax
   cmp eax, 5000
   jae above_or_equal
invoke MessageBox, 0, offset below, offset prompt,0
   jmp done
above_or_equal:
invoke MessageBox, 0, offset above, offset prompt, 0
done:   
   mov eax, ebx
invoke wsprintf, offset result, offset format, eax
invoke MessageBox,0,offset result,offset prompt,0
   ret

end main; 指定程序的起始执行点
```
保存为`sum.asm`到`C:\DEBUG`中（或你自己新建的文件夹的路径中）。然后在`Sublime Text 3`中，选择`Tools` -> `Build System` -> `Asm32`，再使用快捷键`ctrl`+`B`，即可完成编译。如果编译报错找不到`include`文件夹，请将masm32中`include`和`lib`两个文件夹复制到`C:\DEBUG`中，再次`ctrl`+`B`编译即可。生成的32位程序不能在DosBox中运行，直接双击打开即可。
