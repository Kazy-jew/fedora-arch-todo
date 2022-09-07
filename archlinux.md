# 安装完Archlinux之后。。。

## 0. 安装
本来以为安装会很复杂，不过可能是之前折腾过Fedora很多次了，感觉并没有想像中的困难，下载镜像[制作启动U盘](https://wiki.archlinux.org/title/USB_flash_installation_medium#Using_basic_command_line_utilities)后(我直接在Fedora下用的dd命令)，按照[Installation guide](https://wiki.archlinux.org/title/Installation_guide)一步一步来就可以了, 重新熟悉了下fdisk的磁盘分区，发现现在fdisk已经可以支持GPT的磁盘了，引导的话现在应该基本都是UEFI了，不会像以前的win8刚出来的时候，出现用传统模式装在UEFI上，结果装完fedora电脑就开不了机的情况。这里直接按照Fedora的默认的分区模式，磁盘分了三个区，`\boot`，`\boot\efi`，`\home`，由于内存有64GiB，没有创建`\swap`分区，分区完用`mkfs`格式化后结构如下：
```
\nvmes0\1        \boot
\nvmes0\2        \boot\efi
\nvmes0\3        \home
```
不过途中还是有个小插曲，根据[安装指导](https://wiki.archlinux.org/title/Installation_guide#Mount_the_file_systems), 新系统根目录要挂载在Live OS的`\mnt`下，我第一次安装的时候没有创建文件夹，直接挂在了根目录下`\`，结果后续运行`# pacstrap /mnt base linux linux-firmware`安装新系统时目录结构冲突，导至新系统装不上，只好乖乖地完全按照安装指导重来了一次，不过这里挂载点只要不冲突应该是随意的，`\mnt\abcde`或者`\666`什么的结果应该都是一样的。安装完，重启，进Fedora更新了下`grub.cfg`，找到Archlinux的入口，进入，输入用户密码，登录成功，
基本的安装也就结束了。不过~~折腾~~美化之路才刚刚开始。。。

## 1.安装之后

### 1.1 网络问题
没想到最大的问题和N年第一次安装Fedora一样。。。当年在华硕的笔记本上装完fedora 21后，系统设置里怎么都没有wifi标志，为此在网上搜索了无数的帖子加了N个群直到某一天偶然在群里看到一位老哥的发言才搞定(好像是要卸载掉acer的wifi模块，估计是这个模块和asus的wifi模块冲突)。虽然这次是用的是台式机, 不过还是用的Wifi(毕竟wifi不用拉线。。。所以网线什么的也没有)，登陆后准备安装`Gnome`桌面然而`pacman`一直提示Time Out才发现没有连上网。。。想着用`iwctl`连网，结果由于完全照搬的[安装指导的命令](https://wiki.archlinux.org/title/Installation_guide#Install_essential_packages)，只安装了最基础的模块，`iwd`也没有，真是~~似曾相识又~~~~喜闻乐见~~的发展。。。
但是我已经不是N年前的我了！一番~~搜索~~思考之后，发现可以和安装新系统时一样，进入Live OS，用`iwctl`连上wifi后重新挂载新系统根目录`# mount --mkdir /dev/efi_system_partition /mnt/boot`，再`# arch-chroot /mnt`切换到新系统的`root`账户进行相关软件的安装。于是我如上安装了`iwd`，重新进入系统敲入`iwctl`准备连接wifi -- 
然后这个程序卡住了。。。卡住了。。。直到现在我都不知道原因是什么，只记得当时没有搜到有用的结果。。。~~(逐渐开始焦燥)~~
解决无果，想着怎么着也得把个GUI搞出来，不然这系统装的也太没有成就感，于是又重复了一遍Live OS的流程，把`Gnome`的环境装了,重新进入系统，嗯，至少有GUI，看上去是那么一回事了，点开设置，还是没有wifi，但是这次桌面提示没有`Network Manager`服务，这一下子Shed Light On Me，`systemctl enable NetworkManger.service`，提示没有这个服务，又重走了下Live OS的流程，安装`NetworkManager`完，进系统后还是没有wifi，意识到可能是服务没有启动，再`systemctl enable`下，wifi标志终于跳出来了。。。可喜可贺。。。

#### 1.1.x其他

现在由于[Clash for Windows](https://docs.cfw.lbyczf.com/)有Linux版本，安装完`flatpak`，再`flatpak install Clash`，代理的问题相比之前还是容易不少。只是又不知道由于什么不可以描述的原因，Arch下面的chrome和opera都无法使用系统代理(但是firefox可以)，于是只好用firefox去[Crx4Chrome](https://www.crx4chrome.com)下载个离线的`SwitchyOmega.crx`，`unzip`到文件夹，再到浏览器的`extension`页面打开开发者模式加载解压缩的文件夹，再进行后续操作。


### 1.2 Gnome