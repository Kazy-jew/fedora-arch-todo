# 安装完fedora后要做的事

![效果图](Images/Screenshot%20from%202022-09-08%2004-10-39.png)
## 0.代理
[pip相关issue](https://zhuanlan.zhihu.com/p/350015032)和[这里](https://github.com/pypa/pip/issues/9216), [pip相关issue备份](https://web.archive.org/web/20220907164331/https://zhuanlan.zhihu.com/p/350015032)
**0.1 python**
```
from urllib.request import getproxies
```
⸸<font size=2>windows添加环境变量`HTTP_PROXY` & `HTTPS_PROXY`: `http://127.0.0.1:7890`</font>
**0.2. dnf**
```
/etc/dnf/dnf.conf
proxy=http://127.0.0.1:7890
fastestmirror=1
```
## 1.设置键盘快捷键
### 1.1 nautilus
 `/bin/nautilus`
### 1.2 terminal 
`/bin/gnome-terminal`

##2.grub多系统引导，主题, [GNU参考](https://www.gnu.org/software/grub/manual/grub/grub.html)
### 2.1　启动项相关配置(没有对应行添加即可)
```
vim /etc/default/grub
```
### 2.2　查看默认启动项：
```
grub2-editenv list
```
### 2.2　查找启动入口：
```
awk -F\' '/menuentry / {print $2}' /boot/grub2/grub.cfg
```

或者(查找windows)
```
cat /boot/grub2/grub.cfg | grep "Windows"
```
### 2.3 设置默认启动项
```
GRUB_DEFAULT="Windows Boot Manager (on /dev/nvme0n1p1)"
GRUB_SAVEDEFAULT=true
```
###　2.４ 隐藏菜单相关(图形化输出需注释)
```
GRUB_HIDDEN_TIMEOUT
```
###　2.５ 图形化输出
```
GRUB_TERMINAL_OUTPUT="gfxterm"
```
设置屏幕分辨率, default为1024x768
```
GRUB_GFXMODE=1024x768x32,auto
```
####　2.5.ex
fedora 3x grub 开启图形化输出会报错:`xxx not found:`
```
/EFI/fedora/fonts/unicode.pf2
/EFI/fedora/grubenv
```
临时解决方法:将缺失的文件复制到对应的目录
```
cp /usr/share/grub/unicode.pf2 /boot/efi/EFI/fedora/fonts/
cp /boot/grub2/grubenv /boot/efi/EFI/fedora/
```

###　2.６ grub主题
[Sleek GrubBootloader themes](https://www.gnome-look.org/p/1414997)
[Grub-theme-vimix](https://www.gnome-look.org/p/1009236)
[Tela grub theme](https://www.gnome-look.org/p/1307852)
[13atm01](https://github.com/13atm01/GRUB-Theme)
下载完成后,解压到对应位置即可(一般放在`/usr/share/theme`下)
需要在`/etc/default/grub`里添加
`GRUB_THEME="/boot/gurb2/themes/${theme folder}/theme.txt"`

###　2.７ 生成新配置
BIOS：
```
grub2-mkconfig -o /boot/grub2/grub.cfg
```
UEFI:
```
grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```

## 3.安装fedy
[Homepage](https://github.com/rpmfusion-infra/fedy)

### 3.ex
chrome managed by organization
```
sudo rm -rf /etc/opt/chrome
```

## 4.Gnome相关

### 4.1字体
```
cp your/fonts/path/fonts /usr/share/fonts/
fc-cache -f -v
```
### 4.2 Gnome扩展
gnome-shell-extension list
#### dock
[shell configrator](https://extensions.gnome.org/extension/4254/shell-configurator/)
[dash-to-dock](https://extensions.gnome.org/extension/5004/dash-to-dock-for-cosmic/)
[ArcMenu](https://extensions.gnome.org/extension/3628/arcmenu/)
[Clipboard Indicator](https://extensions.gnome.org/extension/779/clipboard-indicator/)
[Vitals](https://extensions.gnome.org/extension/1460/vitals/)
[UserThemes](https://extensions.gnome.org/extension/19/user-themes/)
[appindicator](https://extensions.gnome.org/extension/615/appindicator-support/)
[application menu](https://extensions.gnome.org/extension/6/applications-menu/)
[place indicator](https://extensions.gnome.org/extension/8/places-status-indicator/)
[just perfection](https://extensions.gnome.org/extension/3843/just-perfection/)
[open weather](https://extensions.gnome.org/extension/750/openweather/)

### 4.3 Shell 和 Icon 主题
[Mojave CT icons](https://www.gnome-look.org/p/1210856/)
[OS Catalina](https://www.gnome-look.org/p/1309810/)
[mcOS Mojave for Plank Dock](https://www.gnome-look.org/p/1248226/)
[WhiteSur Shell](https://www.gnome-look.org/p/1403327)
[McMojave](https://www.gnome-look.org/p/1275087/)
[Layan gtk theme](https://www.gnome-look.org/p/1309214/)
[WhiteSur Gtk Theme](https://www.gnome-look.org/p/1403328/)
[Ant Themes](https://www.gnome-look.org/p/1099856/)

```
tar -xf file.name.tar -C /usr/share/themes
```
```
for f in *.tar.xz; do tar -xf "$f" -C /usr/share/themes --overwrite; done
```

```
tar -xf file_name.tar -C /target/directory
```

```
dnf config-manager --set-disabled
dnf config-manager --set-enabled
screenfetch
dnf install gnome-tweaks
```
### 4.4 窗口开启最小化/最大化按钮
```
gsettings set org.gnome.desktop.wm.preferences button-layout ":minimize,maximize,close"
```

### 4.5 缩放
* wayland 开启 fractional scaling feature⸸:
  `gsettings set org.gnome.mutter experimental-features "['scale-monitor-framebuffer']"` 
    ⸸<span style="color:red"><font size=2> 会导致字体显示模糊, 建议切换到xorg</font></span>

### 4.6 更改terminal/nautilus窗口打开的位置
```
gnome-terminal --geometry=100x35+10+10
```
  ⸸<span style="color:red"><font size=2> 尝试后发现只能改大小不能更改位置</font></span>
CompizConfig Settings Manager (CCSM)

## 5.系统相关

### 5.0 和windows共存

#### 5.0.1 关闭windows的fastboot和hibernate, 至少关闭fastboot,如果有以下问题
* wifi模块未加载(`gnome`设置里网络没有wifi选项,`terminal`输入`lspci | grep Network`可以看到wifi相关硬件,但是`rfkill list`或`iwctl`-`device list`没有相关设备
* 声音模块,同上
* 写入ntfs文件系统的数据丢失(一般不会,`ntfs-3g`在windows的休眠状态下会挂载为只读)

#### 5.0.2 时间设置
系统开机时都会首先读取主板上的硬件时间作为系统时间， 所不同的是，windows默认硬件时间为本地时间，而linux一般会默认硬件时间为UTC时间 (然而fedora 36似乎默认的也是本地时间，不知道是不是因为在安装时检测到了Windows系统后作的更改）。一般为了不变性更推荐使用UTC做为默认时间。
Windows修改为UTC的方法可参考[archwiki](https://wiki.archlinux.org/title/System_time#UTC_in_Microsoft_Windows)，Fedora可以用`timedatectl`查看一下，
若`RTC in local TZ`行的值为真，则用`timedatectl set-local-rtc false`修改即可

### 5.1 开机加载硬盘
* 一些相关命令`gdisk`,`parted`,`lsblk`,`df`等等
* 列出(所有)硬盘列表`fdisk -l /dev/sda`
* `vim /etc/fstab`
```
UUID="c2dbc0c5-a8fc-439e-aa93-51b0a61372e8" /mnt/ntfs/ ntfs nls-utf8,umask-0222,uid-1000,gid-1000,ro 0 0
```
[转载](https://blog.51cto.com/zkxfoo/1758529)
```
设备     挂载点   文件系统类型  挂载选项   转储频度   自检次序

（1）要挂载的设备：

        设备文件、LABEL=, UUID=
（2）挂载点：

        swap没有挂载点，挂载点为swap
（3）文件系统类型

        ext2、ext3、ext4、xfs、nfs、smb、iso9660等
（4）挂载选项：多个选项间使用逗号分隔;

        async、sync、_netdev

        defaults（ rw,  suid, dev, exec, auto, nouser, async, and relatime.）
（5）转储频率：
         0：从不备份
         1：每日备份
         2：每隔一天备份
（6）自检次序：
         0: 不自检
         1：首先自检，通常只能被/使用；
         2：等数字为1的自检完成后，再进行自检
```
### 5.1 自定义systemd服务
`usr/lib/systemd/system`

### 5.2 开机启动脚本
`crontab -e`
`@reboot /path/to/your/script.sh`

### 5.3 输入法
从`ibus`切换到`fcitix5`，参见[这里](https://insidelinuxdev.net/article/a0cr1x.html)和[这里](https://yanqiyu.info/2020/11/06/fcitx5-fedora-updated/)
```
$ sudo dnf install fcitx5 kcm-fcitx5 fcitx5-chinese-addons fcitx5-table-extra fcitx5-configtool fcitx5-autostart
```

## EX.其他

### ex1.git
windows下基本都是用一些图形化的客户端(fork, github desktop等等), 由于linux本身集成了git而且图形化的客户端都比较一般，因而列举下常用的命令。