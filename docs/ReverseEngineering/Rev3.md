cs:00b6 : 
	mov cs:[0000] 0000:[46ch];

for(int i=0; i<8; i++){
	int a = cs:[0000];
	int d = 0x15a4e35;
	long long c = a\*d
	a = c & 0xFFFFFFFF
	a++;
	cs:[0000] = a;
	a >>= 16; // 4 bytes
	a &= 0x7FFF;
	unsigned char al = a &= 0x3F;
	cs:[i+0x4] = al;
}

calculate machine code

table = {A-Z, a-z, 0-9, '+', '/'}

for(int i=0; i<8; i++){
	lodsb al = [0x4 + i];
	al = table[al];
	cs:[0x19+i] = al;
}
unsigned char machine_code[8]; @cs:0019h;

unsigned char sn[12]; @cs:00sc
sn = gets();

input@cs:002c

table2 = {0-9,A-F}; @cs:00A5

ans[6]; @ cs:0039

for(int i=0; i<12; i+=2){
	unsigned char  j1, j2;
	lookup(j1, sn[i], table2);
	lookup(j2, sn[i+1], table2);
	ans[i/2] |= (j1 << 4);
	ans[i/2] |= j2;
}
e.g. input 0123456789AB
![[Pasted image 20231111105557.png]]

shell: 01A7 - 01BF 
can not simply disable encode step, cause there exist jmp, so decode and encode must exist in pair.!!!!!!!

`bochs don't need to set breakpoint, just use F7 to trace into int.`

rol ans[i] i

decode order:
1a8 1a9 1aa 1ad 1b0 1b3 1b5 1b6 1b8 1b9 1bb 1bc -> 1b5
for(int bx = 0; bx < 0x30; bx += 6){
	ax = bx;
	al = bx / 8;
	ah = bx % 8;
	ax = ans[al];
	swap(ah, al);
	ax <<= (bx%8)
	ax >>=0xA;
	lookup(al, table1) -> 067c:003F
}

ans = 0x4C, 0x6C, 0x66, 0x39, 0x64, 0x2B, 0x75, 0x36

regcode:
```c
#include<stdio.h>

unsigned char ans[9] = { 0x4C, 0x6C, 0x66, 0x39, 0x64, 0x2B, 0x75, 0x36, '\0'};
unsigned char sn[6];
unsigned char input[13];

unsigned char table[] = {'0','1','2','3','4','5','6','7','8','9','A','B','C','D','E','F'};

unsigned char machine_code[9];

unsigned char lookup(unsigned char x){
    if(x >= 0 && x <= 25){
        x = 'A' + x;
    }else if(x >= 26 && x <= 51){
        x = x - 26 + 'a';
    }else if(x >= 52 && x <= 61){
        x = x - 52 + '0';
    }else if(x == 62){
        x = '+';
    }else{ // '?'
        x = '/';
    }
    return x;
}

unsigned char revBase64(unsigned char x){
    if(x >= 'A' && x <= 'Z'){
        x = x - 'A';
    }else if(x >= 'a' && x <= 'z'){
        x = x - 'a' + 26;
    }else if(x >= '0' && x <= '9'){
        x = x - '0' + 52;
    }else if(x == '+'){
        x = 0x3e;
    }else{ // '?'
        x = 0x3f;
    }
    return x;
}

int main(){
    scanf("%s", machine_code);

    for(int i=0; i<8; i++){
        ans[i] = machine_code[i];
    }
    // printf("ans = %s\n", ans);
    for(int i=0; i<2; i++){
        int index = i*4;
        unsigned ch = revBase64(ans[index]);
        sn[i*3] = sn[i*3+1] = sn[i*3+2] = 0;
        sn[i*3] |= ((ch & 0x3F)<<2);
        ch = revBase64(ans[index+1]);
        sn[i*3] |= ((ch & 0x30) >> 4);
        sn[i*3+1] |= ((ch & 0xF) << 4);

        ch = revBase64(ans[index+2]);
        sn[i*3+1] |= ((ch & 0x3C) >> 2);
        sn[i*3+2] |= ((ch & 0x3) << 6);

        ch = revBase64(ans[index+3]);
        sn[i*3+2] |= (ch & 0x3F);
    }
    // printf("\nbefore base64 : ");
    // for(int i=0; i<6; i++){
    //     printf("%x ", sn[i]);
    // }
    for(int i=0; i<6; i++){
        unsigned char tmp = sn[i];
        sn[i] >>= (i+1);
        sn[i] |= (tmp << (8-(i+1)));
    }
    // printf("\nafter lookup : ");
    // for(int i=0; i<6; i++){
    //     printf("%x ", sn[i]);
    // }
    for(int i=0; i<12; i+=2){
        input[i] = (table[(((sn[i/2]) & (0xF0)) >> 4)]);
        input[i+1] = (table[(sn[i/2]& (0xF))]);
    }
    input[12] = '\0';
    printf("%s", input);
    return 0;
}
```
