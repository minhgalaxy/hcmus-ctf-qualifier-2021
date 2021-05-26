Tác giả cung cấp file `mitm.pcapng`. Mở file này bằng Wireshark, sau đó chọn `File -> Export Objects -> HTTP` sẽ thấy có 3 file.

![image](https://user-images.githubusercontent.com/17811861/119674713-9fb03900-be66-11eb-9936-b09c16dc52d8.png)

Save 3 file này ra sẽ có 3 file `secret.zip`, `OTP.mp3` và `CheckPass.class`.
Dùng công cụ `jd-gui` decompile file `CheckPass.class` thu được source code như sau:

```java
import java.math.BigInteger;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Scanner;

public class CheckPass {
  public static void main(String[] paramArrayOfString) {
    Scanner scanner = new Scanner(System.in);
    System.out.println("Please enter the password!!");
    String str1 = scanner.nextLine();
    if (str1.length() != 8) {
      System.out.println("Oops!!");
      return;
    } 
    if (!str1.substring(0, 6).matches("[0-9]+")) {
      System.out.println("Oops!!");
      return;
    } 
    if (!str1.substring(0, 1).equals(str1.substring(5, 6))) {
      System.out.println("Oops!!");
      return;
    } 
    if (!str1.endsWith("}")) {
      System.out.println("Oops!!");
      return;
    } 
    String str2 = "(?![@',&])\\p{Punct}";
    if (!str1.substring(6, 7).matches(str2)) {
      System.out.println("Oops!!");
      return;
    } 
    if (!getMd5(str1).equals("53e443c9f65cd5f816452ae66ec65834")) {
      System.out.println("Oops!!");
      return;
    } 
    System.out.println("You get the second part!!!");
  }
  
  public static String getMd5(String paramString) {
    try {
      MessageDigest messageDigest = MessageDigest.getInstance("MD5");
      byte[] arrayOfByte = messageDigest.digest(paramString.getBytes());
      BigInteger bigInteger = new BigInteger(1, arrayOfByte);
      String str = bigInteger.toString(16);
      while (str.length() < 32)
        str = "0" + str; 
      return str;
    } catch (NoSuchAlgorithmException noSuchAlgorithmException) {
      throw new RuntimeException(noSuchAlgorithmException);
    } 
  }
}
```

Dựa vào source code này viết chương trình nho nhỏ để brute force password hợp lệ:

```java
package com.company;

import java.math.BigInteger;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

public class Main {

    public static void main(String[] args) {
        char[] arr = new char[]{'!', '"', '#', '$', '%', '(', ')', '*', '+', '-', '.', '/', ':', ';', '<', '=', '>', '?', '[', '\\', ']', '^', '_', '`', '{', '|', '}', '~'};
        char[] numbers = new char[]{'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
        for (char a : numbers) {
            for (char b : numbers) {
                for (char c : numbers) {
                    for (char d : numbers) {
                        for (char e : numbers) {
                            for (char f : arr) {
                                String str1 = "" + a + b + c + d + e + a + f + "}";
                                if (getMd5(str1).equals("53e443c9f65cd5f816452ae66ec65834")) {
                                    System.out.println("Found: " + str1);
                                    return;
                                }
                            }
                        }
                    }
                }
            }
        }

        System.out.println("Not found");
    }

    public static String getMd5(String paramString) {
        try {
            MessageDigest messageDigest = MessageDigest.getInstance("MD5");
            byte[] arrayOfByte = messageDigest.digest(paramString.getBytes());
            BigInteger bigInteger = new BigInteger(1, arrayOfByte);
            String str = bigInteger.toString(16);
            while (str.length() < 32)
                str = "0" + str;
            return str;
        } catch (NoSuchAlgorithmException noSuchAlgorithmException) {
            throw new RuntimeException(noSuchAlgorithmException);
        }
    }
}
```

Kết quả:
```
Found: 897268$}
```

Sử dung password `897268$}` giải nén file `secret.zip` được file `Part1.txt`, nối lại được flag `HCMUS-CTF{Just_Network_Stuff_897268$}`