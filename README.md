----------------------------------------------------------------------------------
AnyKernel3 - Flashable Zip Template for Kernel Releases with Ramdisk Modifications
----------------------------------------------------------------------------------
### by osm0sis @ xda-developers ###

"AnyKernel is a template for an update.zip that can apply any kernel to any ROM, regardless of ramdisk." - Koush

AnyKernel2 pushed the format further by allowing kernel developers to modify the underlying ramdisk for kernel feature support easily using a number of included command methods along with properties and variables to customize the installation experience to their kernel. AnyKernel3 adds the power of topjohnwu's magiskboot for wider format support by default, and to automatically detect and retain Magisk root by patching the new Image.*-dtb as Magisk would.

## // Properties / Variables ##
```
kernel.string=KernelName by YourName @ xda-developers
do.devicecheck=1
do.modules=1
do.systemless=1
do.cleanup=1
do.cleanuponabort=0
device.name1=maguro
device.name2=toro
device.name3=toroplus
device.name4=tuna
supported.versions=6.0 - 7.1.2
supported.patchlevels=2019-07 -

block=/dev/block/platform/omap/omap_hsmmc.0/by-name/boot;
is_slot_device=0;
ramdisk_compression=auto;
```

__do.devicecheck=1__ specified requires at least device.name1 to be present. This should match ro.product.device, ro.build.product, ro.product.vendor.device or ro.vendor.product.device from the build.prop files for your device. There is support for as many device.name# properties as needed. You may remove any empty ones that aren't being used.

__do.modules=1__ will push the .ko contents of the modules directory to the same location relative to root (/) and apply correct permissions. On A/B devices this can only be done to the active slot.

__do.systemless=1__ (with __do.modules=1__) will instead push the full contents of the modules directory to create a simple "ak3-helper" Magisk module, allowing developers to effectively replace system files, including .ko files. If the current kernel is changed then the kernel helper module automatically removes itself to prevent conflicts.

__do.cleanup=0__ will keep the zip from removing its working directory in /tmp/anykernel (by default) - this can be useful if trying to debug in adb shell whether the patches worked correctly.

__do.cleanuponabort=0__ will keep the zip from removing its working directory in /tmp/anykernel (by default) in case of installation abort.

__supported.versions=__ will match against ro.build.version.release from the current ROM's build.prop. It can be set to a list or range. As a list of one or more entries, e.g. `7.1.2` or `8.1.0, 9` it will look for exact matches, as a range, e.g. `7.1.2 - 9` it will check to make sure the current version falls within those limits. Whitespace optional, and supplied version values should be in the same number format they are in the build.prop value for that Android version.

__supported.patchlevels=__ will match against ro.build.version.security_patch from the current ROM's build.prop. It can be set as a closed or open-ended range of dates in the format YYYY-MM, whitespace optional, e.g. `2019-04 - 2019-06`, `2019-04 -` or `- 2019-06` where the last two examples show setting a minimum and maximum, respectively.

`block=auto` instead of a direct block filepath enables detection of the device boot partition for use with broad, device non-specific zips. Also accepts specifically `boot` or `recovery`.

`is_slot_device=1` enables detection of the suffix for the active boot partition on slot-based devices and will add this to the end of the supplied `block=` path. Also accepts `auto` for use with broad, device non-specific zips.

`ramdisk_compression=auto` allows automatically repacking the ramdisk with the format detected during unpack. Changing `auto` to `gz`, `lzo`, `lzma`, `xz`, `bz2`, `lz4`, or `lz4-l` (for lz4 legacy) instead forces the repack as that format, and using `cpio` or `none` will (attempt to) force the repack as uncompressed.

`customdd="<arguments>"` may be added to allow specifying additional dd parameters for devices that need to hack their kernel directly into a large partition like mmcblk0, or force use of dd for flashing.

`slot_select=active|inactive` may be added to allow specifying the target slot. If omitted the default remains `active`.


## // Staying Up-To-Date ##

Now that you've got a ready zip for your device, you might be wondering how to keep it up-to-date with the latest AnyKernel commits. AnyKernel2 and AnyKernel3 have been painstakingly developed to allow you to just drop in the latest update-binary and tools directory and have everything "just work" for beginners not overly git or script savvy, but the best practice way is as follows:

1. Fork my AnyKernel3 repo on GitHub

2. `git clone https://github.com/<yourname>/AnyKernel3`

3. `git remote add upstream https://github.com/osm0sis/AnyKernel3`

4. `git checkout -b <devicename>`

5. Set it up like your <devicename> zip (i.e. remove any folders you don't use like ramdisk or patch, delete README.md, and add your anykernel.sh and optionally your Image.*-dtb if you want it up there) then commit all those changes

6. `git push --set-upstream origin <devicename>`

7. `git checkout master` then repeat steps 4-6 for any other devices you support

Then you should be able to `git pull upstream master` from your master branch and either merge or cherry-pick the new AK3 commits into your device branches as needed.

___For further support and usage examples please see the AnyKernel3 XDA thread:___ _https://forum.xda-developers.com/showthread.php?t=2670512_

__Have fun!__
