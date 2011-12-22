Introduction
============

This archlinux package derives from the current stock Archlinux kernel (as of the time of this
writing, 2011, the 2nd of november).

It is based on the blog articles that you can find [here](http://www.ioncannon.net/system-administration/1290/how-to-build-compile-a-custom-linux-kernel-for-ec2/)
and [here](http://worldmodscode.wordpress.com/2011/10/28/ec2-ami-creation-without-magic/).

Basically, the only change is the config file.

The changes in the config file are the following:

* Enable PAE and high memory support up to 64 GB
* Enable paravirtualized client and Xen paravirtualized client
* Enable XEN front-end block device and network device
* Enable XEN virtual console
* Enable NFS client support as built-in in the kernel to allow setting the ip address at boot

When built, the kernel can be started in a PV-GRUB instance with the following `/boot/grub/menu.lst`:

    default 0
    timeout 1
     
    title Arch Linux
      root (hd0,0)
      kernel /boot/vmlinuz-linux-ec2 root=/dev/xvda1 ip=dhcp console=hvc0 spinlock=tickless ro
      initrd /boot/initramfs-linux-ec2.img


Currently, support is only present for i686 instances.


* "Processor type and features" -> "High Memory Support" -> Make sure it is set to 64GB
* "Processor type and features" -> "PAE (Physical Address Extension) Support" -> enable
* "Processor type and features" -> "Paravirtualized guest support" -> enable
* "Processor type and features" -> "Paravirtualized guest support" -> "Xen guest support" -> enable
* "Device Drivers" -> "Block devices" -> "Xen virtual block device support" -> enable either as a module or built in
* "Device Drivers" -> "Network device support" -> "Xen network device frontend driver" -> enable either as a module or built in
* "Networking support" -> "Networking options" -> "IP: Kernel level autoconfiguration" -> enable and all sub options
