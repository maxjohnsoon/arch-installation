## Arch Linux Installation
### Max Johnson

To install Arch Linux I used the Arch Linux Wiki’s installation guide (https://wiki.archlinux.org/title/Installation_guide) and this youtube guide (https://www.youtube.com/watch?v=RJt4i89Ofsw) to assist me through the process.


### VMware Fusion Set Up
I first started with downloading the arch linux torrent file and used bittorrent to convert the torrent file into a .iso file. For this installation of Arch Linux, I used VMWare Fusion to download Arch Linux on a virtual machine. When first setting up, I selected the version “Other Linux 5.x and later kernel 64 bit”, specified UEFI for the boot firmware, set ram to 2GB, and set the hard drive space to 20GB.

Once this was set, I loaded it into Arch. I first checked to see if there was an internet connection and set the time to my timezone
```
ping Google.com
timedatectl set-timezone America/Chicago
```

### Disk Formating and Mounting
The next step was to partition the disk. I decided to make two partitions, one consisting of 500Mb and the other containing the rest. 
```
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

mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda2
mount /dev/sda2 /mnt 
mkdir /mnt/boot
mkdir /mnt/boot/EFI
mount /dev/sda1 /mnt/boot/EFI
```
### Installation and Configuration of the System
```
pacstrap /mnt base linux linux-firmware nano
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
fallocate -l 1GB /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
nano /etc/fstab 
```
I wrote here "/swapfile none swap defaults 0 0"
I then set up the clock
```
ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime 
hwclock —systohc 
```
### Localization
```
nano /etc/locale.gen
```
Here I uncommented the line #en_US.UTF-8 UTF-8
```
locale-gen
nano /etc/locale.conf
```
Here I wrote "LANG=en_US.UTF-8"
```
nano /etc/hostname 
```
Here I wrote "archmax" for my hostname
```
nano /etc/hosts 

//Here i wrote
127.0.0.1	localhost
::1		localhost
127.0.1.1	archmax.localdomain	archmax
```
### Grub
```
pacman -s grub efibootmgr networkmanager network-manager-applet wireless_tools wpa_supplicant os-prober motors dosftools base-level linux-headers
grub-install —target=x86_64-efi —efi-directory=/boot/EFI —bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```
### Reboot of System
```
exit
umount -a
```
Since I am in a VM I did not type the command reboot but rather restarted the VM and then logged back into root

### Arch Specifications
Enabled NetworkManager so my Arch could have access to networks.
```
systemctl start NetworkManager
systemctl enable NetworkManager
```
Added 3 users with each of them being a part of the wheel. Also specified that user codi and sal should have their passwords reset after logging back in.
```
useradd -m -G wheel max
passwd max
Spam746

useradd -m -G wheel sal
passwd sal
GraceHopper1906
passwd -e sal

useradd -m -G wheel codi
passwd codi
GraceHopper1906
passwd -e codi
```
I also had to uncomment the line #%wheel ALL=(ALL) ALL here so the above users would have access to sudo.
```
EDITOR=nano visudo
```
Here I downloaded and installed the packages needed for the KDE desktop enviorment to be set up
```
pacman -S xorg
pacman -S dddm
systemctl enable sddm
pacman -Syu telegram-desktop 
pacman -S plasma kde-applications
```
### Other Downloads
Firefox
```
sudo pacman -Sy --noconfirm firefox firefox-i18n-en-us
```
SSH
```
sudo pacman -S openssh
```
zsh
```
sudo pacman -S zsh zsh-completions
```
### Alias Setup
```
alias ll='ls -la'
alias cd..='cd ..'
alias ..='cd ..'
alias ...='cd ../../../'
alias ....='cd ../../../../'
```
### Problems that Occured
The first problem that I ran into was not specifying the correct version of linux rather than the correct version, “Other Linux 5.x and later kernel 64 bit”, and booting the system in BIOS rather than UEFI. Another problem I ran into was that some packages were failing to download, in my case plasma and kde-applications for the DE, and was solved by updating. I also missed steps such as installing and setting up grub which had me restart my process a couple of times.
