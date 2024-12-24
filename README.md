# 欢迎来到 Lean 的 LEDE 源码仓库 YvanW 的分支

> Lean 大佬的仓库[地址](https://github.com/coolsnowwolf/openwrt)

## 此仓库提供Github Action编译方法

- ### 编译命令

1. 首先打开 Github Spaces

2. 安装编译依赖

   ```bash
   sudo apt update -y
   sudo apt full-upgrade -y
   sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
   bzip2 ccache clang cmake cpio curl device-tree-compiler flex gawk gettext gcc-multilib g++-multilib \
   git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libfuse-dev libglib2.0-dev libgmp3-dev \
   libltdl-dev libmpc-dev libmpfr-dev libncurses-dev libncurses-dev libpython3-dev libreadline-dev \
   libssl-dev libtool llvm lrzsz genisoimage msmtp ninja-build p7zip p7zip-full patch pkgconf python3 \
   python3-pyelftools python3-setuptools qemu-utils rsync scons squashfs-tools subversion swig texinfo \
   uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
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


- ### 如果需要重新配置
   ```bash
   rm -rf .config
   make menuconfig
   make V=s -j$(nproc)
   ```