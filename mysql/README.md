### 注意

请先提前下载好mysql放置在当前目录，下载链接：

https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.19-1.el7.x86_64.rpm-bundle.tar

下载好后文件名应为：`mysql-8.0.19-1.el7.x86_64.rpm-bundle.tar`



## 制作基础镜像

```
vagrant package –-base mysql_default_1582597895957_84544 –-output ./mysql8-centos7.box
vagrant box add ./mysql8-centos7.box --name mysql8-centos7
```

