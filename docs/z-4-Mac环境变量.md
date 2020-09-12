# Mac环境变量

Mac使用bash做为默认的shell

MAC OS X环境配置的加载顺序

```bash
# 系统级别
/etc/profile
/etc/paths 

# 用户级别
~/.bash_profile  # 目前mac仅有该配置
~/.bash_login 
~/.profile 
~/.bashrc

```

前两个是系统级别的环境变量，针对所有用户，后面四个带有`~/`用户级别的环境变量。

- 前两个环境配置在系统启动时候就会加载。
- `~/.bash_profile`，`~/.bash_login`，`~/.profile`依次加载，如果`~/.bash_profile`不存在，依次加载后面几个文件；如果`~/.bash_profile`文件存在，后面几个文件不会加载
  - `~/.bashrc` 是bash shell打开时候加载

## 全局环境变量设置

修改全局环境变量时候参考系统默认的环境变量配置格式。

修改全局环境变量需要root权限。

- /etc/paths 全局建议修改这个文件
- /etc/profile 不建议修改这个文件，全局共有配置，用户登录时候都会加载该文件
- /etc/bashrc 一般在这个文件中添加系统级别的环境变量，全局共有配置，bash shell执行时候都会加载

## 用户级别环境变量设置

在`~/.bash_profile`中配置环境。



```ruby
# 使用冒号隔开
export PATH=$PATH:<PATH 1>:<PATH 2>:<PATH 3>:------:<PATH N>
# 或者 
export PATH=${PATH}:<PATH 1>
export PATH=${PATH}:<PATH 2>

# 第一种将路径合并在一起，不方便删除，建议使用第二种，换行挨个设置
```



```bash
# java
export JAVA_HOME = /home/myuser/jdk1.7.0_03
export PATH = $JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

# M2_HOME
export M2_HOME=/Users/YourUserName/Documents/Dev/apache-maven-3.3.9
export PATH=$PATH:$M2_HOME/bin
```

## .zshrc配置文件
```shell
#  使用zsh(shell环境),加载配置引入环境变量 source .bash_profile

# PATH优先HOME，再找local，最后找/usr/bin
 export PATH=$HOME/bin:/usr/local/bin:$PATH
```



## 重新载入配置文件

在环境配置完毕后，一般是重新电脑才会生效，如果想要立即生效，执行以下指令

```bash
source <相应文件配置文件>

#示例
source .bash_profile
```



