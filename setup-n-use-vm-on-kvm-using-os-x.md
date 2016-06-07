Setup and Use of a KVM based VM using OS X
===========================================

This is a HOWTO that describes the procedure of setting up and using a KVM based VM hosted on a remote Linux Host from an OS X Host over VNC. The following diagram shows the different components referred to in the procedure below and how they are connected.

    +---------------+                    +---------------+
    |   +-------+   |                    |     +-------+ |
    |   | Guest |<---------------------------->|Chicken| |
    |   |  VM   |   |                    |     +-------+ |
    |   +-------+   |                    |     +-----+   |
    |               |<------------------------>| ssh |   |
    |               |                    |     +-----+   |
    +---------------+                    +---------------+
       Linux Host                            OS X Host 
    (with KVM deployed)


Keywords
---------
HOWTO, KVM, OS X (Mac), Linux, , Ubuntu, VNC, Chicken, Virtual Machine (VM), SSH


Procedure
---------

1. From the OS X Host, make an SSH login to Linux Host on which the VMs will be run
2. Download the .iso of the Linux distribution to install
   - e.g., `wget http://releases.ubuntu.com/14.04/ubuntu-16.04-desktop-amd64.iso`
3. Create the VM disk image (using KVM command)
   - e.g., `qemu-img create name_of_disk_image.img 10G`
   - Parameter `10G` is the disk space
4. Bring up the VM with the Linux .iso attached to install Linux (using KVM command)
   - `sudo qemu-system-x86_64 -hda name_of_disk_image.img -cdrom ubuntu-16.04-desktop-amd64.iso -boot d -m 2048 -machine accel=kvm -device e1000,netdev=net0,mac=DE:AD:BE:EF:A9:83 -netdev tap,id=net0 -display vnc=:20`
   - Adjust the parameters according to your preferences
   - `vnc=:20` refers to the port over which VNC connects (the actual port will be 5920)
5. On the OS X Host, install `Chicken` (VNC client)
   - Download `Chicken` from SourceForge 
   - `https://sourceforge.net/projects/cotvnc/`
6. Create a profile in `Chicken` and set it up with the following settings
   - IP address of Linux Host
   - Port number (e.g., 20 from above example)
7. Connect to just created VM using `Chicken`
   - Install Linux on the VM
   - Once installed, kill the VM (there is no way of gracefully shutting down, me thinks!!!)
8. Bring up the VM again without the .iso (using KVM command)
   - `sudo qemu-system-x86_64 -hda name_of_disk_image.img -m 2048 -machine accel=kvm -device e1000,netdev=net0,mac=DE:AD:BE:EF:A9:83 -netdev tap,id=net0 -display vnc=:20`
9. Connect to VM using `Chicken`


Notes
-----

1. Use `screen` on the Linux Host to run the above commands (e.g., `sudo qemu-system-x86_64`) 
   - this is to keep the VM running even after you logout from SSH
2. OS X has a built-in VNC client (through `Finder`), but it seems that this VNC client has a protocol version issue that prevents it from connecting with KVM

If you have any questions or comments, please write to,

Asanga Udugama (adu@comnets.uni-bremen.de)
