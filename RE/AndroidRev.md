File `FlagChecker` chứa đoạn code kiểm tra flag như sau:

```java
package com.hcmusctf.androidrev;

import android.content.Context;
import android.util.Base64;
import android.util.Log;
import java.security.MessageDigest;
import java.util.HashSet;
import java.util.Set;

public class FlagChecker {
    public static boolean checkFlag(Context ctx, String flag) {
        if (!flag.startsWith("HCMUS-CTF{") || !flag.endsWith("}")) {
            return false;
        }
        String core = flag.substring(10, 42);
        if (core.length() != 32) {
            return false;
        }
        String[] ps = core.split(foo());
        if (ps.length != 5 || !bim(ps[0]) || !bum(ps[2]) || !bam(ps[4]) || !core.replaceAll("[A-Z]", "X").replaceAll("[a-z]", "x").replaceAll("[0-9]", " ").matches("[A-Za-z0-9]+.       .[A-Za-z0-9]+.[Xx ]+.[A-Za-z0-9 ]+")) {
            return false;
        }
        char[] syms = new char[4];
        int[] idxs = {15, 23, 29, 34};
        Set<Character> chars = new HashSet<>();
        for (int i = 0; i < syms.length; i++) {
            syms[i] = flag.charAt(idxs[i]);
            chars.add(Character.valueOf(syms[i]));
        }
        int sum = 0;
        for (char c : syms) {
            sum += c;
        }
        if (sum != 180 || chars.size() != 1 || !m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct1), ctx.getString(C0289R.string.f61k1)), ps[0]), ctx.getString(C0289R.string.f68t1)) || !m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct2), ctx.getString(C0289R.string.f62k2)), ps[1]), ctx.getString(C0289R.string.f69t2)) || !m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct3), ctx.getString(C0289R.string.f63k3)), ps[2]), ctx.getString(C0289R.string.f70t3)) || !m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct4), ctx.getString(C0289R.string.f64k4)), ps[3]), ctx.getString(C0289R.string.f71t4)) || !m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct5), ctx.getString(C0289R.string.f65k5)), ps[4]), ctx.getString(C0289R.string.f72t5)) || !m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct6), ctx.getString(C0289R.string.f66k6)), flag), ctx.getString(C0289R.string.f73t6))) {
            return false;
        }
        return true;
    }

    private static boolean bim(String s) {
        return s.matches("^[a-z]+$");
    }

    private static boolean bum(String s) {
        return s.matches("^[A-Z]+$");
    }

    private static boolean bam(String s) {
        return s.matches("^[0-9]+$");
    }

    /* renamed from: dh */
    private static String m10dh(String hash, String s) {
        try {
            MessageDigest md = MessageDigest.getInstance(hash);
            md.update(s.getBytes());
            return toHexString(md.digest());
        } catch (Exception e) {
            return null;
        }
    }

    private static String toHexString(byte[] bytes) {
        StringBuilder hexString = new StringBuilder();
        for (byte b : bytes) {
            String hex = Integer.toHexString(b & 255);
            if (hex.length() == 1) {
                hexString.append('0');
            }
            hexString.append(hex);
        }
        return hexString.toString();
    }

    public static String foo() {
        String s = "Vm0wd2QyVkZNVWRYV0docFVtMVNWVmx0ZEhkVlZscDBUVlpPVmsxWGVIbFdiVFZyVm0xS1IyTkliRmRXTTFKTVZsVmFWMVpWTVVWaGVqQTk=";
        for (int i = 0; i < 10; i++) {
            s = new String(Base64.decode(s, 0));
        }
        return s;
    }

    /* renamed from: gs */
    private static String m11gs(String a, String b) {
        String s = "";
        for (int i = 0; i < a.length(); i++) {
            s = s + Character.toString((char) (a.charAt(i) ^ b.charAt(i % b.length())));
        }
        return s;
    }

    /* renamed from: me */
    private static boolean m12me(Context ctx, String s1, String s2) {
        try {
            return ((Boolean) s1.getClass().getMethod(m13r(ctx.getString(C0289R.string.f67m1)), new Class[]{Object.class}).invoke(s1, new Object[]{s2})).booleanValue();
        } catch (Exception e) {
            Log.e("HCMUS-CTF", "Exception: " + Log.getStackTraceString(e));
            return false;
        }
    }

    /* renamed from: r */
    public static String m13r(String s) {
        return new StringBuffer(s).reverse().toString();
    }
}
```

Flag phải bắt đầu băng `HCMUS-CTF{` và kết thúc bằng `}`, `flag.substring(10, 42)` phải có độ dài là `32`.

Hàm `foo()` decode base64 chuỗi `Vm0wd2QyVkZNVWRYV0docFVtMVNWVmx0ZEhkVlZscDBUVlpPVmsxWGVIbFdiVFZyVm0xS1IyTkliRmRXTTFKTVZsVmFWMVpWTVVWaGVqQTk=` 10 lần kết quả thu được là ký tự `-`. Có thể bỏ qua đoạn kiểm tra regex vì dùng các dữ kiện bên dưới vẫn có thể giải được. 

```java
char[] syms = new char[4];
int[] idxs = {15, 23, 29, 34};
Set<Character> chars = new HashSet<>();
for (int i = 0; i < syms.length; i++) {
    syms[i] = flag.charAt(idxs[i]);
    chars.add(Character.valueOf(syms[i]));
}
int sum = 0;
for (char c : syms) {
    sum += c;
}
```

Từ `sum == 180` và `chars.size() == 1` suy ra được rằng flag tại index `15`, `23`, `29` và `34` đều là ký tự `-`. Vậy flag phải có dạng `HCMUS-CTF{XXXXX-XXXXXXX-XXXXX-XXXX-XXXXXXX}`.

```java
!m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct1), ctx.getString(C0289R.string.f61k1)), ps[0]), ctx.getString(C0289R.string.f68t1)) ||
!m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct2), ctx.getString(C0289R.string.f62k2)), ps[1]), ctx.getString(C0289R.string.f69t2)) ||
!m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct3), ctx.getString(C0289R.string.f63k3)), ps[2]), ctx.getString(C0289R.string.f70t3)) ||
!m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct4), ctx.getString(C0289R.string.f64k4)), ps[3]), ctx.getString(C0289R.string.f71t4)) ||
!m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct5), ctx.getString(C0289R.string.f65k5)), ps[4]), ctx.getString(C0289R.string.f72t5)) ||
!m12me(ctx, m10dh(m11gs(ctx.getString(C0289R.string.ct6), ctx.getString(C0289R.string.f66k6)), flag), ctx.getString(C0289R.string.f73t6))
```

Strings:
```xml
<string name="ct1">xwe</string>
<string name="ct2">asd</string>
<string name="ct3">uyt</string>
<string name="ct4">42s</string>
<string name="ct5">p0X</string>
<string name="ct6">70 IJTR</string>
<string name="k1">53P</string>
<string name="k2">,7Q</string>
<string name="k3">8=A</string>
<string name="k4">yvF</string>
<string name="k5">=tm</string>
<string name="k6">dxa</string>
<string name="m1">slauqe</string>
<string name="search_menu_title">Search</string>
<string name="status_bar_notification_info_overflow">999+</string>
<string name="t1">6e9a4d130a9b316e9201238844dd5124</string>
<string name="t2">7c51a5e6ea3214af970a86df89793b19</string>
<string name="t3">e5f20324ae520a11a86c7602e29ecbb8</string>
<string name="t4">1885eca5a40bc32d5e1bca61fcd308a5</string>
<string name="t5">da5062d64347e5e020c5419cebd149a2</string>
<string name="t6">58150e58ae8a7275fcce5aea7d983ab5654f549cbeecedec27c89fe8246937d5</string>
```

Đoạn này lấy lần lượt `ct1` xor với `k1`, `ct2` xor với `k2`, `ct3` xor với `k3`,... sẽ ra được chuỗi `"MD5"`. Sau đó lấy lần lượt các chuỗi trong mảng `ps` tính MD5 và so sánh với các chuỗi `t1` đến `t6`. Search các chuỗi này trên trang `https://md5.gromweb.com/` sẽ thu được flag.

Flag: `HCMUS-CTF{peppa-9876543-BAAAM-A1z9-3133337}`