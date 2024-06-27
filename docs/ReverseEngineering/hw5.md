**1**

![image-20231219100401514](C:\Users\13552\AppData\Roaming\Typora\typora-user-images\image-20231219100401514.png)

![image-20231219102837078](C:\Users\13552\AppData\Roaming\Typora\typora-user-images\image-20231219102837078.png)



![image-20231219100441819](C:\Users\13552\AppData\Roaming\Typora\typora-user-images\image-20231219100441819.png)

shell   code :412014 ebp= 412013

shell code end :412425h

OllyDBG dump

import table 13008 file 7208h



41249E< = kernel32 handle

ebp+0x51=412064=Procname <=Procaddr

Replace the first 4  bytes of Procname with it's address,start from 412064

**2**

shell_code exit `067C:01E9`

code `0683:0096`



SI=0013

DI=0024

**username:`067C:0013(DS:SI)`** 

**password:`067C:0024(ES+DI)`**

=> call cs:005F (check code)

cs:005F

```assembly
0683:005F 53              PUSH    BX
0683:0060 56              PUSH    SI
0683:0061 57              PUSH    DI
0683:0062 33C0            XOR     AX,AX
0683:0064 33C9            XOR     CX,CX
0683:0066 33D2            XOR     DX,DX

0683:0068 AC              LODSB
0683:0069 0AC0            OR      AL,AL
0683:006B 0F840700        JE      0076
0683:006F 32C1            XOR     AL,CL
0683:0071 03D0            ADD     DX,AX
0683:0073 41              INC     CX
0683:0074 EBF2            JMP     0068
/*
	uchar username[];
	short dx;
	for(int i=0; i<len; i++){
         dx += username[i] ^ i;
	}
*/
0683:0076 660FB7C2        MOVZX   EAX,DX
0683:007A BB3500          MOV     BX,0035
0683:007D E8B0FF          CALL    0030
0683:0080 8BC8            MOV     CX,AX
0683:0082 B80000          MOV     AX,0000
0683:0085 E30B            JCXZ    0092
0683:0087 8BF3            MOV     SI,BX
0683:0089 F3A6            REPZ CMPSB
0683:008B 0F850300        JNE     0092
0683:008F B80100          MOV     AX,0001
0683:0092 5F              POP     DI
0683:0093 5E              POP     SI
0683:0094 5B              POP     BX
0683:0095 C3              RET
0683:0096 B87C06          MOV     AX,067C
```

cs:0030

```assembly
0683:0030 6655            PUSH    EBP
0683:0032 53              PUSH    BX
0683:0033 B90000          MOV     CX,0000
LOOP1:
0683:0036 66BA00000000    MOV     EDX,00000000
0683:003C 66BD0A000000    MOV     EBP,0000000A
0683:0042 66F7F5          DIV     EBP
/*
	EDX为高位，EAX为低位
	EDX:EAX / EBP => EAX ... EDX
	0000 0000 XOR_RESULT / 0000 000A
	EDX = XOR_RESULT % 0000 000A + 30h
*/
0683:0045 80C230          ADD     DL,30
0683:0048 52              PUSH    DX
0683:0049 41              INC     CX
0683:004A 6683F800        CMP     EAX,+00
0683:004E 75E6            JNZ     0036
/*
	int cnt = 0;
	do{
		int div = xor_result / 0xA;
		int rem = xor_result % 0xA + 0x30;
		push(rem)
		cnt++;
	}while(div != 0);
*/
0683:0050 8BC1            MOV     AX,CX
LOOP2
0683:0052 5A              POP     DX
0683:0053 8817            MOV     [BX],DL // bx = 0035
0683:0055 43              INC     BX
0683:0056 E2FA            LOOP    0052
/*
	int offset = 0x35, i;
	for(i=0; i<cnt; i++){
		(uchar) cs:[offset+i] = pop();
	}
	cs:[offset+i] = 00;
*/
0683:0058 C60700          MOV     BYTE PTR [BX],00
0683:005B 5B              POP     BX
0683:005C 665D            POP     EBP
0683:005E C3              RET
```





