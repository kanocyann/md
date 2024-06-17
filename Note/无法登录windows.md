一.

1.SHIFT加电源进入恢复界面,选择疑难解答中的高级选项,点击命令提示符

2.备份utilman.exe

```cmd
#第一步命令行中输入C:
C:
#第二步备份utilman,输入以下内容
copy c:\windows\system32\utilman.exe c:\windows\system32\utilman1.exe
#第三步cmd替换utilman
copy c:\windows\system32\cmd.exe c:\windows\system32\utilman.exe
#输入yes 提示已复制，该步骤完成
```

3.重启点击轻松使用按钮,会打开一个命令提示符窗口

```cmd
#输出当前用户名字(三选一)
- echo %username%
- whoami
- set
#将密码替换为无密码
net user <用户名> ""
```

