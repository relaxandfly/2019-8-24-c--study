# 在ubuntu上安装并使用Spinnaker
***
## 0.前置工作
- 请您确认已经安装好ubuntu系统
- 系统的网络连接正常
- 建议配置一个高速的安装源 [点我进入参考页](https://www.cnblogs.com/gabin/p/6385800.html)
- 建议给ubuntu增加root用户 [点我进入参考页](https://blog.csdn.net/cfq1491/article/details/81062798)
#### 0.1注意
- 您安装的ubuntu系统应该是16.04（含）以上的版本
#### 0.2新手向
- 打开命令行用Ctrl+Alt+T
***
## 1. 下载spinnaker sdk
[点我进入下载页](https://flir.app.boxcn.net/v/SpinnakerSDK)
#### 1.1 注意
- 本文将以**spinnaker-1.24.0.60-amd64-Ubuntu16.04-pkg.tar.gz**为例，请您根据自己的实际情况，将下面代码中该名替换为您下载的包名。
#### 1.2 建议
英语好的用户请阅读包内的README文件

## 2.安装依赖项
#### 2.1ubuntu18.04适用

      sudo apt-get install libavcode57 libavformat57 libswscale4 libswresample2 libavutil55 libusb-1.0-0 libgtkmm-2.4-dev
#### 2.2ubuntu16.04适用
    sudo apt-get install synaptic
----
    y
----
    synaptic
----
在该包管理软件里搜索(**search**)并安装（**apply**)下面四个依赖
 >**libswscale-ffmpeg3**
**libavcodec-ffmpeg56**
**libavformat-ffmpeg56**
**libgtkmm-2.4-dev**

##3.提升系统内存能力（使用U3相机务必配置）
####3.1更改grub文件

      sudo gedit /etc/default/grub
将内容里的
>GRUB_CMDLINE_LINUX_DEFAULT ="**quiet splash**"

替换为
>GRUB_CMDLINE_LINUX_DEFAULT =“**quiet splash usbcore.usbfs_memory_mb = 1000**”

####3.2 更新grub文件

      sudo update-grub
####3.3 重启

    reboot
####3.4验证是否生效（返回值应为***1000***）

    cat /sys/module/usbcore/parameters/usbfs_momory_mb
## 4.安装spinview
#### 4.1新建一个文件夹
    sudo mkdir /opt/flir
#### 4.2将下载好的压缩包放入您新建的文件夹内
#### 4.3解压压缩包
    cd /opt/flir
---
      tar -zxvf spinnaker-1.24.0.60-amd64-Ubuntu16.04-pkg.tar.gz
---
    cd spinnaker-1.24.0.60-amd64
---
    sudo sh install_spinnaker.sh
----
    y
----
    rene（注意这里是添加自己的用户名，我的用户名是rene。不添加影响不大，请直接回车)
----
    y
----
    y
## 5.相机网卡参数的设置
#### 5.1查看欲配置的网口名称
    ifconfig
>我的返回结果是**enp15s0**，请您根据自己的结果酌情修改下面的代码

#### 5.2配置参数和IP地址

     sudo gedit /etc/network/interfaces
*编辑为
>iface **enp15s0** inet static
>
>address 169.254.0.64
>
>netmask 255.255.0.0
>
>mtu 9000
>
>
>Auto enp15s0
#### 5.3增加buffer size
* 查询目前网口的buffer size
---
    cat /proc/sys/net/core/rmem_max 
---
    cat /proc/sys/net/core/rmem_default
----
* 选项A：设置永久的buffer size
---
     sudo gedit /etc/sysctl.conf
请您将以下内容增加到文件最底部
>net.core.rmem_max=10485760
>
>net.core.rmem_default=10485760

检验成果

---
    sudo sysctl -p


---
* 选项B：设置临时的buffer size
---
     sudo sysctl -w net.core.rmem_max=10485760
---

    sudo sysctl -w net.core.rmem_default=10485760
#### 5.4 重启
    reboot

## 6.运行spinview并调整packet size
####6.1连接相机
####6.2启动spinview
    sudo spinview
#### 6.3调整SCPS Packet Size值为9000
#### 6.4通过调整Device Link Throughput Limit来减少延迟时间(越小则延时越长)




-----

*本人电话：17093820585*
*欢迎咨询探讨*
