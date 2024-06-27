refer to [cc.zju.edu.cn/bhh/pe.txt](http://cc.zju.edu.cn/bhh/pe.txt)

1. **PE头**
   前面的头是假的（DOS头），为了误导dos进入can not run in dos mode，头的结尾（3C）有个地址，才是windows的文件头（PE）

  **PE+28**处，是个32位数，第一条指令的地址（ΔEIP）需要加上40_0000h
  像这种相对地址称为RVA(Relative virtual address)
  40_0000称为base address，是windows exe载入的首地址，这也是虚拟地址，OS会通过页表将其转化为物理地址

  windows中cs ds es ss是操作系统赋值的，不需要在文件头定义
  **PE+6**处，2字节，section数目
  **PE+34**处，EXE载入内存的基地址，一般为40_000h
  **PE+38**处，1000h，一个页的大小，内存对齐值，载入内存时的对齐
  **PE+3C**处，200h，一个扇区，文件对齐值，编译时，每个section长度要对该值对齐，不够补0
  **PE+50**处、32位值，该EXE载入内存后的总长度（byte）,文件头总长度（也要对齐1000h）+各个section载入		   内存后的长度
  **PE+54**处、32位值，该EXE文件头总长度，DOS+PE，该值即为第一个段的位置
  **PE+80** 输入表(import table)的内存偏移（RVA）F8F3查表得到文件地址
  **PE+84** 输入表的内存长度
  **PE+F8**处、.text（code）
  Qv F2从16位切换到32位

  .text 共需28byte描述
  **.text+0** section name，不足在后面补0
  **.text+8**  4字节为该节内容的内存长度（需要拉伸）
  **.text+C** 4字节为该节载入内存的偏移地址（RVA）
  **.text+10h** 该节文件长度(已拉伸)
  **.text+14h** 该节在文件中的偏移位置
  **.text+24**为节的属性(`WREspcUIC`)
  **F8+F3即可查看PE头信息！！**

  .rsrc resource资源
  call 指令，地址为目标函数地址减去call下一条指令的地址

  

2. **import table**

   h= LoadLibrary("user32.dll"); h是dll的基地址
   user32.dll是多个API函数体的集合。Windows启动时会将它载入内存
   p = GetProcAddress(h, "MessageBoxA"); *也可填入API的序号*
   p即为MessageBoxA这个API的地址，需要进行API地址的重定位（import table）
   dos:

   ```asm
   invoke MessageBox, 0, offset result, offset prompt, 0
   ; function name, parameters...
   ```

   而printf()的机器语言代码是一个obj文件，包含在一个静态库文件cs.lib，编译时，编译器会打开lib文件搜索printf.obj，最后把printf.obj与main.obj合并

   **内存地址到文件地址定位**

   如何定位？`402004h` -> `2004h` -> `RVA=2000h, PysOffset=600h`section内 -> `600h+4h=604h` 

   

   一个exe可对应多个输入表。一个输入表对应一个DLL及该DLL包含的函数

   例：调用`user32.dll!MessageBoxA`，又调用`kernel32.dll!GetProcAddress`，那么输入表有两个

   

   **import_table**中的3个成员全是指针

   **import_table+00** 指向API的名字指针表（RVA），以 `00 00 00 00` (NULL)结束

   ​				API名字指针要+2，才指向真的API名字（以 `00` 结束）。前两个字节组成API的序号				（若有API名字，这个没用）。

   ​				如果地址巨大（大于80000000，最高位等于1，因为8 = b'100），说明不是名字指针。				去掉最高位获得API序号

   ```c
   if((x & 0x80000000) == 0)
         printf("API name=%s", baseaddress+x+2);
   else
         printf("API Ordinal Number=%d", x & 0x7FFFFFFF);
   ```

   **import_table + 0C** 4 byte 指向dll的名字，必须以00结束。（RVA） 

   **import_table+10h** 指向API地址表（RVA），以`00 00 00 00` (NULL) 结束，在文件中与名字指针表相同。				载入内存时被替换成API的真实地址，会作为函数指针，作为jmp或call的对象。

   ​				例如API地址表的RVA为2000，则检查代码段内有没有对`402000`的引用，因为`[402000]				`				会在载入内存时被替换为API函数的真实地址。真实程序中会`jmp [402000]`。

   ​				*这样就对API进行了统一重定位，不用对每个call都再重定位了！！*

   

   **每个输入表14h长**，直到连续14h个byte全是0，代表输入表结束。没有`count`

   重定位的时候，每个输入表结束后，要+0x14，去找下一个

   

3. **export table**

   ddl中有一个初始化函数，DllMain，dll被载入内存时调用，释放时也调用。该函数地址为IP的RVA

   ```c
   bool DllMain(HANDLE hModule,
          DWORD ul_reason_for_call, ...)
   ```

   ```c
   extern "C" __declspec(dllexport) int WINAPI add(...){...}
   ```

   `dllexport` 表示该函数需要被输出，如果没有，只能被内部调用

   一般存放于DLL中(仅有一份)

   **PE+78** 输出表内存偏移

   **PE+7C**输出表内存长度

   **PE+80** 输入表

   **export_table + 0C** dll的名字指针（名字可修改，载入dll时看的是文件名，这里仅为编译器生成的提示）

   **export_table + 10** 代表API序号的基数

   **export_table + 14** dll导出的所有的API的个数

   **export_table + 18** dll导出的有名字的API的个数

   **export_table + 1C** API地址表指针

   **export_table + 20** API名字指针表（无API序号）名字指针表中第`i`个名字的序号，为序号表中第`i`个值

   **export_table + 24** API序号表（16位）要和基数相加，序号即为**API地址表的下标**

   名字->序号->地址，`addr[ index[ pos[ name ] ] ]`

   PeExplorer

   **主页搜索：PE资源(resource)查看及修改工具**

   

4. **动态库API的调用**

   - 动态调用

   ```c
   HINSTANCE h;
   typedef int (WINAPI *PF)(int x, int y);
   // WINAPI 被宏定义为 __stdcall，参数由右到左压入堆栈，被调用者清理堆栈中的参数. 两个参数即为(retn 0x8)
   // __cdecl，参数清理由调用者负责，参数由右到左压入堆栈
   PF p;
   __asm int 3; // breakpoint
   h = LoadLibrary("***.dll");
   p = GetPorcAddress(h, "add");
   z = (*p)(10, 20); // z = 10 + 20
   ```

   - 静态调用

   lib文件是个空壳，真正的代码在`***.dll`中。在编译`***.dll`时会自动生成`.lib`文件，但是`.lib`文件中不包含函数体，只起到跳板作用。操作系统帮忙获取函数的地址，并为其生成输入表，这样载入内存就会自动完成重定位。

   ```c
   #pragma comment(lib, "***.lib")
   extern "C" int WINAPI add(int x, int y)
   ```

5. 重定位表

重定位表一般只存在于dll中。

exe载入内存的基地址和编译时的基地址是一致的，所以无需重定位。

而dll假定的地址可能被占用，所以要进行重定位。（编译时基地址`100000h`载入内存时`300000h`）

**PE+A0:** **重定位表指针和长度**



**reloc + 00** 重定位项基地址

**reloc + 04** 当前重定位表的长度，（包括头部8byte，后续数据长度为该值-8）

该表结束，即下一张表开始（下一张表地址=`addr(current_reloc) + len(current_reloc)`）

**reloc + 08 (2 byte)** 每项都是3开头。扔掉最高位3，加上重定位项基地址，即为内存地址(RVA)。

去掉3最大是`0FFF`，加上基地址不会超`1000`，所以基地址一般都是`1000h`倍数。



如`30 36 -> 00 36 -> 00 36 + 00 00 10 00 -> 00 00 10 36`

F2转换为32位，向上滚动几行（让指令从正确位置解析）

加上基地址差



Windows 脱壳

自动脱壳：

pushad or pushfd 后，对栈顶元素mpmd r（内存dword访问断点），然后运行，断住后记得向上滚动才能看到pop指令。

GetModuleHandle(dllname) ，拿到dll的handle

如何快速返回？查看堆栈顶部（即为返回地址，选中，按Enter）

返回值（eax）保存了起来。

从[ecx]取了API名字指针，

检测最高位是否等于1，（test ebx, 0x80000000）

加上基地址，并+2（跳过API hint）

 GetProcAddress（handle， name）获取API地址

其中一张表的名字指针被优化成0了，让壳代码自动填进去

ecx-4，让它指向原来的内容，填进新的表

利用5字节，改成一个call

```assembly
0x406644
call 0x406652
0x406652
mov eax, [ecx-0x4]
stosd ; mov es:[edi], eax
```

**记录输入表地址`0x40200C`**

**usercode地址`0x401000`**

dump内存内容

查看总长度：最后一个节的位置和长度。

OD->查看->内存，末地址40A000 - 400000 = 0xA000



或在内存中定位到40 0000再定位PE头，查看节描述。

+F8即为节描述

在内存窗口右键->MemoryDump->RangeDump



把4D5A改成其他的，然后qv就能打开了

checklist：

- PE+28 需要修改为user eip
- PE+3C 文件对齐
- PE+50 exe文件长度
- PE+80 输入表，改为user的输入表地址
- 节描述，每个节描述28bytes，修改文件长度和文件偏移（+10 + 14）,改成与内存一致即可（+8, +C的值）
- 第一个节的属性，要RWE。



clc表示函数正常返回



DOSBOX 新建一个，输入指令，Ctrl+F1帮助即可查询指令



强制让exe调用我的dll

**查看API的地址保存位置：**

​	PEexplorer， 查看import，API前面的RVA为存该API地址的变量的地址，call [RVA]即可调用API

​	OD，代码窗右键，查找，当前模块中的名称，右键按区段排序（按dll分块排序），右键复制到剪贴板，整个表，到其他地方可以搜索

​	QV，F7搜索API名字，然后搜索谁在引用（将**名字的地址-0x2**，并**转化为RVA**！，输入表用的RVA），注意搜索要用**小端**格式（RVA=57D0F => 0F 7D 05 00）,会搜到两次，在OD中排除其中一个（转化为RVA）



新增一个节。节描述写在其他节后面。查看已有节最后一个节的长度和偏移。

`page name (8byte) Msize RVA physize phyOffset`

`.patch 00001000 00063000 00001000 00058E00`

用010editor开辟新空间，Edit->Insert->Bytes

PE+06 更改节数目

PE+28 更改EIP为补丁代码

```assembly
0x58e00: jmp 0x58e20 ;留一部分存数据 这里的地址是文件内地址
0x58e05: "DllSample.dll" ;string
		"add"
0x58e20: push 0x463005 ; 0x58e05 -> RVA + 40_0000
		call [456300] ; call LoadLibrary, param: name
		push 0x463015 ; API name
		push eax      ; Handle
		call [4562A0]; call GetProcAddress, param: API name, handle, eax <= &add
		push 2
		push 1
		call eax      ; add(1, 2)
		add al, 30
		mov [463002], al ;store as string
		push 0
		push 463002
		push 463002
		push 0
		call [4569f0] ; call MessageBox
		push retaddr
		ret
```



**期末考试**

40min：25单选，50分（调试器用法，OD（；注释，：取标号），TD，Softice，IDA（搜索，交叉索引，对数据改成code解析，变量函数重命名，加注释，F5转化C语言），调试知识，汇编语言知识）

90min：实验：

* 脱壳（DOS(30min)，WINDOWS(7min)）

* 逆向分析算法写注册机（16位和32位，注意可能有壳代码）

  ds:si es:di

  dosbox File->new enter instruction, Ctrl+F1 can see descriptions

  

* 文件头分析（会变换一些，比如某个API的地址存在哪里（API地址表））





