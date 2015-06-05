[GitHubBlog](https://github.com/bbxytl/bbxytl.github.com/tree/master/blog#home--githubblog) /
=====
#[CentOS7安装使用minimal模式记录](https://github.com/bbxytl/bbxytl.github.com/blob/master/blog/pages/150530_CentOS7安装使用minimal模式记录.md#githubblog-)
---

[TOC]

安装使用 **minimal** 模式。安装后使用解决方案：
### 1. 安装完成后第一次使用不能上网， ping 不通。解决方案： 
---
```shell
# 切换到**root**下输入命令：
~$ su  		# 回车后输入root密码
~$ echo "nameserver 8.8.8.8" >> /etc/resolv.conf 	# 设置 DNS
# 使用 ip addr 查看网卡名称，如: ens33
~$ ip addr 	# CentOS minimal 模式下默认没有 ifconfig 命令
~$ cd /etc/sysconfig/network-scripts/
~$ ls 		# 查看刚才看到的网卡名称对应的文件，如这里应该是 ifcfg-ens33
~$ vi ifcfg-ens33
# 打开后查看最后一项： "ONBOOT=no"
# 按“i”键进入编辑状态，将最后一行“no”修改为“yes”，然后按“ESC”键退出编辑状态，并输入“:x”保存退出。
~$ service network restart  	# 重启服务
~$ ip addr 	# 此时可查看到已经有 IP 地址了。
~$ ping www.baidu.com			# 此时能 ping 通，yum install 命令可以使用了
```
可查看：[如何设置CentOS 7获取动态及静态IP地址](http://www.ytyzx.net/index.php?title=%E5%A6%82%E4%BD%95%E8%AE%BE%E7%BD%AECentOS_7%E8%8E%B7%E5%8F%96%E5%8A%A8%E6%80%81%E5%8F%8A%E9%9D%99%E6%80%81IP%E5%9C%B0%E5%9D%80)
### 2. 给虚拟机安装增强功能： 
---
```shell
# Vbos的“设备->安装增强功能”
~$ yum -y update kernel*
~$ yum -y install bzip2 gcc make kernel-devel	# 这个是用来防止运行过程中出现命令找不到的情况
~$ yum -y upgrade kernel kernel-devel kernel-headers
~$ reboot
~$ mkdir /media/cdrom	# 新建一个目录用来映射加载后的CD
~$ mount /dev/cdrom /media/cdrom
~$ export KERN_DIR=/usr/src/kernels/`uname -r`
~$ cd /media/cdrom
~$ ./VBoxLinuxAdditions.run
# 安装完后即可使用共享文件夹等，设置好共享文件夹记得执行：
~$ reboot
~$ df  			# 查看是否挂载
```
可查看:[centos mini安装virtualbox增强功能](http://blog.sina.com.cn/s/blog_7deb436e0101w8n9.html)
### 3. 给非root用户添加使用 sudo 的权限 
---
```shell
~$ su		# 切换到 root 账号
~$ vi /etc/sudoers
# 打开后找到这一行： root    ALL=(ALL)       ALL
# 在下面添加一行： yourusername ALL=(ALL)       ALL
# 需要添加几个账号就添加几个。然后切换到 yourusername 下就可以使用 sudo 了
```

### 4. 使用 Xshell5 远程登陆 CentOS 
---
- **下载链接：[http://pan.baidu.com/s/1gdpNseJ](http://pan.baidu.com/s/1gdpNseJ) 密码: ` ybkh `**
- 安装后新建一个会话，在CentOS中使用命令 `ip addr` 查看 ip 地址；
- 在新建的会话中填入 ip 地址，端口号默认使用 22 ;
- 确认后连接，输入用户名和密码即可连接；
- 具体可查看：[Xshell连接linux（图文教程）](http://jingyan.baidu.com/article/3a2f7c2e71f04d26afd611dc.html)

### 5. 使用共享文件夹
---
此功能的使用需要先安装 VBox的增强工具，具体查看**[第二部分](#2. 给虚拟机安装增强功能：)**。
- 设置->共享文件夹->添加一个固定分配的共享文件夹（要在关机状态下才能设置，否则设置的是临时分配的，下次开机就没了~~），选中自动挂载。这里假如挂载的文件夹名是 **VBoxShare** 。
- 重启后使用如下命令：
```shell
~$ sudo mkdir /mnt/WinShare		# 创建一个用于挂载的文件夹
~$ cd /							# 切换到根目录
~$ sudo mount -t vboxsf VBoxShare /mnt/WinShare	# 挂载
# 在当前用户目录下建立一个软链接
~$ ln -s /mnt/WinShare /home/yourusername/WinShare	
~$ ls							# 查看是否成功
```


### 6. 使用命令
---
CentOS7中安装命令需要使用 sudo ：

- **6.1 安装 ifconfig 命令**
```shell
# 正常使用命令： yum install ifconfig 后显示找不到包
~$ yum search ifconfig		# 查找包含 ifconfig 命令的包，正常结果是net-tools.x86_64
~$ yum install net-tools.x86_64	# 安装后即可使用 ifconfig
```
- **6.2 使用 tar 命令**
使用 tar 命令解压时默认要先切换到要解压文件的所在目录下。否则会出错！如**[“Cannot open: No such file or directory” when extracting a tar file](http://askubuntu.com/questions/486264/cannot-open-no-such-file-or-directory-when-extracting-a-tar-file)**。
- **6.3 安装 locate 命令**
```shell
~$ yum search locate	# 查找包 这里查找到： mlocate.x86_64
~$ yum install mlocate.x86_64
# locate 是在文件索引数据库中查找，所以有时需要更新文件索引数据库
~$ updatedb 	# 更新数据库 ， 需要 root 权限
```
- **6.4 按照 CMake 命令**
```shell
# CentOS7 中使用下面的指令按照 cmake :
~$ yum install cmake
~$ cmake --version
cmake version 2.8.11
```
	-  这里默认安装的是 2.8.11 的版本，版本太低了，官网已经都到 3.x 系列了，所以更新一下：
	- 到**[官网](http://www.cmake.org/download/)**去下载最新的 cmake 压缩包，比如：cmake-3.2.3.tar.gz 
	- 解压到任意文件夹下，运行以下指令：(运行方法在 解压后目录下的 README.rst（MarkDown文件） 文件里，可打开查看)
```shell
~$ ./bootstrap 
~$ make 
~$ make install
# 安装成功后可查看：
~$ cmake --version
cmake version 3.2.3
```
	- 可能会出现 无法创建文件夹的情况，记得用 sudo 就好。时间比较长~~~
- **6.5 安装 pip 命令**
```shell
~$ rpm -iUvh http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
~$ sudo yum install epel-release
~$ sudo yum install python-pip
# 测试是否安装成功
~$ whereis pip
pip: /usr/bin/pip2.7 /usr/bin/pip
```


### 7. 配置Vim
---
首先在minimal模式下默认并没有安装 vim ，只有 vi。所以要先进行安装：
```shell
~$ yum -y install vim
```
#### 7.1 给**vim**添加中文帮助文档：
```shell
# 使用 wget 命令 下载中文帮助文档 -O 选项用来重命名要下载的文件
~$ wget -O /home/viki/WinShare/vimdoczh.tar.gz http://sourceforge.net/projects/vimcdoc/files/vimcdoc/1.8.0/vimcdoc-1.8.0.tar.gz/download
~$ cd /home/viki/WinShare
~$ ls
vimdoczh.tar.gz
~$ tar -zxvf vimdoczh.tar.gz		# 参考 第 6 条
~$ ls
vimcdoc-1.8.0 vimdoczh.tar.gz
# 首先查看刚才安装的 vim 安装目录
~$ whereis vim
vim: /usr/bin/vim /usr/share/vim /usr/share/man/man1/vim.1.gz
~$ su			# 切换到 root 
~$ cd vimcdoc-1.8.0
~$ sh vimcdoc.sh -i		# '-i' 表示安装并配置 中文 文档
~$ vim
# 按 :help 可查看帮助文档，此时已经为中文！
```
#### 7.2 更改默认 Tab 缩进长度，由 8 改为 4
```shell
~$ cd 			# 切换到用户目录
~$ vim .vimrc	# 打开 .vimrc 配置文件，如果原来没有则会新建
# 打开后按 'i' 插入如下内容：
set tabstop=4
set softtabstop=4
set shiftwidth=4
```
### 7.3 使用开源配置
- 一个一个配置太麻烦，这里找到一个比较好的（操作比较明确的）开源配置：**[k-vim](https://github.com/wklken/k-vim)** ；[作者的博客](http://wklken.me/posts/2013/06/11/linux-my-vim.html)，（建议按照github上的说明来安装，安装完成可查看 ~/.vimrc 文件来了解设置，同时配以博客文档）可安装说明的安装方法进行安装配置。
- 安装过程中编译自动补全YouCompleteMe时间很长，而且可能会出现问题，按照方法解决时，可能提示 cmake 版本不行，可按上面的 6.4 安装 cmake的最新版本。

### 8 安装中文man手册
- 官方地址下载压缩包：**[https://code.google.com/p/manpages-zh/](https://code.google.com/p/manpages-zh/)**
- 但没翻墙的话应该打不开，而且项目目前已经搬迁到了GitHub上，所以可以通过GitHub下载：**[https://github.com/lidaobing/manpages-zh](https://github.com/lidaobing/manpages-zh)**
- 完整的安装配置如下：
```shell
# 这里是从官网下载： manpages-zh-1.5.1.tar.gz 
# 也可通过 GitHub 下载，指令如下：
# git clone https://github.com/lidaobing/manpages-zh.git
~$ wget http://manpages-zh.googlecode.com/files/manpages-zh-1.5.1.tar.gz
# 也可直接打开官网去下载
~$ tar zxvf manpages-zh-1.5.1.tar.gz
~$ cd manpages-zh-1.5.1
~$ ./configure --prefix=/usr/local/zhman --disable-zhtw 
~$ make
~$ sudo make install
# 如果需要root权限去创建目录，则需要 sudo
~$ cd ~
~$ vi .bashrc
# 在 .bashrc 中增加：
alias cman='man -M /usr/local/zhman/share/man/zh_CN'
~$ source .bashrc	# 刷新
~$ cman cd			# 查看是否成功，如果要使用英文版，可继续使用 man 命令 
```

##**附录**
- **[博客园-Blog](http://bbxytl.github.io/)**
- **[GitHub-Blog](http://bbxytl.github.io/)**
- **关注微信订阅号：LomperWay**：     
    ![关注微信订阅号](./images/qrcodes/qrcode_100.jpg)


