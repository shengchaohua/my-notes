# 命令

## grep

在文件中查询一个关键字：

```shell
grep <keyword> file
```

## lsof

lsof (list open files)是一个列出当前系统打开文件的工具。

lsof 查看端口占用语法格式：

例如查看服务器 8000 端口的占用情况：

```Bash
lsof -i :8000
```

## ps

查看进程：ps

查看线程：ps/top -H

## netstat

netstat用于显示 tcp，udp 的端口和进程等相关情况。

netstat 查看端口占用语法格式：

参数描述：

- -t：仅显示tcp相关选项
- -u：仅显示udp相关选项
- -n 拒绝显示别名，能显示数字的全部转化为数字
- -l 仅列出在Listen(监听)的服务状态
- -p 显示建立相关链接的程序名

例如，查看 8000 端口的情况，可以使用以下命令：

```Bash
netstat -tlnp | grep 8000
```

## top

查看服务器性能、资源使用情况。



## df

## du

查看文件夹占用存储大小

```
su -sh <dir>
```



## kill

杀死进程：kill -s 9