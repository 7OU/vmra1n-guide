# Creating the VM
Open a Terminal and clone the macOS-Simple-KVM repository (`git clone https://github.com/foxlet/macOS-Simple-KVM`)
Now create a disk using `qemu-img create -f qcow2 macOS.qcow2 32G`. Open
basic.sh in a text editor, and add the following:
```
    -drive id=SystemDisk,if=none,file=macOS.qcow2 \
    -device ide-hd,bus=sata.4,drive=SystemDisk \
```
Save, and now run `sudo ./basic.sh` and install macOS. Once macOS is installed,
power off the VM, and open basic.sh in a text editor. Add the following to the
end of the file (replace XX:XX.X with the info from the USB controller):
```
    -device pcie-root-port,bus=pcie.0,multifunction=on,port=1,chassis=1,id=port.1 \
    -device vfio-pci,host=XX:XX.X,bus=port.1 \
```
Save, and now run `sudo ./basic.sh` Boot into macOS, install and run checkra1n.
And enjoy your new jailbroken iDevice!
