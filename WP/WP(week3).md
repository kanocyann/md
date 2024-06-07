# 1.[极客大挑战 2019]BuyFlag

```PHP
本题打开之后点开MENU发现PAYFLAG,然后提示需要花一亿money买flag。
F12发现源码，需要对password变量进行绕过，弱比较传参404a，同时上交money等于100000000，提交POST请求。
回显不是学生，抓包发现Cookie:user=0关键属性,尝试改成1,发现成功绕过。
但是显示数字太长,换用科学计数法1e10,得到flag。
```



# 2.[护网杯 2018]easy_tornado

