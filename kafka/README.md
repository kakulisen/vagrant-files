# kafka单机版

### 准备工作

下载以下安装包放置在当前目录

* `jdk-8u221-linux-x64.tar.gz`  
* `apache-zookeeper-3.5.7-bin.tar.gz`  

* `kafka_2.12-2.4.0.tgz` 

### 安装JDK

编辑`/etc/profile`

```
export JAVA_HOME=/opt/jdk1.8.0_221
export JRE_HOME=$JAVA_HOME/jre
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib:$JRE_HOME/lib
```

验证：

```
source /etc/profile
java -version
```



### 安装zookeeper

编辑`/etc/profile`

```
export ZOOKEEPER_HOME=/opt/apache-zookeeper-3.5.7-bin
export PATH=$PATH:$ZOOKEEPER_HOME/bin
```

验证：

```
source /etc/profile
zkServer.sh start 
```



### 安装kafka

编辑`/etc/profile`

```
export KAFKA_HOME=/opt/kafka_2.12-2.4.0
export PATH=$PATH:$KAFKA_HOME/bin
```

验证：

```
source /etc/profile
kafka-server-start.sh -daemon config/server.properties
```



### 导出box：

先进入控制台执行以下命令

```
echo "ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEA6NF8iallvQVp22WDkTkyrtvp9eWW6A8YVr+kz4TjGYe7gHzIw+niNltGEFHzD8+v1I2YJ6oXevct1YeS0o9HZyN1Q9qgCgzUFtdOKLv6IedplqoPkcmF0aYet2PkEDo3MlTBckFXPITAMzF8dJSIFo9D8HfdOV0IAdx4O7PtixWKn5y2hMNG0zQPyUecp4pzC6kivAIhyfHilFR61RGL+GPXQ2MWZWFYbAGjyiYJnAmCP3NOTd0jMZEnDkbUvxhMmBYSdETk1rRgm+R4LOzFUGaHqHDLKLX+FIPKcF96hrucXzcWyLbIbEgE98OHlnVYCzRdK8jlqm8tehUc9c9WhQ== vagrant insecure public key" > /home/vagrant/.ssh/authorized_keys
```

退出后执行打包

```
vagrant package --base kafka_node1_1582938586600_2377 --output kafka-centos7.box
vagrant box add kafka-centos7.box --name kafka-centos7
```



