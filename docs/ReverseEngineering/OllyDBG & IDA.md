#### OllyDBG

**Ctrl F9** : Finish current function
**Alt F9**: Finish kernel code, back to user code

Select data in dump:
	**Alt Enter**: goto this pointer in dump (nested pointer)
	**Shift Enter**: goto this pointer in code (function pointer)

Hardware breakpoint:
RightClick->breakpoint->hardware R/W

#### IDA-Pro

`;` `:` comment

`n` rename an address



1. IDA使用技巧
Ida flare,自己做库识别文件。
c命令把机器码翻译成指令
d命令把机器码翻译成变量，同时切换byte,word,dword,alt+d选择更多类型
u命令把机器码转化成未定义
n命令改函数名和变量名
f5转化为C语言

ida中按shift+F5可以看到识别用的特征库
ida有一个工具叫FLAIR可以从第三方的lib文件中
抽取特征生成一个特征库.sig。
生成的.sig文件应该拷到IDA\sig目录里面。
IDA\sig\$vc_libeay32.sig
IDA\sig\$vc_ssleay32.sig
上述两个文件是从openssl的lib文件中提取出来的特征库。
按Shift+F5再按ins键选择特征库应用到当前的exe中。
IDA中可以搜索字符串，也可以搜索16进制串:
(1)搜索字符串: 
①Alt+T（速度慢，对反编译结果搜索）, 字符串不可加引号; 搜到的有可能是字符串本身的内容，也可能是变量名、函数名等。
②Alt+B（速度快，对机器码搜索）, 字符串需要加引号; 搜到的是字符串本身的内容。
(2)搜索16进制串:
Alt+B, 输入16进制机器码
交叉引用(cross reference):
变量名或函数名右侧分号处有提示DATA XREF或
CODE XREF, 双击其中一个XREF可以跳转到相关的引用语句，或者view->open subviews->cross reference可以查看全部引用。
如何对变量名或函数名重命令: n
Esc回退, Ctrl+Enter前进
如何设定变量的宽度: d或alt+d
如何定义一个数组: *
如何定义一个字符串: a
如何把当前的反编译结果转化成未定义状态: u
d命令可以把未定义的内容识别成变量;
c命令可以把未定义的内容识别成指令;
如何注释: 分号

用IDA Pro可以对.exe进行反汇编(把.exe转成.asm)，并自动识别库函数的名字。
IDA: Interactive DisAssembler交互式的反汇编器
举例说明用IDA分析updater.exe:
按alt+b可以搜索十六进制值或字符串
输入"Incorrect registration code"
注意AnsiString格式的字符串前面有一个long类型的长度值。
当IDA找到该字符串时，可以看到字符串右侧有一个注释:
DATA XREF: ...
当引用者仅有一个时, 用户只要双击"DATA XREF:"就可以跳到引用者那里, 按Esc可以返回到字符串那里。
当引用者有多个时，鼠标点到DATA XREF:, 再选菜单View->Open subview->Cross Reference就可以打开交叉引用的窗口，里面会列出各个引用者的信息。
IDA里面双击某个函数时跳到函数体里面，按Esc回退,按Ctrl+Enter可以前进。
OD里面按Esc回退，`或~前进。
**选中变量名、函数名、标号，按n可以进行重命名。
按;可以进行注释**


2. OD使用技巧
可以在源程序main()函数的开头插入一条汇编指令int 3(TC中写成asm int 3; VC中写成__asm int 3)，当exe在调试器里执行时，调试器遇到int 3指令会把它当成一个断点。int 3指令的机器码仅有一个字节，是0CCh。
VC编译时build->set active configuration->release; project->setting->C++->disable;

在OD中要让int 3断点功能发挥作用，需要做以下设置:
(1) 选项->调试设置->int 3中断前面的勾去掉
(2) 插件->StrongOD->Skip some exceptions
的勾去掉

在call指令处敲回车可以进入函数体, Esc回退, ~前进。
如何对函数、标号、变量重命名: 冒号
**如何注释: 分号**

**如何加标签：冒号**

要修改某条指令, 选中该指令, 输入指令的汇编语句即可修改。
在OD中若要修改某个变量的值，则先ctrl+g定位到该变量的地址，选中首字节，敲键盘数字键或A-F字母，就会弹出修改对话框; 
要是不小心改坏了内存信息，可以选中该块内容，按Alt+Backspace恢复原状。
要修改某个寄存器的值，选中该寄存器的值，再双击, 标志位也可以通过双击改变; 
若要改变EIP的值，只要选中某个地址，右键->此处为新eip即可。双击EIP是作用是定位到EIP。

3. TD

   Turbo Debugger
   Turbo Debugger是全屏幕调试工具
   Ctrl+F2 program reset
   Ctrl+o  回到当前将要执行的指令处
   Ctrl+g  定位到某个地址
   F2 设断点
   F4 goto here
   F7 跟踪到call里面
   F8 单步

