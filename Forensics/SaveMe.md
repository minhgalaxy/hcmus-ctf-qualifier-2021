File `text` chứa hex dump của 1 file nào đó, tuy nhiên nhiều dòng đã bị hỏng. Để recover file này, code 1 script python như sau:

```python
import re

def convert(s):
	return s.decode('hex')[::-1]

def set_char(addr, chars):
	global result
	result[addr] = chars[0]
	result[addr + 1] = chars[1]

lines = open('text', 'r').read().splitlines()


result = bytearray('\x00' * 0x5c456)
for line in lines:
	arr = line.split(' ')
	if len(arr) == 9:
		arr = line.split(' ')
		addr, a, b, c, d, e, f, g, h = arr
		addr = int(addr, 16)
		set_char(addr + 0, convert(a))
		set_char(addr + 2, convert(b))
		set_char(addr + 4, convert(c))
		set_char(addr + 6, convert(d))
		set_char(addr + 8, convert(e))
		set_char(addr + 10, convert(f))
		set_char(addr + 12, convert(g))
		set_char(addr + 14, convert(h))


open('out', 'w').write(result)
```

Check file type của file `out` thì đây là file `JPEG`
```
out: JPEG image data, JFIF standard 1.01, resolution (DPI), density 300x300, segment length 16, Exif Standard: [TIFF image data, little-endian, direntries=7, orientation=upper-left, xresolution=98, yresolution=106, resolutionunit=2, software=GIMP 2.10.22, datetime=2021:04:26 10:56:13], progressive, precision 8, 1024x768, frames 3
```

Đổi đuôi `out` thành `out.jpg` sẽ có flag.

![out2](https://user-images.githubusercontent.com/17811861/119676002-b6a35b00-be67-11eb-959b-4b9aa7d5061b.jpg)

Flag: `HCMUS-CTF{You_Know_How_To_Manipulate_Images_1324587}`