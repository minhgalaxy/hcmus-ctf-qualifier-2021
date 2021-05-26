Giải nén file `Evidences.zip` thu được một số thư mục như `.log`, `Desktop`, `Secret`, ...
Trong thư mục .log có chứa file `Master.png`, tuy nhiên 4 byte đầu đã bị sửa thành `\x89FIT` trong khi 4 byte đầu của PNG có dạng `\x89PNG`. Dùng công cụ `HxD` sửa 4 byte này lại sẽ có hint.

![Master](https://user-images.githubusercontent.com/17811861/119678535-cc198480-be69-11eb-9d97-f08b05a71b9a.png)

Hơn nữa, các file trong thư mục log đều có dạng

```
Send To: Kevin
From: Luke
Content: 
9137e8d598e02037bc931442abd4112a38b7af5ae53504bf982608bec418b16a
cb5a70bab06f43e49ef5b5a62783bc1df2679bdd262a5cb448fe1e8fb9089b1b
4a98c736b1a78198d36edbe83a984978881b80b76842af78979cc557b2a1be7b
371d9e144138e25dad171b604d93a4fbe3c3cd1277b8d7f7b9672b0ada9d7e22
ae35601ead8257bd92e96cb9a4879a4f9fdeced1b0a79b49e2a2a288c85e7def
206578069944df443ddc9e52092890e3484c76f04a758c0210b87412e745af60
00bba0f395a426236218207a5e30e59bf600d43973eff5d3346630deec9e4cd1
61dbc71f4b4036136da3db90273c425c8d6bebe6ae0fa2247d60b48a4b982b7a
b29ef2e6ddb8c4982c03620e1c5c709c862d44c768a092f516e3e7deae21e4fb
37a8630556dcb1eabdf3724ace8d3d8c3be877008a8d6c22c1246eecbaebeff3
76011fd2d58cca9f4ac8a1d51d3fd9b5a1a6a1d892e8f65812cf62278b5cb89c
d5f9f539ab3bda61f907c05da6b8debf116112294aeb03f8c6a887b999db8218
e850c17a38c1d8ca2e1fa06126b05d8aa743209be75d7f2ce589899d7b761941
21a07c386298dfc69d60197b5c702d37e81e5b16daf5adffcdec4755bf2c9073
40e153d515a2ca67a6c03ce6f4f1df0a3c276238c78799fd755125d5987ac62b
0c98017a6293e918dd5c4eaea5b2de52169577e7f4fc0f2721c018224a19e479
e34972d065a0ceb7125b36216b353e2a5a23fcfead5181164a6b5b35e927b615
a44d482798403bfb95e6157412fe640e3f6eb3539f14e7cacc197ae9ae66883a
cf05f6dc64fea0691792537ea5e6828355efdb8fe00c883dc2306bb3f434664a
f8bb78786be5184e3615bceaededbb0cdece9797389677328d53972e1282706a
eec1491f80f5ba0c2c21b99d444fe48ccdb8e4ec542d8ad105adb03bb2699b87
0d3a103f698861e977ea51d008de60d5611b84e3a40b87c69fd5a9785d5fbb66
8b5deea3d25504d06482058d3b08947c83b6596cab23450940c098e268a53200
233e7088fe6059201cca6e629fa5f2caff97c80ea640f0248de0a7775c4ed834
e1fbc519b1eedf9a6f6d4d90cb9e30c014127cf4baf8d9baab4e139d035d08c3
301aa3c2dff830d265a81b0489a0f063a0a6f7c49d3f9a7f005043caab5fa13c
0151dbed9ee88cb456a4aa36e64a288bd248c410d70135748cf5b32463bde3ec
18ddf329991935dada80e60ab49a85bd0a70c51f37dfb994f05b15c9f44de725
afde27c43461abf55fe6ac3511b25e8c6eaa73b459f0f6ea65ef43b0d9541759
89729188209873e9518089fe548d73a85db6a3edc53e579de0c7000ba756d70f
```

Dựa vào hint trong `Master.png`, tìm kiếm chuỗi 

```
Send to: Ronaldo
From: Messi
```

sẽ thấy 1 file có content base64 như sau:

![image](https://user-images.githubusercontent.com/17811861/119679126-4cd88080-be6a-11eb-9ab5-fe6c967a6faf.png)

Giải mã base64 `U3VQZXJfR29sZF8zeW1BckpyLg==` sẽ được password `SuPer_Gold_3ymArJr.`, đây là mật khẩu để trích xuất flag bằng `steghide`.

Code 1 đoạn script python để thử password này với tất cả file trong thư mục `Secret`:

```python
import glob
import os

arr = glob.glob('Secret/*')


for path in arr:
	os.system('steghide extract -p SuPer_Gold_3ymArJr. -sf ' + path)
```
Kết quả thu được file `flag.txt` chứa flag.

Flag: `HCMUS-CTF{at_least_I_hope_you_can_code_a_bit}`