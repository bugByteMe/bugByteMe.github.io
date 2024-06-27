#### 考试

理论45min 60 分

判断题+单选题 主要是指令，会有程序阅读题（要细心），基础性知识（16个寄存器用法），地址相关知识（段，偏移，物理）

实验80min 40 分 （来源于作业或上课例子）

2道函数题

2道程序填空题



注意寄存器用前清零

注意ds的初始化

注意函数中bx,bp,si,di需要callee保护

同时调用中断前，保护ax, bx, cx, dx，中断函数可能会破坏他们

# 输出输出中断

* 读字符

  ah = **01**, int 21h

  al = getchar();

* 写字符

  ah = **02**, int 21h

  putchar(**dl**);

* 写buffer

  ah = **09h**, int 21h

  puts(ds:dx), ds:**dx** 需要以`$`结束

* 读buffer

  ah = **0Ah**, int 21h

  ds:[dx] = max size of buffer

  ds:[dx+1] = number of char read

  ds:[dx+2] = actual input

# 寻址规范

TODO 32bit

可以放在方括号内的寄存器：bx, bp, si, di

组合方式 `[{bx, bp}+ {si, di} + imm]`

# 数据传输

* **mov**

  两个操作数不能都为内存变量

* **push, pop**

  不能操作8位寄存器，但可以操作8位变量

* **xchg**

  `xchg r1, r2` swap(r1, r2)

# 地址传送

* **lea**

  `lea dest, src` dest = &src

  这个指令通常用来**算数**（地址内容要符合寻址规范）

  `lea dx, [bx+si+3]`

* **lds**

  `lds r1, dword ptr x` 把内存存储的地址放入ds:r1

* **les**

  `les r1, dword ptr x` 把内存存储的地址放入es:r1

# 标志寄存器

* **标志位**

  * **PF** Parity Flag

    统计计算结果低8位1的个数，偶数时PF=1

  * **AF** Auxiliary Flag

    辅助进位标志，低4位向高四位进位或借位时为1

    用于BCD码的计算 （daa）

  * **CF, ZF, SF, OF**

  * **DF**

    direction flag 跟字符串操作有关

  * **TF**

    trap flag 单步模式，执行该指令前TF=1，执行该指令后插入int 1

  * **IF**

    interrupt flag 是否允许中断 （cli, sti）

    

* **改变标志寄存器**

  * 暴力修改 (TF 只能通过该方法)

    ```assembly
    pushf
    pop ax
    ....
    push ax
    popf
    ```

  * stc, clc (CF)

  * std, cld (DF)

  * sti, cli (IF)

# 符号扩充

* **CBW**

  convert byte to word

  `ax <- al`

* **CWD**

  convert word to dword

  `dx:ax <- ax`

* **CDQ**

  convert dword to qword

  `edx:eax <- eax`

* **movzx, movsx**

  `movzx r1, r2`

  `movsx r1, r2`

# XLAT

translate (xlate)

ds:bx 指向数组首地址，al为offset

`al = ds:[bx+al]`

# ADD

* **inc** (不影响CF)

  increment

* **adc**

  add with carry

  `adc r1, r2` r1 = r1 + r2 + CF
  
* **daa**

  decimal adjust for addition （计算BCD码加减法）

  ```assembly
  mov al 29h
  add al 1 ; al = 2Ah
  daa ; al = 30h  BCD(29) + 1 = BCD(30)
  ```

# SUB

* **dec** (不影响CF)

  decrement

* **neg** (由于本质做减法运算，影响CF，ZF，SF)

  `neg register` register = 0 - register

* **sbb** (sub with borrow)

  `sbb r1, r2` r1 = r1 - r2 - CF

# MUL

* **8bit * 8bit**

  `ax = al * 8bit`

* **16bit * 16bit**

  `dx:ax = ax * 16bit`

* **32bit * 32bit**

  `edx:eax = eax * 32bit`

* **imul**

  `imul r1, r2 or [addr]` r1 *= r2 or var

* **mul extended**

  `mul r1, r2, imm`

  r1 = r2 * imm

# DIV

* **16bit / 8bit**

  `ax / (8bit) = al ... ah`

* **32bit / 16bit**

  `dx:ax / (16bit) = ax ... dx`

* **64bit / 32bit**

  `edx:eax / (32bit) = eax ... edx`

* **overflow**

  * div 0
  * quotient overflow

  除法溢出触发int 00h中断，自定义int 00h中断例子

  ```assembly
  mov ax, 0
  mov es, ax
  mov word ptr es:[0], offset my_int_00h
  mov word ptr es:[2], cs
  ```

# Float

* **运算指令**

  fadd, fsub, fmul, fdiv

  * `fadd st(a), st(b)` st(a) += st(b)
  * `fsub st(a), st(b)` st(a) -= st(b)
  * `fmul st(a) or [addr]`, st(0) *= x
  * `fdiv st(a) or [addr]`, st(0) *= x

* **fld** (float load)

* **fild** (float load from integer)

* **fst** (store float to st(0))

* **fstp** (pop st(0))

  `fstp st(a) or [addr]`, dst = st(0)

# 逻辑运算

* **test**

  `test r1, r2` = `and r1, r2`(丢弃结果)

* **cmp**

  `cmp r1, r2`= `sub r1, r2`（丢弃结果）

# Shift

* **shl, shr**

  逻辑左移，逻辑右移

* **sal, sar**

  算数左移，算数右移（符号扩展）

* **rol, ror**

  循环移位

* **rcl, rcr**

  带进位循环移位

  rcl : {CF, register}作为整体循环移位

  rcr : {register, CF}作为整体循环移位

# String

* **rep**基本结构

  ```c
  again:
  if(cx == 0)
  	goto done
  ... ...
  cx--
  **je/jne done** repe repne
  goto again
  ```

* **stosb, stosw, stosd**

  es:di = al

* **lodsb, lodsw, lodsd**

  al = ds:si

* **movsb, movsw, movsd**

  ```c
  es:di = ds:si
  if(df == 0){
  	si++; di++;
  }else{
  	si--; di--;
  }
  ```

* **cmpsb, cmpsw, cmpsd**

  ```c
  cmp es:di, ds:si
  if(df == 0){
  	si++; di++;
  }else{
  	si--; di--;
  }
  ```

  全等判断：

  ```assembly
  repe cmpsb
  je equal
  ... ...
  ```

  取到最后一个不相等字符：

  ```assembly
  repe cmpsb
  dec si
  dec di
  ```

* **scasb, scasw, scasd**

  ```assembly
  cmp al, es:di
  ```

  获取字符串长度：(假设ds si已赋值)

  ```assembly
  mov al, 0
  mov cx, 0FFFFh
  repne scasb
  not cx ; cx = FFFF-cx
  dec cx
  ```

  

# Loop

先cx--，再判断cx是否等于0

如果需要防止cx=0时进入循环，使用`jcxz`

```asm
jcxz done
next:
... ...
loop next
done:
... ...
```

# Jump

* **短跳 （short jump EB_ _）**

  `jmp offset or lable`

  所有条件jmp都是短跳

  jmp的机器码为目标地址-下一条ip地址

* **近跳（near jump E9_ _ _ _）**

  `jmp 1000h`

  `jmp ax`

  `jmp word ptr [addr]`

* **远跳（far jump EA_ _ _ _ _ _ _ _）**

  `jmp dword ptr [addr]`

  `jmp 1000:1000` 错误，不支持常量跳转，需要将机器码在代码段定义为数据

  段地址：偏移地址按小段格式存放（段地址在高地址）
  
* **条件跳**

  无符号数比较：

  * ja (CF=0 && ZF=0)
  * jb (CF=1)

  有符号数比较：(SF ^ OF) 是不考虑溢出情况下的真实符号

  * jg (SF==OF && ZF==0)
  * jl (SF != OF)

# Call

* **近调用与远调用**

  * 近调用 f proc near... ... f endp
  * 远调用 f proc ... ... f endp

* **变量传参（全局变量）**

* **寄存器传参**

* **堆栈传参的三种方式**

  * **__cdecl (c declare)**

    从右到左压入堆栈，堆栈中的参数由调用者（caller）负责清理

  * **__pascal**

    参数从左到右顺序压入堆栈，被调用者（callee）清理堆栈

    callee清理示例：(ret n)

    ```asm
    f:
    push bp
    mov bp, sp
    mov ax, [bp+6]
    add ax, [bp+4]
    pop bp
    ret 4; pop ip, sp = sp + 4
    ```

  * **__stdcall**

    参数从右到左压入堆栈，由被调用者（callee）清理堆栈

* **动态变量（局部变量）**

  ```asm
  push bp
  mov bp, sp
  sub sp, n ;在堆栈上开辟n bytes空间
  mov [bp-2], ax ;给第一个变量赋值
  ... ...
  mov sp, bp
  pop bp
  ```

  [bp-x] 寻找局部变量，[bp+x] 寻找参数

* **callee saved registers**

  bp, bx, si, di


#### 混合编程 TODO

```c
// naked 不让编译器加入其它指令，比如构造堆栈框架
__declspec(naked) int __stdcall f(int a, int b){
	__asm
	{
		push ebp
		mov ebp, esp
		mov eax, [ebp+8]
		add eax, [ebp+0Ch]
		pop ebp
		ret 8
	}
}
```



dos.bxrc 搜索clock，改成sync=realtime, rtc_sync=1
