# catwrt_base

CatWrt_Base 是基于 [Lean's LEDE](https://github.com/coolsnowwolf/lede) 修改的发行版基础资源仓库，将二次编辑好的内容上传到本仓库再通过自动化脚本安装到对应位置实现预装到系统中。

相比 [miaoermua/CatWrt](https://github.com/miaoermua/CatWrt) 的二进制软件源仓库，这里就差不多是源代码了，不过都是一些修改向的脚本，本仓库不会存储二进制&闭源代码，也不会集成闭源代码到 CatWrt 中!

实现编译半自动化，版本管理，文件统一；脚本需要 root 运行，编译只能普通用户；资源文件和 LEDE 源码需要存储在 `/home` 才能正常食用。

**更新插件库**
```bash
sudo curl https://raw.githubusercontent.com/miaoermua/catwrt_base/main/pull.sh | sudo bash
```

**刷版本号** （更新 CatWrt 关键文件）
```bash
sudo curl https://raw.githubusercontent.com/miaoermua/catwrt_base/main/update_catwrtver.sh | sudo bash
```

---

## 准备环境

用于首次部署编译环境

```bash
sudo bash -c 'bash <(curl -s https://build-scripts.immortalwrt.org/init_build_environment.sh)'
```

```bash
sudo apt update -y
sudo apt full-upgrade -y
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libfuse-dev libglib2.0-dev libgmp3-dev \
libltdl-dev libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libpython3-dev libreadline-dev \
libssl-dev libtool lrzsz mkisofs msmtp ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 \
python3-pyelftools python3-setuptools qemu-utils rsync scons squashfs-tools subversion swig texinfo \
uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
```

## 更新源码

更新 LEDE 源码和执行 feeds 脚本更新源码仓库的插件

```bash
cd ~/lede
git pull
./scripts/feeds update -a
./scripts/feeds install -a
make defconfig
make download -j8
```

## 编译

```bash
make menuconfig
make V=s -j$(nproc)
```

[coolsnowwolf/lede: Lean's LEDE source](https://github.com/coolsnowwolf/lede)

[immortalwrt/immortalwrt: An opensource OpenWrt variant for mainland China users.](https://github.com/immortalwrt/immortalwrt)


## 解决问题

[Alist Golang](https://github.com/sbwml/packages_lang_golang)

[Easymesh - mt7621](https://github.com/coolsnowwolf/routing/pull/7)

[MosDNS - mt7621](https://github.com/coolsnowwolf/lede/issues/12117)

## 协议

CatWrt 和源码你可以自由使用 (GPL2.0) 任何责任需要自行承担，二次发版需要标注来源不得以影响 CatWrt 正式发版，在二次修改发布前需要了解 CatWrt 开发事项应避免关键服务后端 API 和二次开发版本冲突。
