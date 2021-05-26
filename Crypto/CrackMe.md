Giải nén file `phase1.zip`, thu được 3 file: **phase2.zip**, **hashed** và **README.md**. 

Nội dung file README.md:

```
Crack that hashed to plaintext. The base64 of the plaintext is the password to open the zip file.
```

Theo như **README.md**, mục tiêu cần làm là crack hash nằm trong file **hashed** sau đó base64 của plaintext chính là mật khẩu file của file **phase2.zip**.

Crack file **hashed** bằng tool John the Ripper, sử dụng word list là `rockyou.txt`.

![image](https://user-images.githubusercontent.com/17811861/119666172-50b2d580-be5f-11eb-981c-c66a8df3abfd.png)

Vậy plaintext là `playboy123`.

=> Base64 của plaintext là  `cGxheWJveTEyMw==`, sử dụng password này sẽ giải nén được file **phase2.zip**. Nhận được 3 file mới là **flag.zip**, **id_rsa** và **README.md**.

Nội dung file README.md:

```
Crack it to find the passphrase. The password to opened the zip file is the base64 of that passphrase.
```

Tiếp tục sử dụng tool John the Ripper để crack password file `id_rsa`, nhưng trước tiên cần phải convert file `id_rsa` thành dạng hash thì tool `John the Ripper` mới đọc và crack được.

```bash
python ssh2john.py id_rsa > id_rsa.hash
```

Sau đó crack hash trong file `ida_rsa.hash`

```bash
john --wordlist=rockyou.txt id_rsa.hash
```

Password crack được là `felecity`, base64 là `ZmVsZWNpdHk=`. Có password giải nén file **flag.zip** được file flag.txt chứa flag.

Flag: `HCMUS_CTF{cracking_for_fun}`