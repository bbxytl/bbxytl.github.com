[**GitHubBlog**](https://github.com/bbxytl/bbxytl.github.com/tree/master/blog#home--githubblog) /
=====
#[Linux脚本学习笔记（一）](https://github.com/bbxytl/bbxytl.github.com/blob/master/blog/pages/7_Linux脚本学习笔记（一）.md#githubblog-)

##1. 执行脚本文件的方法
- 先给文件添加可执行权限，再执行
```shell
# 假设uad.sh为要执行文件
~$: chmod +x uad.sh  
~$: ./uad.sh
```
- 使用 **`sh`**命令：
```shell
~$: sh uad.sh
```
- 使用 **`source`**命令：
```shell
~$: source uad.sh
```
##2. 查找文件
```shell
# 查找 /etc 目录下以 “.conf” 后缀的文件
~$: find  /etc  -name  "*.conf"  -type  f  
# 统计查找到的文件数目
~$: find  /etc  -name  "*.conf"  -type  f  | wc  -l
```
##3. 重定向操作
重定向名  |  重定向符号  | 描述
--  | -- | --
重定向输入 | <           | 从指定文件读取，不从键盘输入
重定向输出 | > , >>    | 将结果覆盖 ； 追加到文件
标准错误输出 | 2> , 2>> |将错误信息覆盖，追加到文件
混合输出 | &> , &>> | 将标准输出与错误输出覆盖，追加到文件

逻辑名 ：逻辑符号   

- 与 : &&
- 或 : ||
- 顺序执行 ： ；
```shell
~$: mkdir   /mulu/a  2>  /dev/null  && echo  "成功"
# 只有创建成功才会输出 “成功”
~$: mkidr  /mulu/a  2>  /dev/null  || echo  "失败"
# 只有创建失败才会输出
~$: cd /boot/grub ;  ls -lh grub.conf
```
##4. 变量
- 变量的定义与赋值：`变量名 = 变量值`
- 引用变量：`$变量名 、${变量名}`
```shell
~$:  Title = BeiDa
~$:  echo $Title
BeiDa
```
- 双引号`“`：允许引用、转义
- 单引号`'`：禁止引用、转义
- 反撇号 ` 或 $( ) ：以命令输出进行替换
```shell
~$: echo "$Title Group"
BeiDa Group
~$: echo '$Title Group'
$Title Group
# 输出当前linux内核版本号
~$: uname -r
2.6.18-194.e15
# 使用反撇号
~$: ver=`uname -r`
~$: echo $ver
2.6.18-194.e15
```
- 环境变量、记录或设置运行参数
    - 系统赋值：USER, LOGNAME, HOME, SHELL.....
    - 用户操作：PATH, LANG, CLASSPATH.....
```shell
# 输出当前所有环境变量
~$: env
# 输出内容。。。。
~$: echo $USER $HOME SHELL
zhangsan  /root  SHELL
~$: echo $LANG
zh_CN.UTF-8
```
- 其他特殊变量：由操作系统赋值，不可直接赋值
变量符号 | 描述
-- |--
$?      | 前一条命令的状态值，0为正常，非0为异常
$0      | 脚本自身的程序名
$1--$9 | 第1到第9个位置参数
$*      | 命令行的所有位置参数的内容
$#      | 命令行位置参数的个数
##5. 数值运算

- expr命令：`expr  数值1  操作符     数值2`
- $[ ]表达式：`$[  数值1  操作符     数值2 ]`  
- **`*`**号需要转义！ 在 $[]中可不用转义 
```shell
~$: expr 2 \* 3
6
~$: x=45; y=12; expr $x+$y
66
~$: echo $[45+12]&ndash;<table></table><td></td>
66
~$: echo $[45 * 21]
945
~$: echo $[x-y] ; echo $[$x-$y] # 可加$ 也可不加
24 24
```
- 递增处理：`let    变量名++    、let    变量名--
- 使用随机数：`RANDOM 变量名`
- 生成数值序列：`seq   首数   末数 、seq    首数   增量   末数
```shell
~$: x=45 ; let x++; echo $x
46
~$: x=45; let x+=2;  echo $x
47
~$: echo $RANDOM
4411
~$: echo $[RANDOM % 100]
54
~$: seq 3
1
2
3
~$: seq 3  5
3
4
5
~$: seq  3  2 10
3
5
7
9
```
- 小数运算：使用 `bc ` 命令处理，将表达式结果传给 bc
```shell
~$: echo "45.67 - 21.05" | bc
24.62
# scale=n 约束小数位数
~$: echo "scale=4;  10/3 " |bc
3.333
```
##6. 字符串操作

- 字符串截取
     - 路径截取： `dirname ,  basename` 命令
     - expr命令：`expr   substr  $var   起始位置   截取长度`  ，起始位置从 1 开始
     - ${ } 命令 ：`${ var : 起始位置 : 截取长度 }  ， 起始位置从 0 开始
```shell
~$: var="/etc/httpd/conf/httpd.conf"
~$: dirname $var
/etc/httpd/conf
~$: basename $var
httpd.conf
~$: var=BeiDaQingNiao
~$: expr  substr  $var  4  6
DaQing
~$: echo ${ var :4 :6 }
aQingN
#从开头开始截取时可以省略起始位置
~$: echo ${ var : :5}
BeiDa
```
- 字符串替换
    - `${var/old/new }`  ：将第一个 old 替换为 new，**中间没有空格**
    - `${var //old /new }` ：将所有 old 替换为 new，**中间没有空格**
```shell
~$: var=BeiDaQingNiao; echo ${var/i/##}
Be##DaQingNiao
~$: var=BeiDaQingNiao; echo ${var//i/##}
Be##DaQ##ngN##ao
```
- 获取随机字符串
    - `/dev/urandom ` &rarr; `/usr/bin/md5sum` &rarr; `/bin/cut`
    - 随机设备 &rarr; MD5转换 &rarr; 截取字符串
    - cut命令：`cut -b  起始位置-结束位置`，起始位置为开，或结束位置为最后时可省略
```shell
# 随机字符 到 ASCII 字符 ，使用cut命令截取从第4个位置到第8个位置间的字符串
~$: head -1 /dev/urandom | md5summ | cut -b 4-8
53364
```
##7. 条件测试
- 格式：`test 条件表达式`  或  `[ 条件表达式]`
- 文件状态：
    - -e : 目标是否存在（Exist）
    - -d：目标是否为目录（directory）
    - -f：是否为文件（File）
- 权限检测：
    - -r：是否可读（Read）
    - -w：是否可写（Write）
    - -x：是否可执行（eXcute）
- 整数比较：
    - -eq：等于（Equal）
    - -ne：不等于（No Equal）
    - -gt：大于（Greater Than）
    - -lt：小于（Lesser Than）
    - -ge：大于或等于（Greater or Equal）
    - -le：小于或等于（Lesser or Equal）
```shell
# 统计当前用户数
~$: who  |  wc -l
2
~$: [$(who | wc -l) -eq 2] && echo YES
YES
```
- 字符串匹配
    - `=`：两字符串相同
    - `!=`：两字符串不同



##**附录**
- **[GitHub-Blog](http://bbxytl.github.io/)**
- **关注微信订阅号：LomperWay**：     
    ![关注微信订阅号](./images/qrcodes/qrcode_100.jpg)


