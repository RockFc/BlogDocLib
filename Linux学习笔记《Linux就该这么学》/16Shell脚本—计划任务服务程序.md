#### 计划任务服务程序

尽管我们现在已近有了功能彪悍的脚本程序来执行一些批处理工作，但是，如果仍然需要每天凌晨两点敲击键盘回车键来执行这个脚本程序，这简直太痛苦了。为此，我们需要学习如何设置服务器的计划任务服务，把周期性、规律性的工作交给系统自动完成。

计划任务分为一次性计划任务与长期计划任务。

##### 1. 一次性计划任务——at命令

一次性任务只执行一次，一般用于满足临时的工作需求。可以使用at命令实现这种功能，值需要写成“at 时间”的形式就可以。

如果想要查看已设置好但还未执行的一次性任务，可以使用“at -l”命令；想要将其删除，可以用“atrm 任务序号”。

在使用at命令来设置一次性计划任务时，默认采用的是交互式方法。

```shell
#交互式（at中运行的命令要使用绝对路径）
[root@rockman 0620]# ls
[root@rockman 0620]# at now + 1 minutes
at> /bin/echo "Hello world! Hello wordcup!" > attest.txt
at> cp attest.txt atcopy.txt
at> <EOT>
job 8 at Wed Jun 20 09:32:00 2018
[root@rockman 0620]# ls -l
total 8
-rw-r--r--. 1 root root 28 Jun 20 09:32 atcopy.txt
-rw-r--r--. 1 root root 28 Jun 20 09:32 attest.txt
[root@rockman 0620]# cat attest.txt
Hello world! Hello wordcup!
[root@rockman 0620]# cat atcopy.txt
Hello world! Hello wordcup!
#非交互式
[root@rockman 0620]# echo '/bin/echo "I love Brazilian football team! I love Barzilian!">> attest.txt' | at now + 1 minutes
job 12 at Wed Jun 20 09:58:00 2018
[root@rockman 0620]# cat attest.txt
Hello world! Hello worldcup!
I love Brazilian football team! I love Barzilian!
```

##### 2.长期性计划任务——crond服务（略）

创建、编辑计划任务的命令为“crontab -e”；查看当前计划任务的命令为“crontab  -l”；删除某条计划任务的命令为“crontab -r”。另外，如果以管理员的身份登录系统，还可以在crontab命令中加上-u参数来编辑他人的计划任务。

使用crond服务设置任务参数格式为“分、时、日、月、星期  命令”。如果有些字段没有设置，则需要使用星号（*）占位。

假设小时都需要使用tar命令把当前目录打包处理，使其作为一个备份文件。就可以使用crontab -e命令来创建计划任务。

