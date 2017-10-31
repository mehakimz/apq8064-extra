### apq8064-extra
### kernel and extra modules for dogo

You will find here several different configuration of kernel and extra modules for Sony Xperia ZR device known as dogo with [CyanogenMod firmware](https://wiki.cyanogenmod.org/w/About) - an enhanced open source, community built distribution of Android.

If you have or need to install extra linux distribution like debian or kali in `chroot env`, these kernel and extra modules will help you to extend the capabilities of your device. The kernel compiled base on kali-nethunter configuration. See their wiki page on [Porting Nethunter](https://github.com/offensive-security/kali-nethunter/wiki/Porting-Nethunter)


#### Folders Structure

```
apq8064-extra
│
├─ kernel
│  └─ los-14.1
│     ├─ boot_full.img
│     ├─ boot_wifi.img
│     └
│
└─ modules
   ├─ backports
   │  ├─ compat.ko
   │  ├─ cfg80211.ko
   │  ├─ mac80211.ko
   │  ├─ rt2x00lib.ko
   │  ├─ rt2800lib.ko
   │  ├─ rt2x00usb.ko
   │  ├─ rt2800usb.ko
   │  ├─ ~~rtl8xxxu.ko~~
   │  ├─ wcn36xx_msm.ko
   │  └─ wcn36xx.ko
   │
   └─ los-14.1
      ├─ cfg80211-main.ko
      ├─ wlan.ko
      ├─ ~~pn533.ko~~
      └─ ~~pn544.ko~~
```
> modules backported from `--git-revision v3.19` linux-next for Ralink and Realtek chip support

#### Supports & Issues

* Compillation status:
> Lineageos version: 14.1-20171030-NIGHTLY-dogo

> Kernel version: 3.4.113-dogo-gd710842

> Backports branch: linux-3.18.y

> Kernel extra: 3.4.113-lineageos-gcab505550

* Difference between Kernels:
 * boot_full.img Supports for kali Nethunter, HID gadget (keyboard and mouse) and all modules are built-in. Patched for kernel panic related to rtlwifi driver as [ref.](https://github.com/offensive-security/kali-nethunter/issues/624)

 * boot_wifi.img Supports for backported wifi drivers.

To use the internal wifi [prima driver](modules/los-14.1), load 802.11 configuration (cfg80211-main.ko) then wlan.ko:
```
$busybox insmod /system/lib/modules/cfg80211-main.ko
$busybox insmod /system/lib/modules/wlan.ko
```
for backported drivers [like RT5572](modules/backports):
```
$busybox rmmod wlan
$busybox rmmod cfg80211
$busybox insmod /system/lib/modules/compat.ko
$busybox insmod /system/lib/modules/cfg80211.ko
$busybox insmod /system/lib/modules/mac80211.ko
$busybox insmod /system/lib/modules/rt2x00lib.ko
$busybox insmod /system/lib/modules/rt2800lib.ko
$busybox insmod /system/lib/modules/rt2x00usb.ko
$busybox insmod /system/lib/modules/rt2800usb.ko
```

* Currently USB wifi modules based on RT33xx, RT35xx, RT53xx, RTL8192CU supported by boot-full.img and for newer RT3573 and RT5572 chip use boot-wifi.img and rt2800usb in backports. Also you need copy proper firmware(s) - rt2870.bin to /system/etc/firmware path. (tested on "D-Link DWA-160 rev.B2" - RT5572)

* Internal wifi chip "wcn3660" open-source driver backported from "v3.18" and added to modules tree. Monitor mode can be enabled on interface but nothing will be captured. It's firmware issue.

* HID gadget support based on patch from android-keyboard-gadget project. [more information](https://github.com/pelya/android-keyboard-gadget)

* Patched for [Only expose su when daemon is running](https://review.lineageos.org/#/c/170648) and [SU kernel hiding patch](_)

* When you `insmod somemodules.ko` maybe you would see "failed to load somemodules.ko: Bad address". Install latest busybox and use `$busybox insmod` for loading modules.

