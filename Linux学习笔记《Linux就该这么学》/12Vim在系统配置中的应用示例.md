#### Vim 在系统配置中的应用示例

#### 1. 配置主机名称

##### 为了便于咱局域网中查找某台特定的主机，后者对主机进行区分，除了要有IP地址外，还要为主机配置一个主机名，主机名之间可以通过这个类似于域名的名称来相互访问。
 在Linxu系统中，主机名大多保存在/etc/hostname文件中。

```shell
//使用vim修改/etc/hostname中的内容
[root@rockman ~]# vim /etc/hostname
[root@rockman ~]# cat /etc/hostname
rockman.com
[root@rockman ~]# hostname
rockman.com
```

#### 2. 配置网卡信息

##### 第一步：首先切换到 /etc/sysconfig/network-scripts 目录中（存放着网卡的配置文件）。

##### 第二步：使用Vim编辑器修改网卡文件ifcfg-ens33，由于每台设备的硬件及架构是不一样的，所有需要使用ifconfig命令确认各自网卡的默认名称，然后进行相应的修改。

```shell
YPE=Ethernet
BOOTPROTO=dhcp
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=1a497aa0-f2c5-449d-96ed-e15df4ddf8b3
DEVICE=ens33
ONBOOT=yes
```

##### 第三步：重启网络服务并测试网络是否联通。

```shell
[root@rockman network-scripts]# systemctl restart network
[root@rockman network-scripts]# ping baidu.com
PING baidu.com (123.125.115.110) 56(84) bytes of data.
64 bytes from 123.125.115.110 (123.125.115.110): icmp_seq=1 ttl=128 time=5.48 ms
64 bytes from 123.125.115.110 (123.125.115.110): icmp_seq=2 ttl=128 time=5.47 ms
64 bytes from 123.125.115.110 (123.125.115.110): icmp_seq=3 ttl=128 time=6.71 ms
64 bytes from 123.125.115.110 (123.125.115.110): icmp_seq=4 ttl=128 time=5.42 ms
^C
--- baidu.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
```

#### 3. 配置 Yum 软件仓库

##### Yum（Yellow dog Updater, Modified）软件仓库的作用是为了进一步简化RPM（Red Hat Package Manager）管理软件的难度以及自动分析所需软件包及其以来关系的技术。

可以把Yum想象成一个硕大的软件仓库，里面保存有几乎所有的常用工具，而且只需要说出所需的软件包名称，系统就会为你搞定一切。

##### 第一步： 进入到/etc/yum.repos.d目录中（应为该目录存放着Yum软件仓库的配置文件）。

```shell
[root@rockman network-scripts]# cd /etc/yum.repos.d/
[root@rockman yum.repos.d]# ls
CentOS-Base.repo  CentOS-Debuginfo.repo  CentOS-Media.repo    CentOS-Vault.repo
CentOS-CR.repo    CentOS-fasttrack.repo  CentOS-Sources.repo
```

##### 第二步：使用vim编辑器创建一个名为rhel7.repo的新配置文件（文件名称可随意，但后缀必须为.repo），逐项配置参数并保存退出。（本例中只打开一个.repo文件查看）

```shell
[root@rockman yum.repos.d]# vi CentOS-Media.repo
[c7-media]
name=CentOS-$releasever - Media
baseurl=file:///media/CentOS/
        file:///media/cdrom/
        file:///media/cdrecorder/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

##### 第三步：按配置参数的路径挂载光盘，并把光盘挂载信息写入到/etc/fstab文件中。

```shell
[root@linuxprobe yum.repos.d]# mkdir -p /media/cdrom
[root@linuxprobe yum.repos.d]# mount /dev/cdrom /media/cdrom
mount: /dev/sr0 is write-protected, mounting read-only
[root@linuxprobe yum.repos.d]# vim /etc/fstab
/dev/cdrom /media/cdrom iso9660 defaults 0 0
```

##### 第四步：使用“yum install httpd -y”命令检查Yum软件 仓库是否已经可用。

```shell
[root@rockman network-scripts]# yum install httpd -y
Loaded plugins: fastestmirror
base                                                     | 3.6 kB     00:00
extras                                                   | 3.4 kB     00:00
updates                                                  | 3.4 kB     00:00
(1/2): extras/7/x86_64/primary_db                          | 149 kB   00:00
(2/2): updates/7/x86_64/primary_db                         | 2.0 MB   00:01
Loading mirror speeds from cached hostfile
（省略部分输出信息）
Complete!
```

 







