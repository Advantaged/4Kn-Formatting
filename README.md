# 4Kn-Formatting
Convert &amp; Format drives with 4096 PBS/LBS (phisical/logical-block-size

### 1. Scope of instructions
1. Run a 'nvme' drive with '4096' Byte sectors instead of '512' or '512B'.
2. The throughput of a drive is given by 4KiB since more than one decade, & this not only by 'nvme' but also  by 'HDD' & 'SSD'.
3. These instructions are for Linux users, whether they are also valid for Apple, BSD or Windows operating systems... please find it in the related forums. 

### 2. Basic requirements
1. A 'nvme' drive supporting 4Kn directly or at least allow to switch between different (at least tween 512B & 4096) sector-sizes.
2. Manufacturer supporting switching are 'Corsair' since at least model 'MP5xx' and 'WD'. The latter has not been checked, please contact me to make a complete list.
3. The increased throughput is assured by "PCI-e', this mean 'nvme' in 'M.2' design.
4. A live installation-usb with a Linux-Distro (e.g. [Artix-Linux-Plasma](https://download.artixlinux.org/iso/artix-plasma-openrc-20220713-x86_64.iso)) or a computer that already has a Linux on it.
5. Possibility to install additional programs like `nvme-cli`, `sgdisk`, `kaprtx`/`multipath-tools`   

### 3. Suitable manufacturers and models.
1. Corsair serie 'MP 5xx' & 'Force 600' (Phison controller)
2. Western-Digital (WD) to be confirmed
3. Western-Digital (WD) HDD 'Red NAS' or 'Pro' (through `hdpharm`) see "Convert HDD" chapter below.


### 4. Let's go!
1. In case you want erase the whole 'nvme', use the command below. READ THE NOTES FIRST !!! 

- **Note 1**: This erase all the data including 'hidden bootsector', 'RAID-configuration-data' & older 'OS-starting-data'. Use it only if you know what you are doing. In case your 'nvme' have a lower 'PBS/LBS' you can at end note the elapsed time and compare it after the 'switching'.

- **Note** 2: This step is necessary if your drive was used as 'BSD' (Free-BSD, True-NAS) data carrier or hosted a such operating-system (OS), if it was used in a RAID-assembly or hosted an OS starting with old BIOS and not UEFI

- **Note 3**: Countercheck that you choose the right drive with `lsblk` command or equivalent.
```
dd if=/dev/zero of=/dev/nvme0n1 bs=4096 status=progress
```
2. Install missing programs if needed:
```
sudo pacman -S --needed nvme-cli gptfdisk multipath-tools
```
3. Check LBA-status/mode:
```
nvme id-ns /dev/nvme0n1 -H | grep LBA
```
- output:
```
LBA Format  0 : Metadata Size: 0   bytes - Data Size: 512 bytes - Relative Performance: 0x2 Good (in use)
LBA Format  1 : Metadata Size: 0   bytes - Data Size: 4096 bytes - Relative Performance: 0x1 Better
```
4. Format or switch the drive:
```
nvme format /dev/nvme0n1 -l 1
```
- output:
```
Success formatting namespace:1
````
**Note**: This tell the 'nvme'-controller to use the storage cells in '4KiB'-blocks. Of course, due change/switch of blocks-dimension are all data lost, hence is called 'formatting'.
5. Recheck LBA-status/mode:
```
nvme id-ns /dev/nvme1n1 -H | grep LBA
```
output:
```
LBA Format  0 : Metadata Size: 0   bytes - Data Size: 512 bytes - Relative Performance: 0x2 Good 
LBA Format  1 : Metadata Size: 0   bytes - Data Size: 4096 bytes - Relative Performance: 0x1 Better (in use)
```
- or
```
cat /sys/block/nvme0n1/queue/physical_block_size

cat /sys/block/nvme0n1/queue/logical_block_size
```
- recheck in the list with:
```
nvme --list
```
- output:
```
Node                     Model                    Format  
/dev/nvme0n1     Force MP600                      4 KiB +  0 B
/dev/nvme1n1     Samsung SSD 970 PRO 512GB        512   B +  0 B
```
6. Detect all drive-parameter:
```
fdisk -l /dev/nvme0n1
```
**Note**: With this command you get all drive-parameter like 'sectors', 'bytes' etc.. 
```
Disk /dev/nvme0n1: 1,82 TiB, 2000398934016 bytes, 488378646 sectors
Disk model: Force MP600                             
Units: sectors of 1 * 4096 = 4096 bytes
Sector size (logical/physical): 4096 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
```
7. Recheck the speed!?
**Note**: The Linux-Kernel is (most probably) not yet aware of your made changes, reboot in this case before issuing the command!
```
dd if=/dev/zero of=/dev/nvme0n1 bs=4096 status=progress
```
### 5. Partitioning & Formatting








