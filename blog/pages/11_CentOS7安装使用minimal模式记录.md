[**GitHubBlog**](https://github.com/bbxytl/bbxytl.github.com/tree/master/blog#home--githubblog) /
=====
#[CentOS7安装使用minimal模式记录](https://github.com/bbxytl/bbxytl.github.com/blob/master/blog/pages/11_CentOS7安装使用minimal模式记录.md#githubblog-)
---

安装使用 **minimal** 模式。安装后使用解决方案：
- 安装完成后第一次使用不能上网，**ping**不通。解决方案：
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
- 可查看：[如何设置CentOS 7获取动态及静态IP地址](http://www.ytyzx.net/index.php?title=%E5%A6%82%E4%BD%95%E8%AE%BE%E7%BD%AECentOS_7%E8%8E%B7%E5%8F%96%E5%8A%A8%E6%80%81%E5%8F%8A%E9%9D%99%E6%80%81IP%E5%9C%B0%E5%9D%80)
- 给虚拟机安装增强功能：
```shell
# Vbos的“设备->安装增强功能”
~$ yum -y install gcc make 	# 这个是用来防止运行过程中出现命令找不到的情况
~$ mkdir /media/cdrom	# 新建一个目录用来映射加载后的CD
~$ mount /dev/cdrom /media/cdrom
~$ export KERN_DIR=/usr/src/kernels/`uname -r`
~$ cd /media/cdrom
~$ ./VBoxLinuxAdditions.run
# 安装完后即可使用共享文件夹等，设置好共享文件夹记得执行：
~$ reboot
~$ df  			# 查看是否挂载
```
- 可查看:[centos mini安装virtualbox增强功能](http://blog.sina.com.cn/s/blog_7deb436e0101w8n9.html)
- 安装 ifconfig 命令
```shell
# 正常使用命令： yum install ifconfig 后显示找不到包
~$ yum search ifconfig		# 查找包含 ifconfig 命令的包，正常结果是net-tools.x86_64
~$ yum install net-tools.x86_64	# 安装后即可使用 ifconfig
```
- 使用 Xshell5 远程登陆 CentOS
	- **下载链接：[http://pan.baidu.com/s/1gdpNseJ](http://pan.baidu.com/s/1gdpNseJ) 密码: ` ybkh `**
	- 安装后新建一个会话，在CentOS中使用命令 `ip addr` 查看 ip 地址；
	- 在新建的会话中填入 ip 地址，端口号默认使用 22 ;
	- 确认后连接，输入用户名和密码即可连接；
	- 具体可查看：[Xshell连接linux（图文教程）](http://jingyan.baidu.com/article/3a2f7c2e71f04d26afd611dc.html)





##**附录**
- **[GitHub-Blog](http://bbxytl.github.io/)**
- **关注微信订阅号：LomperWay**：     
    ![关注微信订阅号](./images/qrcodes/qrcode_100.jpg)


