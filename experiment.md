# mint升级内核的方法

直接运行命令：

~~~
sudo apt-get install linux-image-5.13
~~~

输完上述命令后，按tab键可以列出可选择的列表，选择一个执行即可安装，然后重新开机后会自动选择这个内核，不用进行其他操作

# mint换国内软件源的方法

找到systemsetting的SoftwareSources，然后切换到国内源。切换完成以后，找到/etc/apt/sources.list.d/official-package-repositories.list文件中的最后两项还没有切换到国内源，依照前面几个写法，手动修改最后两项到国内源即可

# Mint双系统安装

1.硬盘分区

给硬盘添加压缩卷，给linux系统流出足够的空间，一般100G到200G之间就足够

2.制作启动u盘

利用工具Rufus制作U盘，链接：[Rufus](https://rufus.ie/de/),下载Mint镜像，链接：[Mint](https://www.linuxmint.com/edition.php?id=288),之后弹出的写入方式要以DD方式写入，否则会出错

3.进入bios选择u盘启动

4.开始安装mint，安装类型选择其他选项，注意此时应该有检测出windowsbootManager的选项，否则双系统无法安装成功

5.磁盘分区：设置/boot和/分区，给/boot 3G空间，然后剩下都留给/分区。每一个分区都选择逻辑分区，空间起始位置，然后选择对应挂载点即可

# 双系统linux引导修复

1. 用启动u盘进入到待安装的mint系统，连接好网络，输入：

   ```
   sudo add-apt-repository ppa:yannubuntu/boot-repair && sudo apt-get update
   ```

   添加boot-repair所在的源

2. 输入：

   ```
   sudo apt-get install -y boot-repair
   ```

   安装boot-repair工具

3. 输入boot-repair，启动工具，并选择recommended repair，然后开始修复，修复完成如果成功的话重新开机即可看到linux启动项