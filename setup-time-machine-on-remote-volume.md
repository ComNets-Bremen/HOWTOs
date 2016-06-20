Time Machine Backups on a Remote Disk Image (Connected over AFP)
================================================================

This is a HOWTO that describes how a disk image is setup on a remote server to make Time Machine backups on OS X. In this case, the remote server is a Linux file server.

Keywords
---------
Time Machine, Backups, OS X, Linux, AFP, Sparse Bundle, ExFAT 

Procedure
---------

1. Setup AFP (my preference was not Samba) to connect to the Linux server. Install `nettalk` package.
   - `apt-get install netatalk`
   - Installation will result in the AFP service being setup to start automatically (and is also started automatically after the installation)

2. From the OS X host, connect to the Linux server.
   - Open a `Finder` window by invoking `Connect to Server...`
   - Give the connection parameters - e.g., `[[afp://10.10.155.89]]` where 10.10.155.89 is the Linux server address
   - Give the login credentials on the Linux server - authentication process is linked to the Linux authentication system automatically, over PAM, (I think ;-) so nothing extra has to be done

3. Create a disk image on the Linux server (on the volume mounted in Step 2)
   - Run Disk Utility
   - Invoke `New Image`
   - Select a location in the Linux server as the location to create the disk 
   - Rest of the values,
     - Name: `bkup`, Size: 500GB (or what ever), Format: Mac OS Extended (Case-sensitive, Journaled)
     - Encryption: 128-bit (I usually don't use), Partitions: Single partition - Apple Partition Map
     - Image Format: sparse bundle disk image
   - Hit create
   - Creation resulted in it also being mounted (e.g., `/Volumes/bkup`)
   - If it is not mounted, double click on the sparse bundle to mount it

4. Make Time Machine use this new disk image (i.e., `/Volumes/bkup`) as its backup disk. 
   - Open a Terminal
   - Run the command `sudo tmutil setdestination /Volumes/bkup`

5. Check Time Machine Preferences
   - Run System Preferences
   - Select Time Machine
   - Select `Select Disk` and see if the disk (`bkup`) is selected

6. Thats it !!! Now hit "Back Up Now" in Time Machine
   - Usually, the first backup takes painfully long (if large), depending on your network connection
   - See Notes for a method to minimize the initial backup time

Notes
-----

1. Alternative way to make the first backup
   - Since the initial backup is long, an alternative way is to make the first backup on a disk image created on a portable disk attached to the OS X host (e.g., a USB disk) and then transfer it to the Linux file server (over USB)
   - Since FAT cannot handle large file (i.e., over 4 GB), format the USB disk with an appropriate disk format (e.g., ExFAT)  - If ExFAT used (as I did) and since Linux does not have native ExFAT support, a PPA based exFAT implementation was used
     - `add-apt-repository ppa:relan/exfat`
     - `apt-get update`
     - `apt-get install fuse-exfat`
     - `apt-get install exfat-utils`
     - Connect USB disk to Linux server, and check dmesg and fdisk to get disk id
     - `mount -t exfat /dev/sda1 /mnt` (for example)  


If you have any questions or comments, please write to,

Asanga Udugama (adu@comnets.uni-bremen.de)
