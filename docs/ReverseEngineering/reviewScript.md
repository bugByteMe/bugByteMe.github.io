```c
uchar dx = 0;
for(int i=0; i<userlen; i++){
    dx += user[i] ^ i;
}
int eax = dx;
uchar bx = 35;

while(eax > 0){
    dl = eax % 0xA + 30;
    eax = eax / 0xA;
    push dl
}
while(len) pop dl -> 35

```

