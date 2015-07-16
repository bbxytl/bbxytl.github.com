[GitHubBlog](https://github.com/bbxytl/bbxytl.github.com/tree/master/blog#home--githubblog) /
=====
# [mongodb 学习记录](https://github.com/bbxytl/bbxytl.github.com/blob/master/blog/pages/150708_mongodb学习记录.md#githubblog-)
---
 
在CentOS上使用 `C++ driver` 时需要先安装 `c driver` 和 `libson`，过程比较多，整理了以下脚本：

- 添加环境变量

```shell
vim ~/.bash_profile
# 增加下面句子
>> export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
# 保存后一定要使用下面的命令启用：
source ~/.bash_profile
```

- 开始脚本安装

```shell
# 准备工具
sudo yum install git automake autoconf libtool gcc
mkdir mongodb-drivers
cd mongodb-drivers
# 先安装 libbson
git clone git://github.com/mongodb/libbson.git
cd libbson/
./autogen.sh
make
sudo make install
# 使用测试单元
make test
# 安装 c-driver
git clone https://github.com/mongodb/mongo-c-driver.git
cd mongo-c-driver
./autogen.sh
make
sudo make install
# 安装 C++ driver
git clone -b master https://github.com/mongodb/mongo-cxx-driver
cd mongo-cxx-driver/build
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local ..
sudo make && sudo make install
```
各库链接：
[libbson][1]
[mongo-c-driver][2]
[mongo-cxx-driver][3]


---



## **附录**
- **[博客园-Blog](http://www.cnblogs.com/lomper/)**
- **[GitHub-Blog](http://bbxytl.github.io/)**
- **关注微信订阅号：LomperWay**：     
    ![关注微信订阅号](./images/qrcodes/qrcode_100.jpg)

[1]: https://github.com/mongodb/libbson
[2]: https://github.com/mongodb/mongo-c-driver
[3]: https://github.com/mongodb/mongo-cxx-driver/wiki/Quickstart-Guide-(New-Driver)