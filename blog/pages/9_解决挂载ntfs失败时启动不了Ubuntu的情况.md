[**GitHubBlog**](https://github.com/bbxytl/bbxytl.github.com/tree/master/blog#home--githubblog) /
=====
#[**解决挂载ntfs失败时启动不了Ubuntu的情况**](https://github.com/bbxytl/bbxytl.github.com/blob/master/blog/pages/9_解决挂载ntfs失败时启动不了Ubuntu的情况.md#githubblog-)
----
由于使用的是双系统，在Ubuntu中可以挂载 Windows的ntfs 硬盘，了解了两张方式：

- 使用命令 `ntfsfix` ;
- 使用命令 `ntfs-config`

开始时使用的是ntfsfix，感觉每次都手动挂载比较麻烦，就使用了 `ntfs-config`。然后重启就失败了，进不去 Ubuntu 的系统了。一开始并没有意识到是ntfs-config导致的，所以试了很多方法都没解决。开机时显示了一行字：
`××××××××× /media/sda1 ××××××××× S ×××××××× M ××××` ，仔细思考了一下觉得可能是挂载的问题。 下面是摸索出来的解决方法：

- 重启选择**Ubuntu的高级选项**；
- 选择**修复模式(recovery mode)** ；
- 按 **`e`** 编辑；
- 将 **`ro recovery nomodeset` **修改为** `rw single init=/bin/bash` **;
- 再按  **`Ctrl + x`** 进行引导，进入单用户模式。
- 使用命令 **`vim /etc/fstab ` **修改内容，将不是 Ubuntu 的那部分都注释（或删除）掉;
- 再按 **`Ctrl + Alt + Del` **进行重启。然后等待后，OK，问题解决！

进入系统后，果断使用命令** `sudo apt-get remove ntfs-config` **删除了这个包！同时用命令 **`dpkg -P ntfs-config` **删除了所有`ntfs-config `配置文件。瞬间世界就清净了许多啊 ^_^。

我有三个Windows下的分区，都是ntfs的，所以没次手动加载很烦，写了一个脚本来执行，附上：
```shell
# 如果在Windows下可能会使用了  休眠  来关闭的Windows，那么不建议将C 盘也这样挂载！
# echo "你的sudo密码" | sudo -S ntfsfix /dev/sda1    # C
echo "你的sudo密码" | sudo -S ntfsfix /dev/sda5    # D(SoftFile)
echo "你的sudo密码" | sudo -S ntfsfix /dev/sda6    # E(StudyFile)
echo "你的sudo密码" | sudo -S ntfsfix /dev/sda7    # F(PorsenFile)
# 首先你要先在 /media/user-name/下建立以下四个文件夹
if [ ! -d /media/user-name/SoftFile ] ;then 
	echo "你的sudo密码" | sudo -S mkdir /media/user-name/SoftFile
fi
if [ ! -d /media/user-name/StudyFile ] ;then 
	echo "你的sudo密码" | sudo -S mkdir /media/user-name/StudyFile
fi
if [ ! -d /media/user-name/PorsenFile ] ;then 
	echo "你的sudo密码" | sudo -S mkdir /media/user-name/PorsenFile
fi
# sudo mount -t ntfs /dev/sda1 /media/user-name/WinC
sudo mount -t ntfs /dev/sda5 /media/user-name/SoftFile
sudo mount -t ntfs /dev/sda6 /media/user-name/StudyFile
sudo mount -t ntfs /dev/sda7 /media/user-name/PorsenFile
```


##**附录**
- **[GitHub-Blog](http://bbxytl.github.io/)**
- **关注微信订阅号**：     
    ![关注微信订阅号](./images/qrcodes/qrcode_100.jpg)
