- ## 定时更新固件测试，一定要先设置好发布密匙，因为检测更新是检测github的发布地址，settings.ini文件里面可以设置编译的时候是否要编译上定时更新的开关，默认是开启的

- ### 理论上在openwrt-系统-备份/升级里能后台升级的机子都支持定时更新的

- ### 测试方法，分别在两个时间段启动编译，比如1:00启动一个编译,然后1:05分启动一个编译，完成后，安装1:00的，就会自动检测到1:05的
#
- ### 在diy-3.sh里面需要修改的东西
```
Author="281677160"       你的帐号
Default_Device="x86-64"          机型名字，看下面说明
Updete_firmware="openwrt-x86-64-combined-squashfs.img.gz"  安装固件名字,一定要正确填写,源码不一样名字不一样
Extension=".img.gz"              安装固件的扩展（后缀），一定要正确填写
Source="lede"               源码作者名字，随便写，用来区分源码的

```

---
- 机型名字可以看.config的第三行
```
CONFIG_TARGET_rockchip=y
CONFIG_TARGET_rockchip_armv8=y
CONFIG_TARGET_rockchip_armv8_DEVICE_friendlyarm_nanopi-r2s=y

- 上面的就是机型文件的三项，找到 DEVICE_ 跟着的就是机型名字，后面的=y不要，就是friendlyarm_nanopi-r2s
- 就我知道没有明确机型名字的有X86跟N1，x86的填写x86-64或者x86-32根据你编译的填写
```

---

- ### 自动在你现有的.config配置增加以下三个插件，免除SSH进去选择
```
luci-app-autoupdate=y    定时更新插件
luci-app-ttyd=y        openwrt内置SSH命令窗
IMAGES_GZIP=y         把img压缩成img.gz的

#
如果有机子是需要.img格式而不是.img.gz的话，可以到diy-3.sh删除以下两行代码：
找到Diy_Part1在里面有

sed -i '/IMAGES_GZIP/d' .config > /dev/null 2>&1
echo -e "\nCONFIG_TARGET_IMAGES_GZIP=y" >> .config

以上这两行代码，删除了就可以了

```
---
- ### 使用命令更新固件脚本和扩展空间:
```
首先需要打开 Openwrt 主页,点击系统-TTYD 终端或命令窗,按需输入下方指令:

检查更新(保留配置): bash /bin/AutoUpdate.sh

检查更新(不保留配置): bash /bin/AutoUpdate.sh -n


使用一键扩展内部空间\挂载 Samba 共享脚本:
同上方操作,打开TTYD 终端或命令窗,输入bash /bin/AutoBuild_Tools.sh
```
---
- ### 定时更新设置:
- 首先需要打开 Openwrt 主页,点击系统-定时更新 ，定时更新勾选上，设置好时间，保存设置就可以了


---
## 鸣谢

   - [Hyy2001X](https://github.com/Hyy2001X/AutoBuild-Actions)

   - [dhxh](https://github.com/dhxh/Openwrt-Build)

   - 定时更新脚本是由[dhxh](https://github.com/dhxh/Openwrt-Build)从[Hyy2001X](https://github.com/Hyy2001X/AutoBuild-Actions)基础修改完成
2
