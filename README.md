# 欢迎来到LEDE 源码Github Action编译仓库

如何使用Github Action编译自己需要的 Openwrt 固件

## 注意

默认登陆IP 192.168.1.1 密码 password

## 编译命令

1. 首先打开 Github Spaces

2. 安装编译依赖

   ```bash
   sudo apt update -y
   sudo apt full-upgrade -y
   sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
   bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
   git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
   libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
   mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pyelftools \
   libpython3-dev qemu-utils rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip \
   vim wget xmlto xxd zlib1g-dev python3-setuptools
   ```

3. 下载源代码，更新 feeds 并选择配置

   ```bash
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   make menuconfig
   ```
4. 提取本地自定义固件和默认配置的差异
   ```bash
   make defconfig
   ./scripts/diffconfig.sh > diff.config
   ```
5. 在 OpenWrt-CI.yml 中添加自定义配置
   ```bash
      - name: Generate configuration file
        run: |
          rm -f ./.config*
          touch ./.config
          cat >> .config <<EOF
          #
          # ========================固件定制部分========================
          #
          # 删除这一行，粘贴为 diff.config 中的内容，注意对其
          #
          # ========================固件定制部分结束========================
          #
          EOF
          sed -i 's/^[ \t]*//g' ./.config
          make defconfig
   ```


## 软路由介绍

硬酷R2 - N95/N300迷你四网HomeLab服务器

(商品介绍页面 - 硬酷科技（支持花呗）)：

[预售链接](https://item.taobao.com/item.htm?ft=t&id=719159813003)

[![r1](doc/r1.jpg)](https://item.taobao.com/item.htm?ft=t&id=719159813003)
