#### 流程控制语句

尽管可以通过使用Linux命令、管道符、重定向以及条件测试语句编写最基本的Shell脚本，但是这种脚本并不适用于生产环境。原因是它不能根据真实的工作需求来调整具体的执行命令，也不能根据某些条件实现自动循环执行。

例如，我们需要批量创建 1000 为用户，首先要判断这些用户是否已经存在；若不存在，则通过循环语句让脚本一次创建他们。

##### 常用的有if、for、while、case这4种流程控制语句。

#### 1. if 条件测试语句 

##### 1.1 单分支结构

```shell
if	条件测试操作
	then	命令序列
fi
```
```shell
#shell脚本文件内容
[root@rockman 0619]# cat mkcdrom.sh
#!/bin/bash
DIR="/media/cdrom"
if [ ! -e $DIR ]
then
mkdir -p $DIR
fi
#执行脚本
[root@rockman 0619]# bash mkcdrom.sh
#查看执行结果
[root@rockman 0619]# ls  -l /media
total 0
drwxr-xr-x. 2 root root 6 Jun 19 11:10 cdrom
```

##### 1.2 多分支结构

```shell
if	条件测试操作1
	then	命令序列1
elif	条件测试操作2
	then	命令序列2
else
	命令序列3
fi
```
在Linux系统中，read是用来读取用户输入信息的命令，能够把接收到的用户输入信息赋值给后面的指定变量，-p参数用于向用户显示一定的提示信息。
```shell
[root@rockman 0619]# cat chkscore.sh
#!/bin/bash
read -p "Enter your score(0-100): " GRADE
if [ $GRADE -ge 85 ] && [ $GRADE -le 100 ]; then
echo "$GRADE is Excellent"
elif [ $GRADE -ge 70 ] && [ $GRADE -le 84 ]; then
echo "$GRADE is Pass"
else
echo "$GRADE is Fail"
fi
[root@rockman 0619]# bash chkscore.sh
Enter your score(0-100): 98
98 is Excellent
```

#### 2. for条件循环语句 

```shell
for	变量名	in	取值列表
do 
	命令序列
done
```

下面使用for循环语句从列表文件中读取多个用户名，然后为其逐一创建用户账号并设置密码。

首先创建用户名称列表，每个用户名单独一行。

```shell
[root@rockman 0619]# vim users.txt
[root@rockman 0619]# cat users.txt
bob
lily
```

然后编写Shell脚本，其中，/dev/null是一个被称作Linux黑洞的文件，把输出信息重定向到这个文件等同于删除数据（类似于没有回收功能的垃圾箱），可以让用户的屏幕保持简洁。

```shell
[root@localhost 0619]# cat Example.sh
#!/bin/bash
#批量添加用户
#输入用户密码（待创建的所有用户）
read -p "Enter The Users Password: " PASSWD
#对users.txt中的每一个用户名：检查用户名是否存在，如果存在，打印提示信息；如果不存在，则添加用户。添加成功或失败，都打印出错信息
for UNAME in `cat users.txt`
do
# &> 表示将标准输出与错误输出共同写入到文件中
id $UNAME &> /dev/null
if [ $? -eq 0 ]
then
echo "Already exists"
else
useradd $UNAME &> /dev/null
echo "$PASSWD" | passwd --stdin $UNAME &> /dev/null
if [ $? -eq 0 ]
then
echo "$UNAME, Create success"
else
echo "$UNAME, Create failure"
fi
fi
done
```

执行批量创建用户的Shell脚本。/etc/passwd是用来保存用户账号信息的文件。如果想确认这个脚本是否成功创建了用户账户，可以打开这个文件，看其中是否有这些新创建的用户信息。

```shell
[root@localhost 0619]# bash Example.sh
Enter The Users Password: 1
bob, Create success
lily, Create success
[root@localhost 0619]# tail -5 /etc/passwd
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
hk:x:1000:1000:hk:/home/hk:/bin/bash
bob:x:1001:1001::/home/bob:/bin/bash
lily:x:1002:1002::/home/lily:/bin/bash
```

#### 3. while条件循环语句

```shell
while	条件测试操作
do	
	命令序列
done
```
编写Shell脚本。
```shell
[root@localhost 0619]# cat Guess.sh
#!/bin/bash
PRICE=$(expr $RANDOM % 1000)
TIMES=0
echo "商品实际价格为0-999之间，猜猜看是多少？"
while true
do
read -p "请输入您猜测的价格数目：" INT
let TIMES++
if [ $INT -eq $PRICE ]; then
echo "恭喜你答对了，实际价格是 $PRICE"
echo "您共猜测了 $TIMES 次"
exit 0
elif [ $INT -gt $PRICE ]; then
echo "太高了！"
else
echo "太低了！"
fi
done
```

运行shell脚本

```shell
[root@localhost 0619]# bash Guess.sh
商品实际价格为0-999之间，猜猜看是多少？
请输入您猜测的价格数目：555
太高了！
请输入您猜测的价格数目：333
太高了！
请输入您猜测的价格数目：222
太低了！
请输入您猜测的价格数目：300
太高了！
请输入您猜测的价格数目：255
太高了！
请输入您猜测的价格数目：244
太低了！
请输入您猜测的价格数目：250
太高了！
请输入您猜测的价格数目：247
太高了！
请输入您猜测的价格数目：246
恭喜你答对了，实际价格是 246
您共猜测了 9 次
```

#### 4. case条件测试语句（略）

















