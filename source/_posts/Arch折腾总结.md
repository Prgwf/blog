---
title: Arch折腾总结
date: 2017-07-16 16:01:57
categories:
  - Linux
tags:
  - ArchLinux
---
### 分区和挂载
```bash
parted /dev/sda
    mklabel gpt
    mkpart ESP fat32 1M 513M
    set 1 boot on
    mkpart primary ext4 513M 20.5G
    mkpart primary linux-swap 20.5G 24.5G
    mkpart primary ext4 24.5G 100%

mkfs.ext4 -b 4096 /dev/sda2
mkfs.ext4 -b 4096 /dev/sda4
mkswap /dev/sda3
mkfs.vfat -F32 /dev/sda1

mount -t ext4 -o discard,noatime /dev/sda2 /mnt
mkdir -p /mnt/boot
mount /dev/sda1 /mnt/boot
mkdir -p /mnt/home
mount -t ext4 -o discard,noatime /dev/sda4 /mnt/home
swapon /dev/sda3
```

### 系统安装
```bash
pacstrap -i /mnt base base-devel vim
genfstab -U -p /mnt > /mnt/etc/fstab
```

### chroot
```bash
arch-chroot /mnt  /bin/bash

vim /etc/locale.gen
# en_US.UTF-8
# zh_CN.UTF-8
# zh_CN.GBK
# zh_CN.GB2312

locale-gen

echo LANG=en_US.UTF-8 > /etc/locale.conf

ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

passwd # root 密码

echo arch > /etc/hostname # 主机名称
vim /etc/hosts #加入同样的名字
```

### 引导
```bash
pacman -S grub os-prober ntfs-3g efibootmgr iw wpa_supplicant dialog
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub --recheck
grub-mkconfig -o /boot/grub/grub.cfg
```

### 添加用户
```bash
useradd -m -G wheel -s /bin/bash  [用户名]

passwd [用户名]

visudo
%wheel ALL=(ALL) ALL
```

### 添加archlinuxcn源
```bash
sudo nano /etc/pacman.conf
[archlinuxcn]
SigLevel = Optional TrustAll
Server = http://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch

sudo pacman -Syy
sudo pacman -S archlinuxcn-keyring yaourt
```

### 装X
```bash
sudo pacman -S xorg xorg-xinit xterm xorg-xeyes xorg-xclock
sudo pacman -S i3
sudo pacman -S lightdm-gtk-greeter
sudo systemctl enable lightdm
```

### 语言和输入法
```bash
vim ~/.xinitrc
export LANG=zh_CN.UTF-8
export LANGUAGE=zh_CN:en_US
export LC_CTYPE=en_US.UTF-8

sudo pacman -S fcitx-im fcitx-configtool

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
fictx &
```

### 亮度调节
```bash
cat  /sys/class/backlight/intel_backlight/max_brightness
echo 220 > /sys/class/backlight/intel_backlight/brightness
```

### 充电阈值
```bash
https://github.com/mkottman/acpi_call
https://github.com/teleshoes/tpacpi-bat

git clone https://github.com/teleshoes/tpacpi-bat  tpacpi-bat
sudo tpacpi-bat -v -s ST 1 50
# batt1 is going to change the start threshold to 50%.

sudo tpacpi-bat -v -s ST 2 50
# batt2 is going to change the start threshold to 50%.

sudo ./tpacpi-bat -v -s SP 0 90
# changes the stop at 90%.
```

### 字体
```bash
pacman -S wqy-microhei ttf-dejavu ttf-droid cantarell-fonts adobe-source-han-sans-cn-fonts
```

### 软件
- xfce4-terminal 终端
- thunar 文件管理
- dmenu 启动器
- Chromium(pepper-flash) 浏览器和FLASH插件
- Atom
- netease-cloud-music 黄易云
- pnmixer 音量调节
- feh 壁纸设置
- tlp 电源管理


参考：  
- 李天火 《Enjoy arch —— 安装arch》
- K.I.S.S《给妹子看的 Arch Linux 桌面日常安装》
