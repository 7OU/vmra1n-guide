# Installing QEMU and enabling KVM
## Installing QEMU
With Ubuntu 19.10, its as easy as opening a terminal and running:<br/> `sudo apt
install qemu qemu-system qemu-utils python3 python3-pip`
## Enabling KVM
Open a text editor as root and open the file `/etc/default/grub`. In the
quotation marks of `GRUB_CMDLINE_LINUX_DEFAULT`, you'll want to add:<br/>
For Intel CPU's
```
iommu=pt intel_iommu=on
```
For AMD CPU's
```
iommu=pt amd_iommu=on
```
Save the file, and then run `sudo update-grub`.
