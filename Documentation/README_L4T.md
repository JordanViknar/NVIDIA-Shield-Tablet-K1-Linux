## List of changes compared to most guides

| Change | Description |
| --- | --- |
| MultiROM + TWRP | Updated versions. |
| Kernel | Updated versions, contains optimizations. |
| Bootloader | Thanks to the updated versions of the tools above, we can use the latest bootloader and stock image of the tablet. |

## Prerequisites

### Tablet

- K1 model, (original model not tested)
- Latest stock firmware
- Unlocked bootloader
- **(Optional)** Data partitions wiped to free up space.

### Computer

- SSH client.
- ADB and Fastboot installed.

## Instructions

### Flashing MultiROM and TWRP

1. Boot your device into **fastboot mode**.
2. Flash the MultiROM TWRP recovery image from this repository.
```bash
fastboot flash recovery TWRP_3.1.0-2_multirom_shieldtablet_20170415-01.img
```
3. Boot into recovery mode.
4. Using TWRP, install the `reflash.zip` kernel, MultiROM and MultiROM's touch support from this repository.

### Linux4Tegra Installation

*If you made it here, congratulations. The dangerous part is over. Consider this a checkpoint, you only need to rewind back to this part if you mess up later.*

1. Install [Linux4Tegra v23.1](https://xdaforums.com/t/linux4tegra-r23-1-r24-1-beta-for-the-shield-tablet.2985238/) using your TWRP installation's MultiROM settings.
2. A few files need to be replaced/added in Linux4Tegra's root filesystem *(at `/data/media/0/multirom/roms/Linux4TegraR231`)* with the ones from this repository, using TWRP's file manager.  :
   - `boot/zImage`
   - `root/etc/fstab`
   - `root/etc/pm/sleep.d/50tegra`
3. Boot your tablet into Linux4Tegra using MultiROM, by restarting into the system and selectioning the option. If 4 Tux penguins appear, congratulations, you made it to the non-Android Linux world.

### Ubuntu 14.04 Setup

#### Keyboard issues

The lack of a physical keyboard might get you stuck on the setup screen when specifying your login informations. You'll find the onscreen keyboard is unusable too. Do not panic, it's temporary.

In the meantime, go back to the region selection screen, and copy the text, then paste it in the user setup screen's fields until you can get past it (which is how I ended up with *"ParisParis"* as password). Enable automatic login to avoid potential issues, though it *could* be unnecessary.

**Remember the password you set at ALL costs, you'll need it later.**

Once the interface is loaded, you can finally launch *Onboard* to obtain a virtual keyboard. Don't forget to go into settings and grow the interface size while you're at it, to make touchscreen navigation easier.

> [!WARNING]
> 
> **My touchcreen is flipped after I modified the display settings.**
> 
> Restart your system. Any way seemingly works, just restart it.

#### Updating Ubuntu 14.04

> [!WARNING]
>
> **Reminder :** At this point, connect to Wi-Fi and check that SSH is functional. You **will** need it if you cannot use the touchscreen or virtual keyboard at any point.

```
sudo add-apt-repository universe && sudo apt update && sudo apt upgrade -y
```

*(Remember to use Onboard if doing this from the tablet itself.)*

And now, as expected, your touchscreen should be inverted after a reboot.

We fix that through SSH *(or before rebooting)* :

```bash
sudo apt install nano -y
sudo nano /etc/X11/xorg.conf.d/10-evdev.conf
```

Make the last section look like this and save, then restart the system :

```
Section "InputClass"
	Identifier "evdev touchscreen catchall"
	MatchIsTouchscreen "on"
	MatchDevicePath "/dev/input/event*"
	Driver "evdev"
	Option "InvertY" "true"
	Option "InvertX" "true"
	Option "Calibration" "0 1199 0 1919"
EndSection
```

### Updating to Ubuntu 16.04

> [!WARNING]
>
> This is where I get stuck for now.
>
> The touchscreen fix mentioned in other tutorials simply does NOT work for me. My tablet also did not connect to my Wi-Fi automatically, making SSH unusable.

### TODO

> [!IMPORTANT]
>
> However, I have one idea up my sleeve.
>
> What if I scrapped the Ubuntu Linux4Tegra approach entirely, and used an Arch Linux ARM filesystem with SSH and Wi-Fi already set up, then moved/downloaded/updated the Linux4Tegra files over ?
>
> Also, perhaps Wayland could avoid me some touchscreen issues... or do the exact opposite and add more. It's a coin flip for now as far as I know, really.

## Sources

### Files

- Kernel/[reflash.zip](https://github.com/mittorn/android_kernel_nvidia_shieldtablet/releases/tag/first_multirom_release)
- MultiROM/[multirom_shieldtablet_touch_support_m.zip](https://prd.androidfilehost.com/?fid=745425885120719014)
- MultiROM/[multirom-20170621-v33-UNOFFICIAL-shieldtablet.zip](https://androidfilehost.com/?fid=529152257862714116)
- [Tegra4Linux Modifications](https://github.com/mittorn/android_kernel_nvidia_shieldtablet/releases/tag/first_ubuntu_release)
- TWRP/[TWRP_3.1.0-2_multirom_shieldtablet_20170415-01.img](https://androidfilehost.com/?fid=457095661767148485)

### Information

- https://xdaforums.com/t/linux4tegra-r23-1-r24-1-beta-for-the-shield-tablet.2985238
- https://github.com/mittorn/android_kernel_nvidia_shieldtablet/releases
- https://www.piyushaggarwal.in/2020/05/06/plasma-tablet/

### Interesting notes (not necessarily used here)

*On updating rootfs (to newer Ubuntu or other distribution)* :

- https://xdaforums.com/t/linux4tegra-r23-1-r24-1-beta-for-the-shield-tablet.2985238/post-67637938
- https://xdaforums.com/t/linux4tegra-r23-1-r24-1-beta-for-the-shield-tablet.2985238/post-76422376
- https://xdaforums.com/t/linux4tegra-r23-1-r24-1-beta-for-the-shield-tablet.2985238/post-80542629
- https://xdaforums.com/t/linux4tegra-r23-1-r24-1-beta-for-the-shield-tablet.2985238/post-85500149

*On reaching later Ubuntu versions* :

- https://xdaforums.com/t/linux4tegra-r23-1-r24-1-beta-for-the-shield-tablet.2985238/post-83374019
- https://xdaforums.com/t/linux4tegra-r23-1-r24-1-beta-for-the-shield-tablet.2985238/post-84942097 *(latest tutorial I could find)*

*Brightness* :

- https://xdaforums.com/t/linux4tegra-r23-1-r24-1-beta-for-the-shield-tablet.2985238/post-83422989