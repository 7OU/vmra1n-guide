# Re-Binding the USD Controller
Open the terminal and clone the vmra1n repository (`git clone
https://github.com/foxlet/vmra1n`) Then run the lsiommu.sh file. You will get an
output like this:
```
IOMMU Group 14 03:00.0 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD]
300 Series Chipset USB 3.1 xHCI Controller [1022:43bb] (rev 02)
IOMMU Group 14 03:00.1 SATA controller [0106]: Advanced Micro Devices, Inc.
[AMD] 300 Series Chipset SATA Controller [1022:43b7] (rev 02)
IOMMU Group 18 27:00.3 USB controller [0c03]: Advanced Micro Devices, Inc. [AMD]
Zeppelin USB 3.0 Host controller [1022:145f]
```
Now edit rebind.sh and modify it to fit the USB controller information from the
lsiommu.sh to make it look like the following
```
BIND_PID1="1022 145f"
BIND_BDF1="0000:27:00.3"
```
Run `sudo ./rebind.sh` and if it does not disable the USB 3.0 ports you have,
reboot, try another USB controller, and try again. Keep in mind the USB
controller you rebinded isnt supposed to work until reboot.
