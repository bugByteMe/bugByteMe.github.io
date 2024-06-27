Basic info
* Header length = 20h
* Δss = 2023h
* sp = 01A2h
* Δcs = 2023h
* ip = 000Ch
* first seg = 20h
* stack = 203F2h (in file)
* code = 2025Ch (in file)

bx = ip

ax = psp+10h = first-seg
bp = ds = es = first-seg
dx = delta_cs
edx = (dx << 4)   (dx \* 10h)
ebx = bx = ip
**edx** =dx \* 10h + ebx = delta_cs \* 10h + ip = **delta_shell_code_s**
shellcode_len = \[2025C, 202F2) = 96h
067C:0000 - 269F:000C total 2023C
2025C = 200h \* 101 + 5C
si = di = 0

1. DecodeLoop:
	ecx = 10000h
	edx = edx < ecx ? 0 : (delta_shell_code - ecx)
	
	es:\[di] = ((\ds: \[si]) << 4 ) ^ 57 (ds = es = first-seg) (CLD, positive direction)
	![[Pasted image 20231205111450.png]]
	ecx = ecx / 10h (1000h)
	es = ds = ds(first-seg) + 1000h
	loop DecodeLoop(edx != 0)

2. Relocation
	count = cx = cs:\[bx+019C] = 2
	\*table = di = cs:\[bx+01AE] 
	si = bx+di+0196h (cs:1C0)
	Relocation:
	di = cs:\[si] (offset)
	dx = cs:\[si+02h] (delta_seg)
	es = bp(first-seg) + dx
	es:\[di] += bp(first-seg)
	si += 0004h
	loop Relocation

Reloc table

269F:01C0  01 00 20 20 06 00 20 20

![[Pasted image 20231205111513.png]]
3. Recover registers
main entry:
ss = 267C delta_ss = 2000
sp = 200
psp+10h = 67C
jmp 269C:0000 = 269C:0
deltamain = 269C0 - 67C00 = 2020:0000
![[Pasted image 20231205111540.png]]
