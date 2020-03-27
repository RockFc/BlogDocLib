#### 1. grep 命令

##### grep 命令用于在文本中执行关键词搜索，并显示匹配的结果，格式为“grep [选项]\[文件]”。    

###### 	grep 命令的参数及其作用如下：

```
-b	将可执行文件（binary）当做文本文件（text）来搜索
-c	仅显示找到的行数
-i	忽略大小写
-n	显示行号  ***常用***
-v	反向选择——仅列出没有“关键字”的行   ***常用***
```

这些参数中，-n 和 -v 这两个参数几乎能完成您日后 80% 的工作需要。

```c
//用grep命令来查找/etc/passwd 文件中包含“/sbin/nologin”的行
[root@bogon test]# grep -n /sbin/nologin /etc/passwd
2:bin:x:1:1:bin:/bin:/sbin/nologin
3:daemon:x:2:2:daemon:/sbin:/sbin/nologin
4:adm:x:3:4:adm:/var/adm:/sbin/nologin
5:lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
9:mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
10:operator:x:11:0:operator:/root:/sbin/nologin
//省略部分输出信息
```

#### 2. find 命令

##### find 命令用于按照指定条件查找文件，格式为 “find [查找路径] 寻找条件 操作”。

##### Linux中，搜索工作都是通过 find 命令来完成的，它可以使用不同的文件特性作为寻找条件（如文件名、大小、修改时间、权限信息），一旦匹配成功则默认将信息显示到屏幕上。

###### find 命令的参数及其作用如下：

```
-name	匹配名称
-perm	匹配权限
-user	匹配所有者
-group 	匹配所有组
-mtime -n +n	匹配修改内容的时间（-n 指 n 天以内， +n 指 n 天之前）
-atime -n +n	匹配访问文件的时间（-n 指 n 天以内， +n 指 n 天之前）
-ctime -n +n	匹配修改文件权限的时间（-n 指 n 天以内， +n 指 n 天之前）
-nouser			匹配无所有者的文件
-nogroup		匹配无所有组的文件
-newer f1 !f2	匹配比文件f1新但比f2旧的文件
--type b/d/c/p/l/f	匹配文件类型（块设备、目录、字符设备、管道、链接文件、文本文件）
-size			匹配文件的大小（+50KB查找超过50KB的文件，-50KB查找小于50KB的文件）
-prune			忽略某个目录
-exec ..... {}\; 后面可跟进一步处理搜索结果的命令
```

其中， -exec 参数用于把 find 命令搜索到的结果交由紧随其后的命令做进一步处理。

在 /etc 目录中，如果想要获取到该目录中所有以 host 开头的文件列表，可以执行如下命令：

```c
//常规搜索
[root@bogon test]# find /etc -name "host*" -print
/etc/selinux/targeted/active/modules/100/hostname
/etc/host.conf
/etc/hosts
/etc/hosts.allow
/etc/hosts.deny
/etc/hostname
//find命令 -exec ..... {}\;参数使用
//1.开始时，0611文件夹中只有test和findtest两个文件夹，并且test文件夹中有四个文件，findtest为空
[root@bogon 0611]# ls -R
.:
findtest  test

./findtest:

./test:
560_file  aaa.txt  aa.txt  a.txt
//2.执行find命令，将在test文件夹中找的的文件名以“a”打头的文件全部复制到findtest文件夹中
[root@bogon 0611]# find ./test -name "a*" -exec cp -a {} ./findtest/ \;
//3.再次查看文件夹，发现已经将查找到的文件都保存到了findtest中
[root@bogon 0611]# ls -R
.:
findtest  test

./findtest:
aaa.txt  aa.txt  a.txt

./test:
560_file  aaa.txt  aa.txt  a.txt
```










