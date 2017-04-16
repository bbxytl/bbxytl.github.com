# python 虚拟环境设置

## virtualenv
virtualenv 是一个可以在同一计算机中隔离多个python版本的工具。有时，两个不同的项目可能需要不同版本的python，如 python2.6.6 / python3.0 ，但是如果都装到一起，经常会导致问题。所以需要一个工具能够将这两种或几种不同版本的环境隔离开来，需要哪个版本就切换到哪个版本做为默认版本。virtualenv 既是满足这个需求的工具。它能够用于创建独立的Python环境，多个Python相互独立，互不影响，能够：

1. 在没有权限的情况下安装新套件
2. 不同应用可以使用不同的套件版本
3. 套件升级不影响其他应用

### 安装：
`pip install virtualenv`

### 使用方法
##### 1. 创建环境

- `virtualenv [新环境名]` :这会再当前目录下创建一个新环境目录
- 可使用 virtualenv --help 来查看如何使用。可以使用参数 `--python=/usr/bin/python3` 来创建一个已经安装的的Python环境。

##### 2. 使用环境

- 切换到新环境目录
- 执行：`source ./bin/activate` 来激活环境，激活后在命令行的前面会出现新环境名称
- 接下来可使用pip命令安装新环境需要的各种包。（pip命令在创建新环境时自带）

##### 3. 退出虚拟环境

- `deactivate`
- 如果要删除虚拟环境，只需退出虚拟环境后，删除对应的虚拟环境目录即可。不影响其他环境。

##### 4. 注意事项

- 如果没有启动虚拟环境，系统也安装了pip工具，那么套件将被安装在系统环境中，为了避免发生此事，可以在~/.bashrc文件中加上：`export PIP_REQUIRE_VIRTUALENV=true`
- 或者让在执行pip的时候让系统自动开启虚拟环境：`export PIP_RESPECT_VIRTUALENV=true`


## virtualenvwrapper

virtualenvwrapper是virtualenv的扩展管理包，用于更方便管理虚拟环境，它可以做：

1. 将所有虚拟环境整合在一个目录下
2. 管理（新增，删除，复制）虚拟环境
3. 切换虚拟环境

### 安装
`pip install virtualenvwrapper`

### 使用方法
##### 1. 初始配置
默认virtualenvwrapper安装在/usr/local/bin下面，实际上需要运行virtualenvwrapper.sh文件才行；所以需要先进行配置一下：

- 创建虚拟环境管理目录: `mkdir $HOME/.local/virtualenvs` 
-  在~/.bashrc中添加行： 
```shell
  export VIRTUALENV_USE_DISTRIBUTE=1        #  总是使用 pip/distribute                                                                                   
  export WORKON_HOME=$HOME/.local/virtualenvs       # 所有虚拟环境存储的目录             
  if [ -e $HOME/.local/bin/virtualenvwrapper.sh ];then                                                                                                        
      source $HOME/.local/bin/virtualenvwrapper.sh                                                                                                         
  else if [ -e /usr/local/bin/virtualenvwrapper.sh ];then                                                                                                     
            source /usr/local/bin/virtualenvwrapper.sh                                                                                                        
       fi                                                                                                                                                     
  fi                                                                                                                                                          
  export PIP_VIRTUALENV_BASE=$WORKON_HOME                                                                                                                     
  export PIP_RESPECT_VIRTUALENV=true   
```
- 启动 virtualenvwrapper: `source ~/.bashrc`

##### 2. 使用方法
所有的命令可使用：`virtualenvwrapper --help` 进行查看，这里列出几个常用的：

- 创建基本环境：`mkvirtualenv [环境名]`
- 删除环境：`rmvirtualenv [环境名]`
- 激活环境：`workon [环境名]`
- 退出环境：`deactivate`
- 列出所有环境：`workon` 或者 `lsvirtualenv -b`

所有命令都可在后面使用 `--help` 参数查看具体用法！Enjoy it !
