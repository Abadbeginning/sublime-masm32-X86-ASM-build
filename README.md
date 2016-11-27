<h1>在Sublime Text 3中使用masm32配置X86汇编(16位&32位）的build-system。</h1>
<h2>Preparation</h2>
<h3><a href="http://www.sublimetext.com/3">Sublime Text 3</a></h3>
安装完Sublime后为其安装<a href="https://packagecontrol.io/installation">Package Control</a>。
<br>
接下来，使用快捷键ctrl+shift+p打开Package Control，输入Install Package，打开插件列表，输入插件名MasmAssembly，回车，就可以自动安装了。
<h3><a href="http://www.masm32.com/download.htm">MASM32</a></h3>
<h3><a href="http://www.dosbox.com/download.php?main=1">DosBox</a></h3>
<h2>Step 2 16位汇编编译环境配置</h2>
<h3>添加环境变量</h3>
打开masm32文件夹,将其中的include、lib的路径分别添加到系统的环境变量include、lib中，如果系统中没有这两个变量名则新建之。在系统的path变量的末尾加上masm32文件夹中bin的路径。
<h3>新建批处理文件</h3>
打开Sublime，新建空白文件，编辑内容为<br>
ml /c %1.asm<br>
link16 %1.obj;<br>


