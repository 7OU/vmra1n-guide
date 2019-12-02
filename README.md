# Step by step guide on how to install vmra1n
**PLEASE NOTE**: I did not make vmra1n, and have only made a guide to help others use it. Also, if you fuck up your computer in any way, I am not responsible for it (I'm too poor to get sued ;-;).
## Requirements
 - Motherboard with Vt-x and IOMMU support
 - An **install** (**NOT VM**) of a Linux distro (This guide focuses on Ubuntu-based distros)
 - A computer that isn't total shit (Your 2nd gen Celeron won't cut it :/)
 - Some form of Linux experience (This saves the patience of people trying to help if something goes wrong)
 - Hard drive / Parition with a fair amount over 40GB
## Before We Start
### Building Qemu 3
#### Getting the dependencies
Qemu 3 does not come on the Ubuntu repositories, so we will have to build it. To start off, you will have to open a text editor (as root/sudo) and edit the file `/etc/apt/sources.list`. Here you want to go to the very bottom, and uncomment (remove the hashtag) in the lines that say `deb-src`. Once that is done, you will want to run ``sudo apt update && sudo apt build-dep qemu`` and say yes to the apt confirmation.  
  
Now we will want to install git, you can do this by running `sudo apt install git`.
#### Building Qemu
Go to your home folder by running `cd ~`. Now run `git clone https://github.com/qemu/qemu.git`. Once that has cloned you want to go to the directory (`cd qemu`) and move to the Qemu 3 branch, you can do this with `git checkout v3.0.0`.  
Now, we will update the submodules with ``git submodule init && git submodule update --recursive``.  
We will need to configure the build, we can customize the build to make it faster to compile, but for the most part, this is not neccesary. To configure the build, run `./configure`. Once this has finished, we can compile Qemu with `make -j5`. This will take a while, so don't freak out if this takes over 10-20 minutes. The speed of this depends on your processor.  
Once the build has finished, we will install it with ``sudo make install``. Now Qemu 3.0 is installed!
### Modifying the boot parameters for IOMMU
This step is very simple. As root/ with sudo, modify `/etc/default/grub` and if you are using an Intel CPU, you want to add the following inside the quotation marks in `GRUB_CMDLINE_LINUX_DEFAULT`: 
```
iommu=pt intel_iommu=on
```
If you are using an AMD CPU, add the following inside the quotation marks in `GRUB_CMDLINE_LINUX_DEFAULT`:
```
iommu=pt amd_iommu=on
```
Now that that is done, save, and run the following: `sudo update-grub`, if it did not finish sucessfully, double check your file. Now you will want to reboot.
## Doing the vmra1n stuff.
First, move to the home directory again with `cd ~`, and then clone the vmra1n repo with `git clone https://github.com/foxlet/vmra1n`. Now go into the directory (`cd vmra1n`), and run `./lsiommu.sh`, the output should be something like this:
```
IOMMU Group 14 03:00.0 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] 300 Series Chipset USB 3.1 xHCI Controller [1022:43bb] (rev 02)
IOMMU Group 14 03:00.1 SATA controller [0106]: Advanced Micro Devices, Inc. [AMD] 300 Series Chipset SATA Controller [1022:43b7] (rev 02)
IOMMU Group 18 27:00.3 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] Zeppelin USB 3.0 Host controller [1022:145f]
```
Now find a USB controller, and highlight the line. You will want to write down what you got for these parts (they are in bold):
IOMMU Group 14 **03:00.0** USB controller [0c03]: Advanced Micro Devices, Inc. [AMD] 300 Series Chipset USB 3.1 xHCI Controller [**1022:43bb**] (rev 02)
Now open a text editor for `rebind.sh` and modify the top of the file so it'll look like this, but with your details (you can figure it out, I belive in you.)
```
BIND_PID1="1022 145f"
BIND_BDF1="0000:27:00.3"
```
## Making the VM
Go back to the home directory with `cd ~`, and clone the KVM installer with `git clone https://github.com/foxlet/macOS-Simple-KVM`. Go into the directory with `cd macOS-Simple-KVM`. Install the dependencies for the KVM with `sudo apt-get install qemu-system qemu-utils python3 python3-pip`. And once thats done, download the install image with `./jumpstart --mojave` (I found catalina doesn't work, you can try it if you **really** desire.). Now make a disk with `qemu-img create -f qcow2 macOS.qcow2 32G`. Now add the following to the bottom of `basic.sh`:
```
    -drive id=SystemDisk,if=none,file=macOS.qcow2 \
    -device ide-hd,bus=sata.4,drive=SystemDisk \
```
Save, and run `sudo ./basic.sh`. And now Install macOS, You will have to erase the 32GB drive in disk utility before you install. Once installed, you can go to the desktop, and then shutdown the VM. Now add this to the end of `basic.sh`, and remember to replace XX:XX.X with the numbers you got earlier:\
```
    -device pcie-root-port,bus=pcie.0,multifunction=on,port=1,chassis=1,id=port.1 \
    -device vfio-pci,host=XX:XX.X,bus=port.1 \
```
Now save, and run `sudo ./basic.sh`. Boot into macOS and then run checkra1n! Enjoy your new jailbroken device!

## Troubleshooting
If you have any issues, visit the r/Jailbreak discord (discord.gg/jb) and somebody will be there to help you out (most of the time). Or you can post on the r/Jailbreak subreddit.
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
Guide by Evan, https://cumbox.best
