## Arch Linux Installation

To install Arch Linux I used the Arch Linux Wiki’s installation guide (https://wiki.archlinux.org/title/Installation_guide) and this youtube guide (https://www.youtube.com/watch?v=RJt4i89Ofsw) to assist me through the process.


### VMware Fusion Set Up
I first started with downloading the arch linux torrent file and used bittorrent to convert the torrent file into a .iso file. For this installation of Arch Linux, I used VMWare Fusion to download Arch Linux on a virtual machine. When first setting up, I selected the version “Other Linux 5.x and later kernel 64 bit”, specified UEFI for the boot firmware, set ram to 2GB, and set the hard drive space to 20GB.

Once this was set, I loaded it into Arch. I first checked to see if there was an internet connection and set the time to mt timezone
```markdown
ping Google.com
timedatectl set-timezone America/Chicago
```

###Disk Formating
The next step was to partition the disk. I decided to make two partitions, one consisting of 500Mb and the other containing the rest. 
```markdown
fdisk /dev/sda
G
N
1
Default
+500M 
N
2
Default
Default
W

Mkfs.fat -F32 /dev/sda1
Mkfs.ext4 /dev/sda2
Mount /dev/sda2 /mnt 
Mkdir /mnt/boot
Mkdir /mnt/boot/EFI
Mount /dev/sda1 /mnt/boot/EFI
```

```markdown 
Syntax highlighted code block
```


