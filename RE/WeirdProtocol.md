Sử dụng **Process Monitor** để theo dõi quá trình chạy file WeirdProtocol.exe, nhận thấy chương trình tạo 1 file nằm ở trong thư mục `C:\Users\Admin\AppData\Local\Temp`. File này chính là Server dùng để giao tiếp với WeirdProtocol.exe bằng socket trên port `4919`. Cho nên có thể bỏ qua file này luôn, chỉ cần phân tích file server bằng IDA Pro để tìm flag.

```C++
int __usercall sub_3B7680@<eax>(int a1@<xmm0>)
{
  int v1; // eax
  int v2; // edx
  int v3; // ecx
  int v4; // edx
  int v5; // eax
  int v6; // edx
  int v7; // ecx
  int v8; // ST08_4
  int v9; // ST0C_4
  int v11; // [esp+0h] [ebp-778h]
  int v12; // [esp+10h] [ebp-768h]
  unsigned int i; // [esp+25Ch] [ebp-51Ch]
  char v14; // [esp+268h] [ebp-510h]
  char v15; // [esp+269h] [ebp-50Fh]
  char v16; // [esp+26Ah] [ebp-50Eh]
  char v17; // [esp+26Bh] [ebp-50Dh]
  char v18; // [esp+26Ch] [ebp-50Ch]
  char v19; // [esp+26Dh] [ebp-50Bh]
  char v20; // [esp+26Eh] [ebp-50Ah]
  char v21; // [esp+26Fh] [ebp-509h]
  char v22; // [esp+270h] [ebp-508h]
  char v23; // [esp+271h] [ebp-507h]
  char v24; // [esp+272h] [ebp-506h]
  char v25; // [esp+273h] [ebp-505h]
  char v26; // [esp+274h] [ebp-504h]
  char v27; // [esp+275h] [ebp-503h]
  char v28; // [esp+276h] [ebp-502h]
  char v29; // [esp+277h] [ebp-501h]
  char v30; // [esp+278h] [ebp-500h]
  char v31; // [esp+279h] [ebp-4FFh]
  char v32; // [esp+27Ah] [ebp-4FEh]
  char v33; // [esp+27Bh] [ebp-4FDh]
  char v34; // [esp+27Ch] [ebp-4FCh]
  char v35; // [esp+27Dh] [ebp-4FBh]
  char v36; // [esp+27Eh] [ebp-4FAh]
  char v37; // [esp+27Fh] [ebp-4F9h]
  char v38; // [esp+280h] [ebp-4F8h]
  char v39; // [esp+281h] [ebp-4F7h]
  char v40; // [esp+282h] [ebp-4F6h]
  char v41; // [esp+283h] [ebp-4F5h]
  char v42; // [esp+284h] [ebp-4F4h]
  char v43; // [esp+285h] [ebp-4F3h]
  char v44; // [esp+286h] [ebp-4F2h]
  char a2; // [esp+290h] [ebp-4E8h]
  char v46; // [esp+291h] [ebp-4E7h]
  char v47; // [esp+292h] [ebp-4E6h]
  char v48; // [esp+293h] [ebp-4E5h]
  char v49; // [esp+294h] [ebp-4E4h]
  char v50; // [esp+295h] [ebp-4E3h]
  char v51; // [esp+296h] [ebp-4E2h]
  char v52; // [esp+297h] [ebp-4E1h]
  char v53; // [esp+298h] [ebp-4E0h]
  char v54; // [esp+299h] [ebp-4DFh]
  char v55; // [esp+29Ah] [ebp-4DEh]
  char v56; // [esp+29Bh] [ebp-4DDh]
  char v57; // [esp+29Ch] [ebp-4DCh]
  char v58; // [esp+29Dh] [ebp-4DBh]
  char v59; // [esp+29Eh] [ebp-4DAh]
  char v60; // [esp+29Fh] [ebp-4D9h]
  char v61; // [esp+2A0h] [ebp-4D8h]
  char v62; // [esp+2A1h] [ebp-4D7h]
  char v63; // [esp+2A2h] [ebp-4D6h]
  char v64; // [esp+2A3h] [ebp-4D5h]
  char v65; // [esp+2A4h] [ebp-4D4h]
  char v66; // [esp+2A5h] [ebp-4D3h]
  char v67; // [esp+2A6h] [ebp-4D2h]
  char v68; // [esp+2A7h] [ebp-4D1h]
  char v69; // [esp+2A8h] [ebp-4D0h]
  char v70; // [esp+2A9h] [ebp-4CFh]
  char v71; // [esp+2AAh] [ebp-4CEh]
  char v72; // [esp+2ABh] [ebp-4CDh]
  char v73; // [esp+2ACh] [ebp-4CCh]
  char v74; // [esp+2ADh] [ebp-4CBh]
  char v75; // [esp+2AEh] [ebp-4CAh]
  char v76; // [esp+2AFh] [ebp-4C9h]
  int v77; // [esp+2B8h] [ebp-4C0h]
  char v78; // [esp+2C4h] [ebp-4B4h]
  int v79; // [esp+33Ch] [ebp-43Ch]
  int a1a; // [esp+348h] [ebp-430h]
  char v81[1028]; // [esp+370h] [ebp-408h]
  int v82; // [esp+774h] [ebp-4h]
  int savedregs; // [esp+778h] [ebp+0h]

  sub_3B3986(&unk_480016);
  if ( (unsigned __int8)sub_3B2F31() )
  {
    sub_3B13DE(dword_47C004, (int)&v79, 4);
    if ( v79 < 0 || v79 > 1023 )
    {
      v5 = MessageBoxW(0, L"Error", L"Hacker", 0x10u);
      sub_3B2D0B(v7, v6, &v11 == &v11, v5, a1);
    }
    else
    {
      sub_3B13DE(dword_47C004, (int)v81, v79);
      v12 = v79;
      if ( (unsigned int)v79 >= 0x400 )
        sub_3B12AD();
      v81[v12] = 0;
      sub_3B173F(a1, (int)&v78);
      sub_3B22E8(a1, (int)&v78, (int)v81, v79);
      sub_3B3189(a1, (int)&v78, (int)&a1a);
      a2 = 0x2C;
      v46 = 0xF2u;
      v47 = 77;
      v48 = -70;
      v49 = 95;
      v50 = -80;
      v51 = -93;
      v52 = 14;
      v53 = 38;
      v54 = -24;
      v55 = 59;
      v56 = 42;
      v57 = -59;
      v58 = -71;
      v59 = -30;
      v60 = -98;
      v61 = 27;
      v62 = 22;
      v63 = 30;
      v64 = 92;
      v65 = 31;
      v66 = -89;
      v67 = 66;
      v68 = 94;
      v69 = 115;
      v70 = 4;
      v71 = 51;
      v72 = 98;
      v73 = -109;
      v74 = -117;
      v75 = -104;
      v76 = 36;
      v14 = 32;
      v15 = 38;
      v16 = 33;
      v17 = 57;
      v18 = 60;
      v19 = 69;
      v20 = 38;
      v21 = 56;
      v22 = 42;
      v23 = 20;
      v24 = 6;
      v25 = 10;
      v26 = 24;
      v27 = 51;
      v28 = 28;
      v29 = 7;
      v30 = 58;
      v31 = 27;
      v32 = 9;
      v33 = 6;
      v34 = 26;
      v35 = 1;
      v36 = 51;
      v37 = 4;
      v38 = 10;
      v39 = 0;
      v40 = 0;
      v41 = 20;
      v42 = 8;
      v43 = 18;
      v44 = 0;
      if ( str_cmp((int)&a1a, &a2, 32) )
      {
        v77 = (int)"Nice try buddy";
      }
      else
      {
        for ( i = 0; i < 30; ++i )
          *(&v14 + i) ^= v81[(signed int)i % v79];
        v77 = (int)&v14;
      }
      v79 = sub_3B3D14(v77);
      sub_3B25B8(dword_47C004, (int)&v79, 4);
      sub_3B25B8(dword_47C004, v77, v79);
      sub_3B37D3();
    }
  }
  else
  {
    v1 = MessageBoxW(0, L"Error", L"Error1", 0x10u);
    sub_3B2D0B(v3, v2, &v11 == &v11, v1, a1);
  }
  sub_3B2856(&savedregs, &dword_3B7A88, 0, v4);
  return sub_3B2D0B((unsigned int)&savedregs ^ v82, v9, 1, v8, a1);
}
```

Đây là đoạn code kiểm tra password, sau khi biến đổi password bằng giải thuật X nào đó,, chương trình sẽ kiểm tra X(password) với chuỗi có giá trị hex `2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824`. Tôi tìm kiếm chuỗi này trên google và nhận ra đây là giá trị SHA256 của chuỗi `hello`. Vậy giải thuật X ở trên chính là SHA256 và password đúng là `hello`.

![image](https://user-images.githubusercontent.com/17811861/119601851-34d61200-be14-11eb-92de-a306a26ac4f7.png)

Flag: `HCMUS-CTF{not_so_weird_hehexd}`