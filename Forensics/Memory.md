Phân tích file `memory.raw` bằng công cụ `volatility` bằng lệnh `filescan`, phát hiện có file `flag.txt` nằm trên Desktop. Tiến hành dump file `flag.txt` ra ngoài.

![image](https://user-images.githubusercontent.com/17811861/119672344-ba81ae00-be64-11eb-931b-3dd5d4f55d8b.png)

Xem file `file.None.0xfffffa8000659910.dat` sẽ được second part của secret:

```
Second part of secret_key: P@zzw0rD
```

Vậy là còn thiếu 1 nửa của secret nữa, sử dụng lệnh `volatility -f memory.raw --profile=Win7SP1x64 consoles` sẽ lấy được first part của secret key:

![image](https://user-images.githubusercontent.com/17811861/119673212-62977700-be65-11eb-8c37-cce6fbad1002.png)

Vậy secret key là: `SuP3r_P@zzw0rD`. Dựa vào hint ở trên tác giả để flag ở đâu đó online, có khả năng trong lịch sử trình duyệt có link dẫn tới file flag. Sử dụng plugin `chromehistory` xem lịch sử trình duyệt chrome, xuất hiện link `https://drive.google.com/file/d/1BBtY2q5h89Wkml6DLwlUSMJUUls3khtE/view `.

Tải file `flag.zip` này về và giải nén bằng password `SuP3r_P@zzw0rD` sẽ được flag `HCMUS-CTF{simple_memory_forensics_stuff}`