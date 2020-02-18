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