#### 调用MFC的exe

API只有序号，没有名字

**import table** 

名字指针表：地址巨大（大于80000000，最高位等于1，因为8 = b'100），说明不是名字指针。去掉最高位即为API序号

QV F8+F5 跳到第一条指令处



**PeCompact 脱壳**

`pushfd` 压入32位flag寄存器（`EFL`）

`pushad` 除flag和段地址的寄存器都压入堆栈（`eax, ecx, edx, ebx, esp（这条push之前的esp）, ebp, esi, edi`按顺序压入，共`20h`字节）

一定存在`popad, popfd`，可在push完后在堆栈顶端设置硬件访问断点(Dword, 4byte)。

右键->分析->分析代码 ，恢复解密的代码

插件->OllyDump->Dump debugged process



手工脱壳

一定会调用`LoadLibraryA` (载入dll，并获取基地址)或 `GetModuleHandleA`（获取dll基地址，需要曾经载入过），和GetProcedureAddress（获得API地址）

Ctrl+g GetModuleHandleA，设置断点。

断住后取消断点。在堆栈中看返回地址（栈顶），选中敲回车就可过去。

`or eax eax`判断返回的基地址是否等于0，否则保存。

`ecx`指向其中一项表，`[ecx]`名字指针表。然后加上基地址，得到绝对地址。`add ebx edx`，加2跳过前两个没用的数字。





