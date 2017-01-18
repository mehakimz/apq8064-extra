### apq8064-extra
### kernel and extra modules for dogo

You will find here several different configuration of kernel and extra modules for Sony Xperia ZR device known as dogo with [CyanogenMod firmware](https://wiki.cyanogenmod.org/w/About) - an enhanced open source, community built distribution of Android.

If you have or need to install extra linux distribution like debian or kali in `chroot env`, these kernel and extra modules will help you to extend the capabilities of your device. The kernel compiled base on kali-nethunter configuration. See their wiki page on [Porting Nethunter](https://github.com/offensive-security/kali-nethunter/wiki/Porting-Nethunter)


#### Folders Structure

```
apq8064-extra
│
├── kernel
│   ├
│   └── cm-13.0
│       ├── boot_full.img
│       ├── boot_wifi.img
│       └
│
└── module
    ├── backports
    │   ├── compat.ko
    │   ├── cfg80211.ko
    │   ├── mac80211.ko
    │   ├── wcn36xx_msm.ko
    │   └── wcn36xx.ko
    │
    └── cm-13.0
        ├── cfg80211-main.ko
        ├── wlan.ko
        ├── ~~pn533.ko~~
        └── ~~pn544.ko~~
```

modules backported from `--git-revision next-20160324` linux-next for Ralink and Realtek chip support
```
	- rt2x00lib.ko
	- rt2800lib.ko
	- rt2x00usb.ko
	- rt2800usb.ko

	- rtlwifi.ko
	- rtl_usb.ko
	- rtl8192c-common.ko
	- rtl8192cu.ko
```

#### Supports & Issues

* Compillation status
...CyanogenMod version: 13.0-20161218-NIGHTLY-dogo
...Kernel version: 3.4.112-cm-g79fe27ede33
...Backport tag: linux next-20160324

* Difference between Kernels

**boot_full.img**
Supports for kali Nethunter, HID gadget (keyboard and mouse) and all modules are built-in.

**boot_wifi.img**
Supports for backported wifi drivers.
802.11 configuration (cfg80211-main.ko) & wcn3660 driver (wlan.ko) for default wireless interface:
```
$busybox insmod /system/lib/modules/cfg80211-main.ko
$busybox insmod /system/lib/modules/wlan.ko
```
for backported derivers (like RT5572 support):
```
$busybox rmmod cfg80211
$busybox rmmod cfg80211
$busybox insmod /system/lib/modules/compat.ko
$busybox insmod /system/lib/modules/cfg80211.ko
$busybox insmod /system/lib/modules/mac80211.ko
$busybox insmod /system/lib/modules/rt2x00lib.ko
$busybox insmod /system/lib/modules/rt2800lib.ko
$busybox insmod /system/lib/modules/rt2x00usb.ko
$busybox insmod /system/lib/modules/rt2800usb.ko
```

* Currently USB wifi module based on RT33xx, RT35xx, RT53xx, RTL8192CU supported by boot-full.img and for newer RT3573 and RT5572 chip use rt2800usb in backports. (tested on "D-Link DWA-160 rev.B2" - RT5572) 

* Internal wifi chip "wcn3660" open-source driver backported from "next-20160324" and added to modules tree. Monitor mode can be enabled on interface but nothing will be captured by `tshark`. It's firmware issue.

* HID gadget support based on patch from android-keyboard-gadget project. [more information](https://github.com/pelya/android-keyboard-gadget)

* When you `insmod somemodules.ko` maybe you would see "failed to load somemodules.ko: Bad address". Use `insmod` inside of `chroot env`

