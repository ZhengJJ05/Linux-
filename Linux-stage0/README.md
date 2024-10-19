# 事前准备

## 下载镜像
    推荐在官网下载，第三方镜像源可能参杂“后门”
    这里我使用版本为Ubuntu-24.04-live-server-amd64的iso文件
 附上官方网址：[Ubuntu](https://ubuntu.com/download/server)
 ![https://ubuntu.com/download/server](/Linux-stage0/img/Ubuntu-24.04-live-server-amd64.PNG)

## 虚拟机下载
    常用虚拟机有很多种
    这里我推荐两种
    1. VirtualBox
    2. VMware Workstation Pro

# 镜像安装
    我将会选用VM作为本次实验以及后续的实验的虚拟机软件

## 实验虚拟机版本
![VM17Pro版本](/Linux-stage0/img/VM17%20Pro.PNG)
    详细的安装教程附上博客链接自行查阅
 [转载自CSDN（查阅请点我）](https://blog.csdn.net/m0_74860678/article/details/140565137?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522FBC80D5D-69F9-44A9-99D7-D7031BAF0709%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=FBC80D5D-69F9-44A9-99D7-D7031BAF0709&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-3-140565137-null-null.142^v100^pc_search_result_base4&utm_term=vm%20Ubuntu-24.04-live-server-amd64%E5%AE%89%E8%A3%85&spm=1018.2226.3001.4187)

# SSH
    在虚拟机上安装Linux时一定要选择安装OpenSSH
 ![SSH](/Linux-stage0/img/SSH.PNG)

## ssh远程登陆

    虚拟机的Linux需要提前打开

### Xshell
   点击新建 创建新的会话
   ![Xhell](/Linux-stage0/img/SSH1.PNG)

   主机填入虚拟机分配的ipv4地址
   ![Xhell](/Linux-stage0/img/SSH2.PNG)

   填入创建Linux时的账户密码即可远程登入
   ![Xhell](/Linux-stage0/img/SSH3.PNG)

### windows自带的cmd/power shell
   以cmd为例
   win+R 输入 cmd 回车
   在命令窗口中输入 ssh 登入的账户名@主机地址

   ![cmd](/Linux-stage0/img/cmd.PNG)
    登入成功
    ![cmd](/Linux-stage0/img/cmd2.PNG)
   因为我已经配置了免密登入所以不需要输入密码

## ssh免密登入
    作为示例我会用cmd和power shell两种同时进行
    首先需要ssh远程登入Linux主机
    下图左边窗口为cmd（已登入Linux）右边窗口为power shell（无操作）
 ![ssh免密登入](/Linux-stage0/img/ssh免密登入1.PNG)
    在Linux中通过 shh-keygen -t 4096 命令生成公私钥用于身份验证
 ![ssh免密登入](/Linux-stage0/img/ssh免密登入2.PNG)
    第一次回车是确认公钥的存储位置 回车三次 就创建成功了 
    复制公钥的路径一般为/home/ubuntu/.ssh/id_rsa.pub 但是最后的文件因人而异因为我的是id_ed25519.pub
 ![ssh免密登入](/Linux-stage0/img/ssh免密登入3.PNG)
    至此Linux的操作就结束了，接下来转入power shell
    在power shell中输入ssh-copy-id -i 公钥路径 用户名@主机地址
 ![ssh免密登入](/Linux-stage0/img/ssh免密登入4.PNG)
    如果你像我一样出现无法识别
 ![ssh免密登入](/Linux-stage0/img/ssh免密登入5.PNG)
    那么在power shell中先输入以下代码再输入ssh-copy-id命令就可以解决
    ```function ssh-copy-id([string]$userAtMachine, $args){
        $publicKey = "$ENV:USERPROFILE" + "/.ssh/id_rsa.pub"
        if (!(Test-Path "$publicKey")){
            Write-Error "ERROR: failed to open ID file '$publicKey': No such file"
        }
        else {
            & cat "$publicKey" | ssh $args $userAtMachine "umask 077; test -d .ssh || mkdir .ssh ; cat >> .ssh/authorized_keys || exit 1"
        }
    }
    ```
 ![ssh免密登入](/Linux-stage0/img/ssh免密登入6.PNG)
    再次输入ssh-copy-id命令
 ![ssh免密登入](/Linux-stage0/img/ssh免密登入7.PNG)
    如果和我一样报警告找不到文件或者文件夹 不用管
    此时输入ssh 用户名@主机地址 你就会发现不用输入密码直接成功登入！
 ![ssh免密登入](/Linux-stage0/img/ssh免密登入8.PNG)

# 恭喜你完成Linux的入门设置
 

    


