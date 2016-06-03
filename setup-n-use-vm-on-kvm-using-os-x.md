Setup and Use of a KVM based VM using OS X
===========================================

This is a HOWTO that describes the procedure of setting up and using a KVM based VM hosted on a remote Linux host from an OS X based host over VNC.


Procedure
---------

1. From the OS X, SSH login to host machine on which the VMs will be run (Linux host)
2. Download the .iso of the Linux distribution to install
   - e.g., `wget http://releases.ubuntu.com/14.04/ubuntu-16.04-desktop-amd64.iso`
3. Create the VM disk image
   - `qemu-img create name_of_disk_image.img 10G`
   - 10G is the disk space
4. Bring up the VM with the linux .iso attached to install Linux
   - `sudo qemu-system-x86_64 -hda name_of_disk_image.img -cdrom ubuntu-16.04-desktop-amd64.iso -boot d -m 2048 -machine accel=kvm -device e1000,netdev=net0,mac=DE:AD:BE:EF:A9:83 -netdev tap,id=net0 -display vnc=:20`
5. On the OS X, install `Chicken` (VNC client)
   - Download from SourceForge 
   - `https://sourceforge.net/projects/cotvnc/`
6. Setup `Chicken` with the following settings
   - host machine IP address
   - port = 20
7. Connect to VM using `Chicken`
   - install linux on the VM
   - Once installed, kill the VM 
   - as there is no way of gracefully shutting down
8. Bring up the VM again without the .iso
   - `sudo qemu-system-x86_64 -hda name_of_disk_image.img -m 2048 -machine accel=kvm -device e1000,netdev=net0,mac=DE:AD:BE:EF:A9:83 -netdev tap,id=net0 -display vnc=:20`
9. Connect to VM using `Chicken`


Notes
-----

1. Use `screen` on the Linux host to run the above commands (e.g., `sudo qemu-system-x86_64`) 
   - this is to keep the VM running even after you logout from SSH
2. OS X has a built-in VNC client (through `Finder`), but it seems that it has a protocol version issue that prevents it from connecting with KVM

If you have any questions or comments, please write to,

Asanga Udugama (adu@comnets.uni-bremen.de)
