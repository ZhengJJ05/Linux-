# 制作Ubuntu-24.04-live-server-amd64的无人值守镜像文件
##
过程中有些步骤冗余，可自行选择更快地方法达到相同目的
## 事前准备
安装Linux
在stage0中已经演示过一遍，不再重复
[点我进入stage0](https://github.com/ZhengJJ05/Linux-Beginer/tree/stage0/Linux-stage0)

## 创建 Cloud-Init ISO
登入Linux 并准备以下两个文件
首先切换到存储 ISO 镜像的目录,这是因为我们想在那里创建所需的文件

    cd /mnt/pve/ISOimages/template/iso
 如果找不到文件可以自行创建或者自行选择别处
1. meta-data（只是空的）
2. user-data（包含带有 Autoinstall 数据的特定部分）

### meta-data
创建空文件：meta-data

    touch meta-data


### user-data
首先我们需要从手动安装好的Ubuntu Linux中获取/var/log/installer/autoinstall-user-data
这个文件就是刚才手动配置生成的自动安装用的文件
不能直接使用它 我们需要手动修改这个文件
我们需要把/var/log/installer/autoinstall-user-data这个文件传到windows上
打开cmd或者power shell 利用ssh的scp传输文件

    scp root@主机地址:/var/log/installer/autoinstall-user-data ./
用文本编辑器打开并(根据个人情况)修改
修改后：

    #cloud-config
    autoinstall:
    version: 1
    identity:
      hostname: jj
      realname: JJ
      username: jj
      password: $6$4phqO8VayTuH60YE$yVpQAgALxC.ASqFAwyNq.bhmj8uYDlAgTCOVqPia77P0YEUIwIFOSJR7zylXIXwbxwKjRCxKFlwjjqcRf3..o.
    storage:
      layout:
        name: lvm
    ssh:
      install-server: yes


在Linux中创建user-data并将修改后的autoinstall-user-data内容复制进user-data

    vim user-data

### Cloud-Init ISO
    mkisofs -V cidata -lJR -o cloud-init.iso user-data meta-data

将创建好的cloud-init.iso传到windows中

     scp root@主机地址:/mnt/pve/ISOimages/template/iso/cloud-init.iso ./

## 使用双镜像进行无人值守创建虚拟机

创建虚拟机的步骤和先前一模一样
只需要在选择镜像时额外添加一个cloud-init.iso镜像文件
一定要把ubuntu-24.04-live-server-amd64.iso放在一号位把cloud-init.iso放在二号位
![双镜像设置](/image/vm1.PNG)

## 进行无人值守安装Linux

启动虚拟机
选择第一个选项
等待一段时间会出现是否继续自动安装
输入yes即可
![无人值守](/image/vm2.PNG)






参考链接：
[eploy Ubuntu 24.04 (Noble Numbat) with Autoinstall to Proxmox](https://sekureco42.ch/posts/deploy-ubuntu-24.04-with-autoinstall-to-proxmox/)
[Ubuntu server版本无人值守安装自动安装](https://blog.csdn.net/weixin_49393427/article/details/123505287)
[Ubuntu22.04应答文件](http://lujinkai.cn/%E8%BF%90%E7%BB%B4%E8%87%AA%E5%8A%A8%E5%8C%96/%E7%B3%BB%E7%BB%9F%E9%83%A8%E7%BD%B2/Ubuntu22.04%E5%BA%94%E7%AD%94%E6%96%87%E4%BB%B6/#%E8%87%AA%E5%8A%A8%E5%AE%89%E8%A3%85%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8)






    

    