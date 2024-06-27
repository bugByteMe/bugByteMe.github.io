```asm
push ds
pop es
push si
pop di
lodsb al, ds:[si]
xor al, 42h
stosb es:[di], al
loop 4h(cx正好就是数据长度)
retf
```
按enter进入汇编模式
Shellcode需要对ds+10h:0000到cs:ip（shellcode是追加到末尾的）进行还原
假设长度不超过FFFFh

calculate cs, ip, 1st segment
```asm
0350 mov ax, ds
0352 add ax, 0010 #首段地址
0355 mov dx, cs
0357 call 035A(下一条指令的地址，当前地址加3)
#如何获得ip？执行call后会将ip压入堆栈
035A pop bx #bx = 35A
035B sub bx, 0Ah #bx 相当于
```
calculate block length
```asm
sub dx, ax  #cs - 首段段地址，这里的cs为shellcode的cs
shl dx, 4h  # *10h
add dx, bx  #块的总长度
mov cx, dx
```
decode
```asm
xor si, si
xor di, di
push ds
push es
mov ds, ax # ds=首段段地址
mov es, ax # es=首段段地址
*lodsb al, ds:[si]
xor al, 42h
stosb es:[di], al
loop * 
pop es
pop ds #无法还原ds，因为堆栈段被修改了，需要自己分配堆栈段
```

```asm
.386
code segment use16
assume cs code
shell:
	;mov sp, offset stk+100h 
	;不能这样写，因为shell的偏移地址可能不为0	
	call next
next:
	pop bx; bx = [next]的运行时的偏移地址
	sub bx, offset next - offset shell
			; bx 即为shell的偏移地址
	
	cli ; 禁止中断
	mov ax, cs
	mov ss, ax
	lea sp, stk[bx+100h] ;修改为自己的堆栈，或者直接将文件头的ss，sp
						 ;修改为自己的堆栈
	sti

	push ds
	push es
	mov ax, ds
	add ax, 10h ;ax = 首段段地址, ds=psp psp长度为100h
	mov bp, ax ;备份
	mov ds, ax
	mov es, ax
	mov dx, cs
	sub dx, ax
	movzx edx, dx ;防止溢出
	shl edx, 4h
	movzx ebx, bx
	add edx, ebx ;edx = blocklen

	xor si, si
	xor di, di
decode_test:
	lodsb
	xor al 42h
	stosb
	cmp si, 0 ;处理跨段，防止si一直循环(从FFFF变到0)
	jne skip
	mov ax, ds
	add ax, 1000h ;已经解密了10000h字节了
	mov ds, ax
	mov es, ax
	add 
skip:
	dec edx
	jnz decode_text
	mov 
	mov cx, [bx + reloc_count] ;bx 为shell的偏移地址
							   ;访问任何变量都要加上bx
	push cs
	pop ds
	lea si, [bx+reloc_table]
reloc_next:
	mov di, ds:[si] ;偏移地址
	mov dx, ds:[si+2] ;delta段地址
	add dx, bp
	mov es, dx
	add es:[di], bp
	add si, 4
	loop reloc_next

	add [bx+delta_ss], bp
	add [bx+delta_cs], bp
	pop es
	pop ds
	cli
	mov ss cs:[bx+delta_ss] ;恢复用户堆栈
	mov sp cs:[bx+old_sp]
	sti
	jmp dword ptr cs:[bx+old_ip] ;返回

;用老exe中的值替换：（需要补入一些placeholder，保证对其方便直接复制源程序的值）
delta_ss dw 0
old_sp dw 0
old_ip dw 0
delta_cs dw 0
reloc_count dw 0
reloc_table db 200h dup(0) ;每项2个word，前一个word为偏移地址，
						   ;后一个word为delta段地址

stk db 100h dup(0)
code ends
end
```

选中后 shift F3 即可用原exe选中内容覆盖
