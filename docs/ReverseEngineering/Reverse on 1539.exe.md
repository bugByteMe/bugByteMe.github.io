**-Remember to deactivate the hardware interrupt**
```shell
r fl i
```

**1 Shell 1:**
```shell
bpmb 067c:00b6 x
x
```


**2 Shell 2:**
```shell
bpmb 067c:0137 x
x
```

**3 Base64 encode**

6 bit forms a 26('A'-'Z')+26('a'-'z')+10('0'-'9')+2('+','?')=64 char set
```shell
bpmb 067c:01b6 x
x
```

**4 Swap & xor**

peseudo code:
for(int i=0; i<16; i++){
	uchar l = base64[i];
	uchar h = base64[i+0x10];
	l ^= 0x7;
	h ^= 0xc;
	base64[i] = h;
	base64[i+0x10] = l;
	or:
	swap(base[i], base[i+0x10])
	base[i] ^= 0xc;
	base[i+0x10] ^= 0x7;
}

```shell
bpmb 067c:01ce x
x
```

**5 Shuffle**

uchar resstr[] @ 003C
for(int i=0; i<16; i++){
	resstr[i] = base64[i*\2];
	resstr[i+0x10] = base64[i\*2+1];
}
```shell
bpmb 067c:01e4 x
x
```

**6 Compare**

cmpare with ans @ 067c:005d len = 33

ans[] = {0x45, 0x49, 0x41, 0x67, 0x56, 0x4e, 0x6f, 0x4a,
0x66, 0x49, 0x5d, 0x73, 0x5d, 0x45, 0x4e, 0x41,
0x48, 0x3d, 0x76, 0x6b, 0x48, 0x62, 0x75, 0x35,
0x40, 0x37, 0x69, 0x42, 0x43, 0x69, 0x43, 0x7d,
0x00}

![[Pasted image 20231107161408.png]]

compare ends at 067c:01f9

**Register code:**
```c
#include<stdio.h>

unsigned char ans[] = {0x45, 0x49, 0x41, 0x67, 0x56, 0x4e, 0x6f, 0x4a,
0x66, 0x49, 0x5d, 0x73, 0x5d, 0x45, 0x4e, 0x41,
0x48, 0x3d, 0x76, 0x6b, 0x48, 0x62, 0x75, 0x35,
0x40, 0x37, 0x69, 0x42, 0x43, 0x69, 0x43, 0x7d,
0x00};

unsigned char base64[33];
unsigned char sn[25];

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
    for(int i=0; i<16; i++){
        base64[i*2] = ans[i];
        base64[i*2+1] = ans[i+0x10];
    }

    for(int i=0; i<16; i++){
        base64[i] ^= 0xc;
        base64[i+0x10] ^= 0x7;
        unsigned char tmp = base64[i];
        base64[i] = base64[i+0x10];
        base64[i+0x10] = tmp;
    }

    for(int i=0; i<8; i++){
        int index = i*4;
        unsigned ch = revBase64(base64[index]);
        sn[i*3] = sn[i*3+1] = sn[i*3+2] = 0;
        sn[i*3] |= ((ch & 0x3F)<<2);
        ch = revBase64(base64[index+1]);
        sn[i*3] |= ((ch & 0x30) >> 4);
        sn[i*3+1] |= ((ch & 0xF) << 4);

        ch = revBase64(base64[index+2]);
        sn[i*3+1] |= ((ch & 0x3C) >> 2);
        sn[i*3+2] |= ((ch & 0x3) << 6);

        ch = revBase64(base64[index+3]);
        sn[i*3+2] |= (ch & 0x3F);
    }

    for(int i=0; i<24; i++){
        printf("%c", sn[i]);
    }

    return 0;
}
```
 **Answer:**
 
 `hctf{Dd0g 1s 1539 d0gs!}`