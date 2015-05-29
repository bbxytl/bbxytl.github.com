[**GitHubBlog**](https://github.com/bbxytl/bbxytl.github.com/tree/master/blog#home--githubblog) /
=====
#[git多站点多用户情况下SSH配置](https://github.com/bbxytl/bbxytl.github.com/blob/master/blog/pages/10_git多站点多用户情况下SSH配置.md#githubblog-)
---
个人使用github，但是公司使用的是 GitLab 。那么在一个电脑上进行处理时，由于先设置了 github 的，导致没办法从 GitLab 上处理 git 。其实是由于 ssh 的问题。
下面记录一下处理过程：
- 首先使用下列命令生成新的**ssh**

```shell
ssh-keygen -t rsa -C 'myusername@mycompanyname.com'
# 然后会让你输入文件名，可以输入 id_rsa_mycompanyname
# 然后一路回车就行
```
- 将生成的两个文件 `id_rsa_mycompanyname` 和 `id_rsa_mycompanyname.pub` 拷贝到目录 `C:\Users\yourName\.ssh` 下。
- 在目录 `.ssh` 下找到文件 `config` ，如果没有这个文件，则新建一个，切记，这个文件没有后缀名！
- 在 `config` 里加入如下内容：

```shell
# Default github user(myusername@mygithubMail.com)
Host github
	HostName github.com
	User git
	IdentityFile ~/.ssh/id_rsa

# second user(myusername@mycompanyname.com)
Host gitlab.yourcompanyname.com
	HostName gitlab.yourcompanyname.com
	User git
	Port 22
	IdentityFile ~/.ssh/id_rsa_mycompanyname
```
- 将这个生成的 `id_rsa_mycompanyname.pub` 的内容加入到 **gitlab**上的SSH上后，即可使用！
- 此时，github和gitlab 都可正常使用。如果有其他的网站，也可以按照这种方法继续处理。每一个 ssh的生成都要对应站点使用的email 。




##**附录**
- **[GitHub-Blog](http://bbxytl.github.io/)**
- **关注微信订阅号：LomperWay**：     
    ![关注微信订阅号](./images/qrcodes/qrcode_100.jpg)


