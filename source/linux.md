---
title: linux版
permalink: /linux/
description: Verison 1.0.0
---

# 安装应用软件

## 介质

####程序目录
mkdir -p /u01/software
chown -R hdm:dba /u01
####上传介质
>hdm用户上传介质

```yaml
[hdm@hdm01 software]$ pwd/u01/software[hdm@hdm01 software]$ ls -l-rwxrwxrwx. 1 hdm dba   8491533 Feb  7 17:10 apache-maven-3.3.9-bin.tar.gz-rwxrwxrwx. 1 hdm dba   9323097 Feb  7 17:10 apache-tomcat-8.5.8.tar.gz-rwxrwxrwx. 1 hdm dba 191659031 Feb  7 17:11 hbiparent.zip-rwxrwxrwx. 1 hdm dba 192091468 Feb  8 14:05 HDM_SOURCE.zip-rwxrwxrwx. 1 hdm dba 181442359 Feb  7 17:11 jdk-8u111-linux-x64.gz-rwxrwxrwx. 1 hdm dba   1544040 Feb  7 17:11 redis-3.2.5.tar.gz-rwxrwxrwx. 1 hdm dba       165 Feb  7 17:11 starthdm.sh-rwxrwxrwx. 1 hdm dba       129 Feb  7 17:11 stophdm.sh
```

## 安装tomcat

####下载tomcat

>本次安装的tomcat介质：apache-tomcat-8.5.8.tar.gz

####安装tomcat
```yaml
mkdir /u01/HDMcd /u01/HDMtar -zxvf /u01/software/apache-tomcat-8.5.8.tar.gz
```


####启动tomcat
>/u01/HDM/apache-tomcat-8.5.8/bin/startup.sh

```yaml[hdm@hdm01 bin]$ /u01/HDM/apache-tomcat-8.5.8/bin/startup.shUsing CATALINA_BASE:   /u01/HDM/apache-tomcat-8.5.8Using CATALINA_HOME:   /u01/HDM/apache-tomcat-8.5.8Using CATALINA_TMPDIR: /u01/HDM/apache-tomcat-8.5.8/tempUsing JRE_HOME:        /usrUsing CLASSPATH:       /u01/HDM/apache-tomcat-8.5.8/bin/bootstrap.jar:/u01/HDM/apache-tomcat-8.5.8/bin/tomcat-juli.jarTomcat started.[hdm@hdm01 bin]$
```
>启动port端口为8080*对应的停止脚本*/u01/HDM/apache-tomcat-8.5.8/bin/shutdown.sh

## 安装redis
####下载redis

>本次安装的redis介质如下：redis-3.2.5.tar.gz

####安装redis

>cd /u01/HDMtar -zxvf /u01/software/redis-3.2.5.tar.gzcd /u01/HDM/redis-3.2.5sudo make 

日志会留下如下：
```yaml
make[1]: Leaving directory `/u01/HDM/redis-3.2.5/src'
```

>cd /u01/HDM/redis-3.2.5/srcmkdir /u01/HDM/redis-3.2.5/redissudo make install  PREFIX=/u01/HDM/redis-3.2.5/redis

```yaml
日志：Hint: It's a good idea to run 'make test' ;)    INSTALL install    INSTALL install    INSTALL install    INSTALL install    INSTALL install
```

####配置文件
/*******************************注释开始*********************************/先新建文件夹mkdir /u01/HDM/redis-3.2.5/logs/*******************************注释结束*********************************/vi /u01/HDM/redis-3.2.5/redis.conf1.启动port端口为63792.将daemonize的值改为yes3.logspidfile /u01/HDM/redis-3.2.5/logs/redis_6379.pidlogfile "/u01/HDM/redis-3.2.5/logs/redis.log"dir /u01/HDM/redis-3.2.5/logs
####启动redis
Redis放在服务器后台运行，修改配置文件属性：将daemonize的值改为yes （默认值为no）
>启动命令：/u01/HDM/redis-3.2.5/redis/bin/redis-server  /u01/HDM/redis-3.2.5/redis.conf停止命令：/u01/HDM/redis-3.2.5/redis/bin/redis-cli shutdown　　或者pkill redis-server

##安装mavenv 部署工具
####说明
安装maven
####下载maven
URL: http://maven.apache.org/download.cgi本次安装的maven介质如下：apache-maven-3.3.9-bin.tar.gz
####安装jdk
```yaml
cd  /u01/HDMtar -zxvf /u01/software/jdk-8u111-linux-x64.gz设置java环境变量export JAVA_HOME=/u01/HDM/jdk1.8.0_111export PATH=$JAVA_HOME/bin:$PATHexport JAVA_BIN=$JAVA_HOME/binexport JAVA_LIB=$JAVA_HOME/libexport CLASSPATH=.:$JAVA_LIB/tools.jar:$JAVA_LIB/dt.jar
```
####安装maven
```yaml
cd /u01/HDMtar -zxvf /u01/software/apache-maven-3.3.9-bin.tar.gz
```
####环境变量
```yaml
配置maven的环境变量export MAVEN_HOME= /u01/HDM/apache-maven-3.3.9export PATH=${MAVEN_HOME}/bin:${PATH}export PATH=$PATH:/u01/HDM/redis-3.2.5/redis/binexport CATALINA_HOME=/u01/HDM/apache-tomcat-8.5.8export CATALINE_BASH=/u01/HDM/apache-tomcat-8.5.8
```
####安装后验证
```yaml
[hdm@hdm01 ~]$ mvn -vApache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:47+08:00)Maven home: /u01/HDM/apache-maven-3.3.9Java version: 1.8.0_111, vendor: Oracle CorporationJava home: /u01/HDM/jdk1.8.0_111/jreDefault locale: en_US, platform encoding: UTF-8OS name: "linux", version: "2.6.32-358.el6.x86_64", arch: "amd64", family: "unix"
```
#配置
##配置数据库
####创建表空间
```yaml
CREATE TABLESPACE "HDM_DATA"     LOGGING     DATAFILE '/u01/app/oracle/oradata/HYTST/hdm_data01.dbf' SIZE 1000M     AUTOEXTEND     ON NEXT  100M MAXSIZE  8000M EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT  AUTO;
```
####创建数据库用户
```yaml
create user hdm identified by z8MhZismTJ default tablespace HDM_DATA TEMPORARY TABLESPACE "TEMP";
```
####授权
```yaml
grant connect ,resource,create view,create any directory ,create database link to hdm;
```
##配置应用
####源代码
```yaml
cd /u01/HDMunzip /u01/software/core-db.zip[hdm@hdm01 core-db]$ pwd/u01/HDM/core-db
```
####编译（需要连接外网）
(该操作只需要在一台机器上执行，第二个节点可以考虑直接拷贝core.war包)```yaml
在源码文件夹下执行：mvn clean install例如本次：cd  /u01/HDM/core-dbmvn clean install
```(日志显示download的包，存放在当前用户/home/user/.m2下，该部分可以拷贝)/******************************注释开始*******************************/这个下载时间挺长，有时会卡住，有时会执行失败，若失败就再执行一次mvn clean install。如果卡住可以等待。或者ctrl+c退出，然后再执行一次。/******************************注释结束*******************************/
####初始化数据到hdm用户（只需要在一台机器上执行）
编译成功后，执行初始化数据库命令：
```yamlmvn process-resources -D skipLiquibaseRun=false -D db.driver=oracle.jdbc.driver.OracleDriver -D db.url=jdbc:oracle:thin:@ys-dbtest.jingpai.com:1531:HYTST -Ddb.user=hdm -Ddb.password=z8MhZismTJ
``````yaml[INFO] Reactor Summary:[INFO] core-db ............................................ SUCCESS [  1.073 s][INFO] ------------------------------------------------------------------------[INFO] BUILD SUCCESS[INFO] ------------------------------------------------------------------------[INFO] Total time: 39.836 s[INFO] Finished at: 2017-02-09T09:55:50+08:00[INFO] Final Memory: 68M/974M[INFO] ------------------------------------------------------------------------[hdm@hdm01 core-db]$
```
####修改tomcat
修改tomcat目录（/u01/HDM/apache-tomcat-8.5.8/conf）下的conf/context.xml,在最后面添加如下配置：
```yaml<Resource auth="Container" driverClassName="oracle.jdbc.driver.OracleDriver" name="jdbc/hbi_dev" type="javax.sql.DataSource" url="jdbc:oracle:thin:@ys-dbtest.jingpai.com:1531:HYTST" username="hdm" password="z8MhZismTJ"/>
```本次截图：
####拷贝war包
将源代码目录下/core/target/core.war拷贝到tomcat的webapps目录下，注意：模板文件上传路径默认如下：需要更改文件存储路径的，可修改配置文件，修改方式为：可将core.war先重命名为core.zip，找到core.zip\WEB-INF\classes路径下config.properties文件，进行相应修改。若修改此配置文件，则以后每次更新系统，都需要修改配置文件。
```cp /u01/HDM/hbiparent/core/target/core.war /u01/HDM/apache-tomcat-8.5.8/webapps$ls -l /u01/HDM/apache-tomcat-8.5.8/webapps/core.war -rw-r--r--. 1 hdm dba 127410530 Feb  9 10:04 /u01/HDM/apache-tomcat-8.5.8/webapps/core.war
```
####添加永久环境变量
编辑hdm用户目录(/home/hdm)下的.bash_profilevi /home/hdm/.bash_profile设置java环境变量
```export JAVA_HOME=/u01/HDM/jdk1.8.0_111export PATH=$JAVA_HOME/bin:$PATHexport JAVA_BIN=$JAVA_HOME/binexport JAVA_LIB=$JAVA_HOME/libexport CLASSPATH=.:$JAVA_LIB/tools.jar:$JAVA_LIB/dt.jar
```配置maven的环境变量
```export MAVEN_HOME=/u01/HDM/apache-maven-3.3.9export PATH=${MAVEN_HOME}/bin:${PATH}export PATH=$PATH:/u01/HDM/redis-3.2.5/redis/binexport CATALINA_HOME=/u01/HDM/apache-tomcat-8.5.8export CATALINE_BASH=/u01/HDM/apache-tomcat-8.5.8
```注：修改文件后要想马上生效还要运行$ source /home/hdm/.bash_profile不然只能在下次重进此用户时生效。
####redis和tomcat服务启停
先启动redis服务，再启动tomcat服务，redis启动/停止/u01/HDM/redis-3.2.5/redis/bin/redis-server  /u01/HDM/redis-3.2.5/redis.conf/u01/HDM/redis-3.2.5/redis/bin/redis-cli shutdownTomcat启动/停止/u01/HDM/apache-tomcat-8.5.8/bin/startup.sh/u01/HDM/apache-tomcat-8.5.8/bin/shutdown.sh
####访问url（两个节点同样的安装方式）
通过http://<ip>:<prot>/core访问hdm，http://172.16.2.112:8080/corehttp://172.16.2.113:8080/core初始用户admin/admin