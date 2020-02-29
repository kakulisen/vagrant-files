# vagrant-files
### 测试环境

* vagrant 2.2.6
* virtualbox 6.0.14
* windows 10



### 已完成的软件

1. mysql 8.0



### 如何手动下载 Vagrant boxs

* 通过命令行：

```
# The box URL is https://app.vagrantup.com/centos/boxes/7/versions/1905.1
# In general, we just take the box URL, and then append the provider URL
# https://app.vagrantup.com/<organization name>/boxes/<box name>/versions/<version>/providers/<provider>.box
wget https://app.vagrantup.com/centos/boxes/7/versions/1905.1/providers/virtualbox.box -O CentOS-7-x86_64-Vagrant-1905_01.VirtualBox.box

# Then you can add it to Vagrant
vagrant box add centos/7 CentOS-7-x86_64-Vagrant-1905_01.VirtualBox.box
```

* 通过浏览器

  1. 打开网址： https://app.vagrantup.com/boxes/search

  2. 搜索box，如centos

  3. 点击选中的box（[centos](https://app.vagrantup.com/centos)/[7](https://app.vagrantup.com/centos/boxes/7)），进入详情页后再点击选中的版本（[v1905.1](https://app.vagrantup.com/centos/boxes/7/versions/1905.1)），跳转后拷贝浏览器上的URL，URL应该如下

     `https://app.vagrantup.com/centos/boxes/7/versions/1905.1`

  4. 根据以下格式再拼接provider

     ```
     https://app.vagrantup.com/<organization name>/boxes/<box name>/versions/<version>/providers/<provider>.box
     ```

     最终可下载的链接如下

     `https://app.vagrantup.com/centos/boxes/7/versions/1905.1/providers/virtualbox.box`



如无特殊情况，都以centos作为基础系统，下载链接如下：

`https://app.vagrantup.com/generic/boxes/centos7/versions/2.0.6/providers/virtualbox.box`

```
vagrant box add ./centos7.box --name generic/centos7
```



### 自定义Vagrant box

```
vagrant package --base virtualbox虚拟机实例的名字 --output 自定义.box
vagrant box add 自定义.box --name 自定义名称
```

在这里遇到了一个问题，未找到根本解决方案，但有临时方案。

##### 问题：使用新的box进行`vagrant up`启动时`vagrant ssh`失败

临时解决方案：

等待`vagrant up`的进程结束，再`vagrant ssh`，此时需要输入密码（一般是`vagrant`），进入控制台后执行以下命令

```
 wget --no-check-certificate https://raw.githubusercontent.com/hashicorp/vagrant/master/keys/vagrant.pub -O /home/vagrant/.ssh/authorized_keys  
 chmod 0700 /home/vagrant/.ssh  
 chmod 0600 /home/vagrant/.ssh/authorized_keys  
 chown -R vagrant /home/vagrant/.ssh  
```

使用`exit`退出，执行`vagrant reload	`

参考文章：https://www.gisremotesensing.com/2016/06/solution-vagrant-box-authentication.html