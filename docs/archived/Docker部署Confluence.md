## 一、Mysql安装

###  背景

Atlassian Confluence（简称Confluence）是一个专业的wiki程序。它是一个知识管理的工具，通过它可以实现团队成员之间的协作和知识共享。[官网](https://www.atlassian.com/software/confluence)

------

- 环境：CentOS7.2
- 容器：Docker
- 镜像：atlassian/confluence-server
- 辅助：Mysql

------

###  安装Mysql镜像
```shell
#启动docker
$ systemctl start docker 

#搜索mysql镜像
$ docker search mysql 

#拉取镜像
$ docker pull mysql:5.7 

#查看镜像
$ docker images 
```

### 创建映射目录&配置文件

```shell
#创建映射数据目录
$ mkdir -p /export/data/mysql  

#创建映射配置目录
$ mkdir -p /export/config/mysql 

#编辑Mysql配置文件
$ vim     /export/config/mysql/mysqld.cnf 
```

> mysqld.cnf配置的内容如下:

```shell
# Copyright (c) 2014, 2016, Oracle and/or its affiliates. All rights reserved.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; version 2 of the License.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA

#
# The MySQL  Server configuration file.
#
# For explanations see
# http://dev.mysql.com/doc/mysql/en/server-system-variables.html

[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
log-error      = /var/log/mysql/error.log
# By default we only accept connections from localhost
#bind-address   = 127.0.0.1
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

# confluence官网指定的自定义配置
character-set-server=utf8
collation-server=utf8_bin
default-storage-engine=INNODB
max_allowed_packet=256M
innodb_log_file_size=2GB
transaction-isolation=READ-COMMITTED
binlog_format=row
```

### 启动mysql

```shell
#-v表示将容器路径挂载到相应宿主机路径下，按照配置文件参数启动mysql容器
$ docker run --name mysql -p 3306:3306 -v /export/config/mysql/mysqld.cnf:/etc/mysql/my.cnf -v /export/data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root -d mysql:5.7

#查看容器是否正常启动
$ docker ps -a  
```



### 登录mysql

```shell
#进入mysql容器
$ docker exec -it mysql bash 

#登录mysql
$ mysql -u root -p 
```



### 设置登录信息

```shell
#可设置root用户只能本地登录
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '1Aa@11'; 

#添加登录用户
mysql> CREATE USER 'linlixin'@'%' IDENTIFIED WITH mysql_native_password BY 'linlixin';

#设置允许远程登录
mysql> GRANT ALL PRIVILEGES ON *.* TO 'linlixin'@'%';

#刷新权限
mysql> FLUSH PRIVILEGES;
```



### 创建数据库

```shell
# 创建confluence数据库及用户
mysql> CREATE DATABASE confluence CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
grant all on confluence.* to 'confluence'@'%' identified by 'confluence';

# confluence要求设置事务级别为READ-COMMITTED
mysql> set global tx_isolation='READ-COMMITTED';
```



## 二、Confluence安装

### 素材准备

- Docker镜像 [Github链接](https://github.com/cptactionhank)
- 破解工具 [Gitee链接](https://gitee.com/pengzhile/atlassian-agent)

在gitee 中下载编译好的破解`jar`包即可，放置在Dockerfile同目录下

```css
- Confluence
  --Dockerfile
  --atlassian-agent.jar
```

采用以上工具，理论上可以破解**几乎**全部版本。



### 编写Dockerfile文件

```dockerfile
FROM cptactionhank/atlassian-confluence:latest

USER root

# 将代理破解包加入容器
COPY "atlassian-agent.jar" /opt/atlassian/confluence/

# 设置启动加载代理包
RUN echo 'export CATALINA_OPTS="-javaagent:/opt/atlassian/confluence/atlassian-agent.jar ${CATALINA_OPTS}"' >> /opt/atlassian/confluence/bin/setenv.sh
```



### 构建镜像

```shell
#构建镜像
$ docker build -t linlixin/confluence:latest .
```



### 启动容器

> **-d:** 后台运行容器，并返回容器ID；
>
> **--name confluence:** 为容器指定一个名称；
>
> **-p:** 指定端口映射，格式为：**主机(宿主)端口:容器端口**
>
> **-e TZ="Asia/Shanghai" :** 设置环境变量；
>
> **--volume , -v:** 绑定一个卷
>

更多参数[查看](https://www.runoob.com/docker/docker-run-command.html)
```bash
#启动容器
docker run -d --name confluence \
  --restart always \
  -p 8090:8090 \
  -e TZ="Asia/Shanghai" \
  -v /home/data/www/confluence.linlixin.com:/var/atlassian/confluence \
  linlixin/confluence:latest
```



### 访问 confluence

访问 IP:18010，参照安装流程，进行操作。

![image-20200510152424850](https://imgs.wzlinux.com/blog/202005/10/152425-514599.png)

我们就选择一个应用吧

![image-20200510152518219](https://imgs.wzlinux.com/blog/202005/10/152519-264644.png)


![image-20200831093907802](/Users/linlixin/Library/Application Support/typora-user-images/image-20200831093907802.png)

### 获取授权码
> -m:本人注册邮箱；
>
> -p:设置产品类型-p conf
>
> -o:confulence所在服务器地址
>
> -s:服务器ID

```shell
#执行获取授权码
java -jar atlassian-agent.jar \
  -d -m 18898871624@189.cn -n ALIBABA \
  -p conf -o http://47.115.117.138 \
  -s B8SR-7VLE-FD9H-IWGH
```

![image-20200510152622252](https://imgs.wzlinux.com/blog/202005/10/152623-217452.png)

### 连接数据库

选择单机模式，并设置数据库

```shell
#数据库URL
$ jdbc:mysql://47.115.117.138/confluence?useUnicode=true&characterEncoding=utf8
```

![微信图片_20200510204736](https://imgs.wzlinux.com/blog/202005/10/204802-411920.png)

###  配置 confluence

我们做个示范站点

![image-20200510153008698](https://imgs.wzlinux.com/blog/202005/10/153009-849739.png)

### 查看授权信息

登陆查看授权情况

![image-20200510154035663](https://imgs.wzlinux.com/blog/202005/10/154036-494460.png)

