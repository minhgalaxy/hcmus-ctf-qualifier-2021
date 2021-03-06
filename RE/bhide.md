Sử dụng `dnspy` decompile file `BHide.exe` thu được source code:

```C#
using System;
using System.Drawing;
using System.Drawing.Imaging;
using System.IO;
using System.Runtime.InteropServices;

namespace BHide
{
	// Token: 0x02000002 RID: 2
	internal class Program
	{
		// Token: 0x06000001 RID: 1 RVA: 0x00002050 File Offset: 0x00000250
		private static long gfs(string zz)
		{
			return new FileInfo(zz).Length;
		}

		// Token: 0x06000002 RID: 2 RVA: 0x0000205D File Offset: 0x0000025D
		private static Bitmap gbfp(string zz2)
		{
			return new Bitmap(zz2);
		}

		// Token: 0x06000003 RID: 3 RVA: 0x00002068 File Offset: 0x00000268
		private static byte[] gbfb(Bitmap zsh)
		{
			Rectangle rect = new Rectangle(0, 0, zsh.Width, zsh.Height);
			BitmapData bitmapData = zsh.LockBits(rect, ImageLockMode.ReadOnly, PixelFormat.Format24bppRgb);
			int num = bitmapData.Stride * bitmapData.Height;
			byte[] array = new byte[num];
			Marshal.Copy(bitmapData.Scan0, array, 0, num);
			zsh.UnlockBits(bitmapData);
			return array;
		}

		// Token: 0x06000004 RID: 4 RVA: 0x000020C4 File Offset: 0x000002C4
		private static byte asdf(byte n, int ll, int vv)
		{
			byte result;
			if (vv == 0)
			{
				result = (n & ~(byte)(1 << (int)((byte)ll)));
			}
			else
			{
				result = (byte)(1 << (int)((byte)ll) | (int)n);
			}
			return result;
		}

		// Token: 0x06000005 RID: 5 RVA: 0x000020F1 File Offset: 0x000002F1
		private static byte[] rmrf(string zas)
		{
			return File.ReadAllBytes(zas);
		}

		// Token: 0x06000006 RID: 6 RVA: 0x000020F9 File Offset: 0x000002F9
		private static bool cs(long sz1, long sz2)
		{
			return sz1 * 8L <= sz2;
		}

		// Token: 0x06000007 RID: 7 RVA: 0x00002108 File Offset: 0x00000308
		private static void h(byte[] sr, byte[] bb)
		{
			int num = 0;
			int num2 = 0;
			for (int i = 0; i < sr.Length; i++)
			{
				for (int j = 7; j >= 0; j--)
				{
					int vv = sr[i] >> j & 1;
					byte b = Program.asdf(bb[num2], num, vv);
					bb[num2++] = b;
					num ^= 1;
				}
			}
		}

		// Token: 0x06000008 RID: 8 RVA: 0x0000215C File Offset: 0x0000035C
		private static void rb(Bitmap zzmb, byte[] bbb)
		{
			Rectangle rect = new Rectangle(0, 0, zzmb.Width, zzmb.Height);
			BitmapData bitmapData = zzmb.LockBits(rect, ImageLockMode.WriteOnly, PixelFormat.Format24bppRgb);
			IntPtr scan = bitmapData.Scan0;
			Marshal.Copy(bbb, 0, scan, bbb.Length);
			zzmb.UnlockBits(bitmapData);
		}

		// Token: 0x06000009 RID: 9 RVA: 0x000021A8 File Offset: 0x000003A8
		private static void Main(string[] args)
		{
			if (args.Length != 2)
			{
				Console.WriteLine("[+] Usage: bhide.exe <file1> <bitmap_file>");
				return;
			}
			string text = args[0];
			string text2 = args[1];
			if (!File.Exists(text) || !File.Exists(text2))
			{
				Console.WriteLine("[+] One of two files doesn't exist");
				return;
			}
			Bitmap bitmap = Program.gbfp(text2);
			byte[] array = Program.gbfb(bitmap);
			if (!Program.cs(Program.gfs(text), (long)array.Length))
			{
				Console.WriteLine("[+] Invalid size");
				return;
			}
			Program.h(Program.rmrf(text), array);
			Program.rb(bitmap, array);
			bitmap.Save("bhide-" + text2);
			Console.WriteLine("[+] Done");
		}
	}
}

```

Bài này ý tưởng giống LSB nhưng có nâng cấp thêm 1 chút xíu. Bình thường LSB sẽ ghi bit cần chèn vào bit cuối, nhưng bài này sẽ lưu so le giữa bit cuối và bit gần cuối bằng cách xor biến `num` với 1 sau mỗi vòng lặp.

```java
// Token: 0x06000007 RID: 7 RVA: 0x00002108 File Offset: 0x00000308
private static void h(byte[] sr, byte[] bb)
{
	int num = 0;
	int num2 = 0;
	for (int i = 0; i < sr.Length; i++)
	{
		for (int j = 7; j >= 0; j--)
		{
			int vv = sr[i] >> j & 1;
			byte b = Program.asdf(bb[num2], num, vv);
			bb[num2++] = b;
			num ^= 1;
		}
	}
}


// Token: 0x06000004 RID: 4 RVA: 0x000020C4 File Offset: 0x000002C4
private static byte asdf(byte n, int ll, int vv)
{
	byte result;
	if (vv == 0)
	{
		result = (n & ~(byte)(1 << (int)((byte)ll)));
	}
	else
	{
		result = (byte)(1 << (int)((byte)ll) | (int)n);
	}
	return result;
}
```

Chương trình dùng để trích xuất file đã được ẩn trong file `bhide-sample.bmp`:

```C#
using System;
using System.Collections;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Imaging;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace BitmapDecode
{
    class Program
    {
		private static byte[] BitmapToBytes(Bitmap zsh)
		{
			Rectangle rect = new Rectangle(0, 0, zsh.Width, zsh.Height);
			BitmapData bitmapData = zsh.LockBits(rect, ImageLockMode.ReadOnly, PixelFormat.Format24bppRgb);
			int num = bitmapData.Stride * bitmapData.Height;
			byte[] array = new byte[num];
			Marshal.Copy(bitmapData.Scan0, array, 0, num);
			zsh.UnlockBits(bitmapData);
			return array;
		}

		public static byte[] GetBytesFromBinary(string data)
		{
			List<byte> byteList = new List<byte>();

			for (int i = 0; i < data.Length; i += 8)
			{
				byteList.Add(Convert.ToByte(data.Substring(i, 8), 2));
			}
			return byteList.ToArray();
		}


		static void Main(string[] args)
        {
			

			Bitmap bmpHiden = new Bitmap(@"D:\CTF\HCMUS\BHide\bhide-sample.bmp");
            byte[] bytes = BitmapToBytes(bmpHiden);
            StringBuilder sb = new StringBuilder();
			int num = 0;
			for (int i = 0; i < bytes.Length; i++)
			{
                byte b = bytes[i];
				if(num == 0)
                {
					byte r = (byte)(b & 1);
					sb.Append(r);
                } 
				else
                {
					byte r = (byte)((b & 3) >> 1);
					sb.Append(r);
                }
				num ^= 1;
			}
			File.WriteAllBytes("flag.jpg", GetBytesFromBinary(sb.ToString()));
		}
	}
}

```

![flag](https://user-images.githubusercontent.com/17811861/119651573-5a344180-be4f-11eb-8389-b64e2b6fb6ea.jpg)

Flag: `HCMUS-CTF{i_dont_use_LSB}`