# 1.SimpleRev

放到脱壳软件中发现是64位程序,用IDA64打开后筛选字串关键字,发现"**Please input your flag:**"字样,进入后F5展示伪C代码,有几个十六机制数字(小端顺序排布),逆转并转换为ascii码后得到几个关键变量

src=NDCLS	V9=hadow

到内存中寻找剩下几个关键变量

key3=kills	  key1=ADSFK

按代码拼接	text=killshadow	key=ADSFKNDCLS

审计代码后写出反加密代码

```python
text = 'killshadow'
key = 'adsfkndcls'
v3 = 0
v5 = len(key)
n = 0
flag = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
for i in range(0, 10):
    for j in range(0, 10):
        v1 = (ord(text[j]) - 97) + 26*i + ord(key[v3%v5]) - 58
        if (v1>=65 and v1<=90) or (v1>=97 and v5<=122):
            flag[j] = chr(v1)
            n += 1
            if (n==10):
                flag = ''.join(flag)
                print(flag)
                break
        v3 = v3 + 1
```



# 2.Java逆向解密

附件是一个java的class,用jd-gui工具打开

```java
import java.util.ArrayList;
import java.util.Scanner;

public class Reverse {
  public static void main(String[] args) {
    Scanner s = new Scanner(System.in);
    System.out.println("Please input the flag );
    String str = s.next();
    System.out.println("Your input is );
    System.out.println(str);
    char[] stringArr = str.toCharArray();
    Encrypt(stringArr);
  }
  
  public static void Encrypt(char[] arr) {
    ArrayList<Integer> Resultlist = new ArrayList<>();
    for (int i = 0; i < arr.length; i++) {
      int result = arr[i] + 64 ^ 0x20;
      Resultlist.add(Integer.valueOf(result));
    } 
    int[] KEY = { 
        180, 136, 137, 147, 191, 137, 147, 191, 148, 136, 
        133, 191, 134, 140, 129, 135, 191, 65 };
    ArrayList<Integer> KEYList = new ArrayList<>();
    for (int j = 0; j < KEY.length; j++)
      KEYList.add(Integer.valueOf(KEY[j])); 
    System.out.println("Result:");
    if (Resultlist.equals(KEYList)) {
      System.out.println("Congratulations);
    } else {
      System.err.println("Error);
    } 
  }
}
```

重点是Encrypt处理函数,使用了异或加密,先将字符的ascii码值加上64,再与0x20(十进制32)按位异或得到密文,所以要对key进行逆向,python代码如下

```python
KEY = [180, 136, 137, 147, 191, 137, 147, 191, 148, 136, 133, 191, 134, 140, 129, 135, 191, 65]
flag = ""
for i in range(len(KEY)):
    flag += chr((KEY[i]^32) - 64)
print(flag)
```

#### Hint:按位异或^的计算优先级低于加法运算,所以原式按照从左到右计算



## 3.[BJDCTF2020]JustRE

用ida打开附件后寻找字串,发现flag格式语句BJD{%d%d2069a45792d233ac},定位到内存位置并转换为伪C代码后审计发现

```
sprintf(String, " BJD{%d%d2069a45792d233ac}", 19999, 0);
```

进行了替换,所以答案是flag{1999902069a45792d233ac}



# 4.[GXYCTF2019]luck_guy

使用ida64打开程序后寻找子串,发现一串**GXY{do_not_**类似flag字样,先复制待处理,寻找到主函数审计代码,发现这一串是flag的前一部分,后半段与变量s有关,s是一段小端储存的数据,将其逆转并且转换为字符串得到后半部分,拼接得到答案

```python
s = 0x7F666F6067756369LL
s = [0x7F,0x66,0x6F,0x60,0x67,0x75,0x63,0x69][::-1]
key1 = 'GXY{do_not_'
flag = key1
for i in range(0,len(s)):
    x = s[i]
    if (i % 2 == 1):
        x = x - 2
    else:
        x = x -1
    flag = flag+chr(x)
print(flag)
```

