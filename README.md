# 4Kn-Formatting
Convert &amp; Format drives with 4096 PBS/LBS (phisical/logical-block-size

### 1. Scope of instructions
1. Run a 'nvme' drive with '4096' Byte sectors instead of '512' or '512B'.
2. The throughput of a drive is given by 4Kn since more than one decade, & this not only by 'nvme' but also  by 'HDD' & 'SSD'.
3. These instructions are for Linux users, whether they are also valid for Apple, BSD or Windows operating systems... please find it in the related forums. 

### 2. Basic requirements
1. A 'nvme' drive supporting 4Kn directly or at least allow to switch between different (at least tween 512B & 4096) sector-sizes.
2. Manufacturer supporting switching are 'Corsair' since at least model 'MP5xx' and 'WD'. The latter has not been checked, please contact me to make a complete list.
3. The increased throughput is assured by "PCI-e', this mean 'nvme' in 'M.2' design.
4. A live installation-usb with a Linux-Distro or a computer that already has a Linux on it.
5. Possibility to install additional programs like `nvme-cli`, `sgdisk`, `kaprtx`/`multipath-tools`   

### 3. Suitable manufacturers and models.
1. Corsair serie 'MP 5xx' & 'Force 600' (Phison controller)
2. Western-Digital (WD) to be confirmed


### 4. Let's go!
1. In case you want erase the whole 'nvme', use following command. 
**Note 1**: This erase all the data including 'bootsector'. Use it only if you know what you are doing. In case your 'nvme' have a lower 'PBS/LBS' you can at end note the elapsed time and compare it after the 'switching'.
**Note 2**: Countercheck that you choose the right drive with `lsblk`
```
dd if=/dev/zero of=/dev/nvme0n1 bs=4096 status=progress
```
2. Install missing programs if needed:
```
sudo pacman -S --needed nvme-cli gptfdisk multipath-tools
```
3. Check if it's possible to switch the LBS
```
nvme id-ns /dev/nvme0n1 -H | grep LBA
```
output:
```
LBA Format  0 : Metadata Size: 0   bytes - Data Size: 512 bytes - Relative Performance: 0x2 Good (in use)
LBA Format  1 : Metadata Size: 0   bytes - Data Size: 4096 bytes - Relative Performance: 0x1 Better
```





