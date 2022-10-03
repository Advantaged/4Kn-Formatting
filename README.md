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
6. Assure you can akt as 'Admin/Root' with/over `sudo`, `sudo -s` or `su -s`   

### 3. Suitable manufacturers and models (white-list only).
#### NVMe's M.2 (PCIe)
1. Corsair serie 'MP 5xx' & 'Force 600' (Phison controller)
2. Western-Digital (WD) to be confirmed

#### HDD's
1. Western-Digital (WD) HDD 'Red NAS' or 'Pro' (through `hdparm`) see "Convert HDD" chapter below.


### 4. Let's go!
1. In case you want erase the whole 'nvme', use the command below. READ THE NOTES FIRST !!! 

- **Note 1**: This erase all the data including 'hidden bootsector', 'RAID-configuration-data' & older 'OS-starting-data'. Use it only if you know what you are doing. In case your 'nvme' have a lower 'PBS/LBS' you can at end note the elapsed time and compare it after the 'switching'.

- **Note** 2: This step is necessary if your drive was used as 'BSD' (Free-BSD, True-NAS) data carrier or hosted a such operating-system (OS), if it was used in a RAID-assembly & or ZFS, hosted an OS starting with old BIOS and not with UEFI.

- **Note 3**: Countercheck that you choose the right drive with `lsblk` command or equivalent.
```
dd if=/dev/zero of=/dev/nvme0n1 bs=4096 status=progress
```
2. Install missing programs if needed:
```
pacman -S --needed nvme-cli gptfdisk multipath-tools
```
3. Check LBA-status/mode and possibility to 'format namespace':
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
- **Note**: This tell the 'nvme'-controller to use the storage cells in '4KiB'-blocks. Of course, due change/switch of blocks-dimension are all data lost, hence is called 'formatting'.
5. Recheck LBA-status/mode and possibility to 'format namespace':
```
nvme id-ns /dev/nvme1n1 -H | grep LBA
```
- output:
```
LBA Format  0 : Metadata Size: 0   bytes - Data Size: 512 bytes - Relative Performance: 0x2 Good 
LBA Format  1 : Metadata Size: 0   bytes - Data Size: 4096 bytes - Relative Performance: 0x1 Better (in use)
```
- or commands
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
- **Note**: With this command you get all drive-parameter like 'sectors', 'bytes' etc.. 
```
Disk /dev/nvme0n1: 1,82 TiB, 2000398934016 bytes, 488378646 sectors
Disk model: Force MP600                             
Units: sectors of 1 * 4096 = 4096 bytes
Sector size (logical/physical): 4096 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
```
7. Recheck the speed!?
- **Note**: The Linux-Kernel is (most probably) not yet aware of your made changes, reboot in this case before issuing the command!
```
dd if=/dev/zero of=/dev/nvme0n1 bs=4096 status=progress
```
### 5. Partitioning & Formatting
- **Note 1**: Here are three different things to setup; 
  - a) 1. Partition-table type (GPT or MBR/DOS). 
  - b) 2. Partitioning of drive (subdivision). 
  - c) 3. Formatting the partition with a file-system (fs) e.g. 'ext4', 'btrfs', 'FAT-32' etc..
- **Note 2**: I prefere to do it (for many reasons) manually; 
  - a) better control of 'start & end' (letting empty space at end of drive). 
  - b) exact dimensions (in KiB, MiB, GiB, etc.). 
  - c) commit/assign special parameters assuring/force 4KiB block-size (that other CLI- or Visual-Tools don't do). 
  - d) plan exactly in a text-file or script avoiding errors.
#### 5.1. Partition-table
- **Note**: Check anyway you select the right drive with `lsblk` and/or `fdisk -l /dev/nvme0n1` or `sgdisk -p /dev/nvme0n1`
1. Erase all partition-table information present on the drive:
```
sgdisk -Z /dev/nvme0n1
```
2. Make a new GPT partition-table
```
sgdisk -o /dev/nvme0n1
```
3. In case you cannot reach the drive issue this command and reboot:
```
blkdiscard /dev/nvme0n1
```
#### 5.2. Partitioning
- **Note 1**: Assure you use full KiB, MiB, GiB and not KB, MB, GB. Use besides an uniformed measure, on my side I prefer GiB. Calculate in advance the partitions sizes, e.g. from command `fdisk -l /dev/nvme0n1` I know my drive contain '2000398934016 bytes' divided by '1024x1024x1024' (this is equal to 1 GiB) have I a total of 1863 GiB.
1. EFI-partition
```
sgdisk -n 1:1M:+1G -t 1:ef00 -c 1:EFI-0003 /dev/nvme0n1
```
2. OS-partition
```
sgdisk -n 2:0:+1860G -t 2:8300 -c 2:ARTIX-0003 /dev/nvme0n1
```
3. SWAP-partition, in case you want one... reduce the size of OS-partition accordingly.
- **Note 2**: I set the SWAP-size to RAM-size multiplied 1.5, for 32 GB RAM is SWAP equal 48 GiB.
```
sgdisk -n 3:0:+8G -t 3:8200 -c 3:SWAP-0003 /dev/nvme0n1
```
- **Note 3**: EFI & SWAP don't need necessarily a label, hence omit in case `-c 1:EFI-0003` & `-c 3:SWAP-0003`.
- **Note 4**: The first writable sector on '512' or '512B' drives is `2048`, the first writable sector on 4Kn is `256` equal 2048/8.
4. Once you’ve created partition successfully, you need to update the partition table changes to kernel for that let us run the partprobe command to add the disk information to kernel and after that list the partition as shown below.
```
partprobe -s
```
- Other information and man-page of 'sgdisk`
  - [SGDISK](https://www.rodsbooks.com/gdisk/sgdisk-walkthrough.html)
  - [SGDISK Man-Page](https://man.archlinux.org/man/sgdisk.8.en)

#### 5.3. Formatting
- **Note 1**: In case you get error messages by formatting... again, the Kernel not yet recognized your changes. Use command-s:
   `kpartx -u /dev/nvme0n1` or `partprobe -s` or reboot.
1. Formatting EFI-part in 4Kn/4KiB
```
mkfs.vfat -F32 -s 2 -S 4096 -v /dev/nvme0n1p1
```
2. Formatting OS-part in 4Kn/4KiB
- **Note 2**: In case you want to format in `btrfs`, jump to point **5.4.2**
```
mkfs.ext4 -F -b 4096 -F /dev/nvme0n1p2
```
3. Formatting SWAP-part in 4Kn/4KiB
```
mkswap -f -p 4096 /dev/nvme0n1p3
```
4. These are the results (without Swap) of my 'nvme':
```
fdisk -l /dev/nvme0n1
Disk /dev/nvme0n1: 1,82 TiB, 2000398934016 bytes, 488378646 sectors
Disk model: Force MP600                             
Units: sectors of 1 * 4096 = 4096 bytes
Sector size (logical/physical): 4096 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: AC509C30-1A7B-4E47-BBEA-52D88EF8176B

Device          Start       End   Sectors  Size Type
/dev/nvme0n1p1    256    262399    262144    1G EFI System
/dev/nvme0n1p2 262400 487850239 487587840  1,8T Linux filesystem
```
5. These are the results (with swap = RAM x 1.5 = 96 GiB) of my 'nvme':
```
disk -l /dev/nvme0n1
Disk /dev/nvme0n1: 1,82 TiB, 2000398934016 bytes, 488378646 sectors
Disk model: Force MP600                             
Units: sectors of 1 * 4096 = 4096 bytes
Sector size (logical/physical): 4096 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 4C8B88A0-1775-478B-8B90-857B4A705215

Device             Start       End   Sectors  Size Type
/dev/nvme0n1p1       256    262399    262144    1G EFI System
/dev/nvme0n1p2    262400 461635839 461373440  1,7T Linux filesystem
/dev/nvme0n1p3 461635840 486801663  25165824   96G Linux swap
```
6. Explanation-Table of partitioning & formatting

| Partition N° | Dimension | Type | Description |
| --- | --- | ---- | ---- |
| 1 | 1 GiB | EFI (ef00) | large enough for 'rEFInd' and many other 'OS' |
| 2 | 1760 GiB | Linux (8300) | 32 GiB x 55 |
| 3 | 96 GiB | Swap (8200) | 32 GiB x 3 |
| empty | 6 GiB | none | overprovisioning: 3 Gib by no swap 6 Gib with swap |

- **Note** for formatting:
  - a) 1. EFI: Use exact above parameter
  - b) 2. Ext4: We have to force twice this operation '-F' in order to take effect.
  - c) 3. Swap: Normally the 'page-size' with the option `-f -p 4096` [is not necessary](https://www.computerhope.com/unix/mkswap.htm)
#### 5.4. Additional steps in case of need
1. Labelling partitions even after formatting:
```
sgdisk -c 1:EFI-0003 /dev/nvme0n1
sgdisk -c 2:ARTIX-0003 /dev/nvme0n1
sgdisk -c 3:SWAP-0003 /dev/nvme0n1
```
- **Note**: EFI & SWAP normally don't need a label at all.
2. Format OS-part to [btrfs](https://wiki.archlinux.org/title/btrfs)
- Install in case `btrfs-progs`. In most of the today’s latest Linux distributions, btrfs package comes as pre-installed. If not, install btrfs package using the following command. 
```
pacman -S btrfs-progs
```
- Enable `btrfs` in the Kernel. After btrfs package has been installed on the system, now we need to enable the Kernel module for btrfs using below command.
```
modprobe btrfs
```
- Format finally the OS-part. partitioned as `8300` Linux-fs as `btrfs`
```
mkfs.btrfs /dev/nvme0n1p2
```
- Other information on ['btfrs'](https://www.tecmint.com/create-btrfs-filesystem-in-linux/)
3. Mount options `btrfs` of OS-part on 'nvme' in `/etc/fstab`
```
UUID=<uuid-of-dev-nvme0n1p2>  / noatime,space_cache=v2,compress=zstd,ssd,discard=async,subvol=@ 0 0
UUID=<uuid-of-dev-nvme0n1p2>  /home noatime,space_cache=v2,compress=zstd,ssd,discard=async,subvol=@home 0 0
```
4. Backup & restore with `btrfs`, for this install `timeshift` & `timeshift-autosnap`. 
```
5 aur/timeshift-bin 22.06.1-1 [+11 ~1.91]
    A system restore utility for Linux
4 aur/timeshift-autosnap 0.9-1 [+39 ~2.50]
    Timeshift auto-snapshot script which runs before package upgrade using Pacman hook.
3 aur/timeshift 22.06.5-1 [+308 ~15.72]
    A system restore utility for Linux
```
### 6. Convert/switch an HDD to 4Kn
- Following these threads: [Check/Switch LBA](https://unix.stackexchange.com/questions/562571/switching-hdd-sector-size-to-4096-bytes),
[Arch-Linux](https://wiki.archlinux.org/index.php/Advanced_Format), [WD](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/collateral/white-paper/white-paper-advanced-format.pdf), [HGST](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/collateral/tech-brief/tech-brief-advanced-format.pdf).
- **Note-s**:
  - a) This conversion can cause unexpected results. Use it only if you know what you are doing.
  - b) This action destroy/delete all data inside the HDD, hence backup your data before beginning.
  - c) It works only if the `physical_block_size` (PBS, setled by manufacturer) is already 4Kn, it changes only the LBS.
1. Install programm:
```
pacman -S hdparm
```
2. Individuate drive with `lsblk` command.
3. Detect "Logical  Sector size" and "Physical Sector size"
```
cat /sys/block/sdd/queue/physical_block_size

cat /sys/block/sdd/queue/logical_block_size
```
- or with command
```
hdparm -I /dev/sdd
```
- relevant output for me
```
Logical  Sector size: 512 bytes
Physical Sector size: 4096 bytes
```
- For an existing filesystem the block size can be determined with:
```
tune2fs -l /dev/sdd
```
4. Change/switch LBA (logical block size)
```
hdparm --verbose --set-sector-size 4096 --please-destroy-my-drive /dev/sdd
```
- in case something going wrong, try
```
hdparm --dco-restore /dev/sdd
```
5. Check result repeating point **6.3.**

- [x] **Done & Enjoy !**
