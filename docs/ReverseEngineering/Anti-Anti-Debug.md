### Anti-Anti-Debug

Sign:

```assembly
pushf
pushf
pop ax
or ax 0100
push ax
popf
```

Interrupt address:

`0000:0004 Offset`

`0000:0006 Segment`

Use quickview to modify the program(decode and disable the interrupt). Be careful of branches.

IDAPro **F5** Decompile

DOS - BUFFERED KEYBOARD INPUT **DS:DX** -> buffer

**DS:DX+1** = strlen

```assembly
lodsb al, ds:[si]
stosb es:[di], al
```



```c
for(int i=0; i<0xC; i++){
    cx = 0xC-i;
    cx &= 0x1;
    cx ^= 0x1;
    cx <<= 2;
    str[i] <<= (cx);
    
}
01 23 45 67 89 AB
```



