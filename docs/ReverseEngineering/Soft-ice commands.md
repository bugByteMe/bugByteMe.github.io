`d ss:sp` see memory dump
`bpmb addr x` set hardware execute breakpoint
`bc *` clear all breakpoints
`u addr` jump to address in code window
`wd x` set data window height
`wc x` set code window height
`? expression` can calculate expression
`.`return to cs:ip in code window

`exit rd` stop debugging

copy data in data window: 用bochs自带调试器，writememory，记住地址，换成物理地址
`help writemem "<filename>" <laddr> <len>`
filename 是bochs外地址



```
soft-ice常用命令
任何时候按ctrl+d可以呼出soft-ice, 再按ctrl+d可以隐藏
单步跟踪操作跟TD完全一样, 即F7=trace into, F8=step over, F4=goto cursor
ec     在命令窗与代码窗之间切换(enter code), 快捷键F6
rs     查看用户屏(restore screen), 快捷键F5
.      表示回到当前cs:ip, 相当于TD里面的ctrl+o
?                           显示帮助信息
?      表达式               计算表达式的值
d      段地址:偏移地址      查看该地址处内存变量的值(dump)
u      段地址:偏移地址      查看该地址处的指令(unassemble)
e      段地址:偏移地址 值   修改该地址处内存变量的值(edit), 例如: e ds:0 12 34 56 78
a      段地址:偏移地址 指令 修改该地址处的指令(assemble), 例如:a cs:ip mov ah, 2
s      地址 L长度 值        在[地址,地址+长度-1]范围内搜索该值, 例如: s cs:0 L10 B4 4C
f      地址 L长度 值        把该值填入[地址,地址+长度-1], 例如: f ds:0 L10 0
m      地址1 L长度 地址2    把[地址1,地址1+长度-1]范围内的块复制到地址2, 例如: m ds:0 L 10 ds:100
x      继续执行, 相当于debug中的g命令, 快捷键F9或ctrl+d
p ret  运行到返回(执行到ret或iret), 快捷键F12
bpx    段地址:偏移地址      设置软件断点,快捷键F2
bpmb   段地址:偏移地址 x    设置硬件执行断点
bpmb   段地址:偏移地址 r    设置硬件读断点
bpmb   段地址:偏移地址 w    设置硬件写断点
bpint  中断号n AH=m         当发生中断n且ah=m时断住
bpio   n                    当n号端口有i/o时断住
i3here on/off               当cpu执行到int 3指令时要不要让soft-ice弹出

map        查看内存分配, 可以用此命令查到各个进程的psp段址及占用内存的节长度(1节=16字节)
BL         显示所有断点
BC     n   清除第n个断点
BD     n   禁止第n个断点
BE     n   允许第n个断点
BC     *   清除所有断点
exit   rd  若调试时产生了非法操作导致dos系统崩溃,此命令可以强制恢复中断向量表。
hboot      重启(hard boot)
```

**bochs:**
pb: physical breakpoint

