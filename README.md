# 4Kn-Formatting
Convert & Format drives with 4096 PBS/LBS (phisical/logical-block-size), LBS is also called LBA.

## A comprehensive minimalistic compendium for newbies & proofs, about proper Data-Carrier-Handling of UEFI-GPT 'Disks' (DC) for using them not only with Linux but also with BSD, MAC & WIN.

- **General Note**: Some manufacturer use '4096' as 'physical_block_size' (PBS) but '512e' or '512' as 'logical_block_size' LBS/LBA, ([see here the differences](https://en.wikipedia.org/wiki/Advanced_Format#Overview) as well the [Arch-Linux Wiki](https://wiki.archlinux.org/title/Advanced_Format#Setting_native_sector_size)). Well, some manufacturer allow to modify/change the LBS (for-all by 'NVMe') and some don't. Some other use '4096n / 4Kn' (it's mean native) both as PBS and LBS (for-all by very big HDD's). The PBS is NOT changeable❗️

### 1. Scope of instructions
1. Run a 'nvme' drive with '4096' Byte sectors instead of '512' or '512e'.
2. The throughput of a drive is given by Manufacturer in 4KiB since more than two decade, & this not only by 'nvme' but also  by 'HDD' & 'SSD'.
3. Still, [using 4KN formatting](https://carlosfelic.io/misc/how-to-switch-your-nvme-ssd-to-4kn-advanced-format/) for newer devices ensure they are operating in their native space, without any need of conversion to happen at the controller-side resulting in major speed & less warming.
4. These instructions are for Linux users, whether they are also valid for Apple, BSD or Windows operating systems… please find it in the related forums.
5. Preferably buy 4Kn-DC (Data-Carrier) also called Enterprise-DC like HGST (HDD) or modifiable with simple Linux-Commands (`nvme` ,  `sg_format` , `hdparm`) convertible DC. 

### 2. Basic requirements
1. A 'nvme' drive supporting 4Kn directly or at least allow to switch between different (at least tween 512 & 4096) logical-sector-sizes (LBS/LBA).
2. Manufacturer supporting switching are 'Corsair' (since at least model 'MP5xx' = PCI-e 3, let say a long tradition). Please contact me to make a complete list. Please commit all manufacturer, series and model you was able to convert, thanks.
3. The increased throughput is assured by "PCI-e', this mean 'nvme' in 'M.2' design.
4. A live installation-usb with a Linux-Distro (e.g. Arcolinux-D, CachyOS…) or a computer that already has a Linux installed on it.
5. Possibility to install additional programs like `nvme-cli`, `gptfdisk`, `multipath-tools`
6. Assure you can act as 'Admin/Root' with/over `sudo` , `sudo -s` , `sudo -i` or `su -s`   

### 3. Suitable manufacturers and models (white-list only).

* Please contributing to enhanced/fulfill this Whitelist & hence help other people to make the right choice of Data-Carrier (DC).

#### 3.1. NVMe's M.2 (PCIe)
1. Corsair® serie 'MP 5xx' & ['MP Force 600'](https://forum.corsair.com/forums/topic/165279-mp600-2tb-cssd-f2000gbmp600/) ('Phison' controller) using `nvme` command.
2. Western-Digital® (WD) serie ['WD_BLACK SNxxx'](https://carlosfelic.io/misc/how-to-switch-your-nvme-ssd-to-4kn-advanced-format/) ('Phison' & 'Western Digital Black' controllers) over/trough `nvme` command.

#### 3.2. SSD (SATA)
1. For industrial standard SSDs you can use `sg_format` from `sg3_utils` see "Convert SSD" chapter **6.** below.
2. Some manufacturer offer similar Win®-apps too. Those apps are mainly for "Consumer-DC".

#### 3.3. HDD's (SATA)
1. For industrial standard HDDs you can use `hdparm` command, see "Convert HDD" chapter **7.** below.
2. HGST appartain to Western-Digital (WD) an offer already 4Kn HDDs.
3. Some manufacturer offer similar Win®-apps too. Those apps are mainlyy for "Consumer-DC".

#### 3.4. Suitable Operatings-System-s (OS)
1. Linux-es/Unix-oides OOTB (out of the box):
   * CachyOS: Archlinux-based, rolling-release, also using perfectly `btrfs` & `zfs` file-systems, for Desktop & Gaming.
   * Proxmox: Debian-Based, point-release, also using `btrfs` & `zfs` file-systems, for Server, hosting 'OPNsense', 'pfSense' (Firewall, Networks-Management), & other OSs & Server-applications.
   * True-NAS-Scale: Debian-based, point-release, also use `zfs` file-system, as the name already say… this's a open source & free of charge NAS-OS.
   * Some other Linux-es with Live-ISO-GUI &|| CLI, need in addition to be partitioned & formatted in advance. Forall the EFI (`vfat`) and ROOT if using `ext4` or `btrfs` .
   * BSD-based-OSs: all point-release, using `udf` & `zfs` file-systems, used on SOC, Desktops & Servers. 
2. Windows®: Yes, also Win (10/11) 'understand' (meanwhile) 4K & 4Kn OOTB. For Servers & Desktops, there you havent the 'opportunity' to make partition & formatting by youself. Conversion of DC to 4Kn over manufacturer-win-apps or each Linux-Live-ISOs like… CachyOS, ArcolinuxD, Artix-Linux-Plasma, Bluestar-Linux, but also Kubuntu or each other Linux-ISO with GUI-Installer.
3. MAC-OS should also be able to 'understand' 4K or 4Kn… earlier than Win, but i have no knowledges about this OS.


### 4. Let's go!
1. In case you want erase the whole 'nvme', use the command below. READ THE NOTES FIRST❗️ 

* **Note-s:**
   * This erase all the data including 'hidden bootsector', 'RAID-configuration-data' & older 'OS-starting-data'. Use it only if you know what you are doing. In case your 'nvme' have a lower 'LBS/LBA' you can at end note the elapsed time and compare it after the 'switching'.
   * This step is necessary if your drive was used as 'BSD' (Free-BSD, True-NAS) data carrier or hosted a such operating-system (OS), if it was used in a RAID-assembly & or ZFS, hosted an OS starting with old BIOS and not with UEFI.
   * Countercheck that you choose the right drive with `lsblk` command or equivalent.
* Speed-Test & deletion of all data from a data carrier.  If the disk is not empty & you still need the data... copy, clone, save this data to another disk BEFORE executing this 'operation'.
```
dd if=/dev/zero of=/dev/nvme0n1 bs=4096 status=progress
```
2. Install missing programs if needed:
```
pacman -S --needed --noconfirm nvme-cli gptfdisk multipath-tools
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
nvme id-ns /dev/nvme0n1 -H | grep LBA
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
- This is the output of 4x nvme on a [nvme-HBA](https://www.aliexpress.com/item/1005004819139295.html?), with two such HBA's you can build and install a 'True-NAS-Scale' (Debian-based on ZFS) with 8xnvme's, but, this's almost out of topic.
```
Node                  Model                                    Format         

/dev/nvme0n1          Force MP600                              4 KiB +  0 B   
/dev/nvme1n1          Force MP600                              4 KiB +  0 B   
/dev/nvme2n1          Corsair MP600 PRO                        4 KiB +  0 B   
/dev/nvme3n1          Corsair MP600 PRO                        4 KiB +  0 B   

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
- 2000398934016 bytes = 1863 GiB
7. Recheck the speed⁉️
- **Note**: The Linux-Kernel is (most probably) not yet aware of your made changes, use command-s <br/>
   `kpartx -u /dev/nvme0n1` or `partprobe -s` or `reboot` before issuing the command!
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
  - e) choose `cgdisk /dev/nvme0n1` or `cfdisk /dev/nvme0n1` or `gdisk /dev/nvme0n1` to use a terminal-graphical-version of the three best GPT-partitioning-programs.
#### 5.1. Partition-table
- **Note**: Check anyway you select the right drive with `lsblk` and/or `blkid` or `fdisk -l /dev/nvme0n1` or `sgdisk -p /dev/nvme0n1` .

1. Erase all partition-table information (Linux-RAID, BTRFS, ZFS or other simple partitions) present on the drive:
```
sgdisk -Z /dev/nvme0n1
```
2. Make a new GPT partition-table
```
sgdisk -o /dev/nvme0n1
```
3. In case you cannot reach the drive issue this command and reboot:
```
blkdiscard -f /dev/nvme0n1
```
#### 5.2. Partitioning
* **Note 1**: Assure you use full KiB (2^10), MiB (2^20), GiB (2^30) and not KB, MB, GB or TB. Use besides an uniformed measure, on my side I prefer GiB. Calculate in advance the partitions sizes, e.g. from command `fdisk -l /dev/nvme0n1` . I know my drive contain '2000398934016 bytes' divided by '1024x1024x1024' (this is equal to 1 GiB = 2^30), have I a total of 1863 GiB. 

1. Common Partitions-Types / Overview 
* **Note 2**: Many Linux-Distros use today (already) 2 GiB for the EFI-Partition, the maximum is 4 GiB & this do i too. That permit you to 'store' (really) many 'image' of your os that permit you to rollback (in case) your OS at any point. For cloning a partition or for replace DC (Data-Carrier) in a RAID or Partition an Intel-Optane for [ZIL & LOG in True-NAS](https://www.truenas.com/docs/references/zilandslog/) is convenient to assign to the ROOT-Partition (or others partitions) a certain or fix amount of Storage-Quantity/-Place. See also [partitions-codes](https://github.com/Advantaged/4Kn-Formatting/blob/main/partition-codes.md)
* EFI:
```
  sgdisk -n 1:1M:+4G -t 1:ef00 -c 1:EFI-0003 /dev/nvme0n1
```
* EXT4 [ArchLinux-Wiki / UEFI-GPT](https://wiki.archlinux.org/title/partitioning):
```
   sgdisk -n 2:0:+1850G -t 2:8304 -c 2:ARCO-D /dev/nvme0n1
```
* BTRFS (following [Stackexchange](https://unix.stackexchange.com/questions/617918/what-type-of-partition-guid-should-i-use-for-linux-with-btrfs), [Github](https://github.com/util-linux/util-linux/issues/1175) & [Arch-Wiki](https://wiki.archlinux.org/title/GPT_fdisk):
```
  sgdisk -n 2:0:+1850G -t 2:8304 -c 2:ARCO-D /dev/nvme0n1
```
* ZFS:
```
   sgdisk -n 2:0:+1850G -t 2:bf00 -c 2:CACHYOS-Z /dev/nvme0n1
```
* SWAP:
```
   sgdisk -n 3:0:+8G -t 3:8200 -c 3:SWAP-0003 /dev/nvme0n1
```

* **Note-s 3**
   * SWAP-partition, in case you want one... reduce the size of OS-partition accordingly.
   * SWAP-recommendation: Don't use SWAP-Partition or -file, use ['ZRAM❗️'](https://github.com/Advantaged/ZRAM) instead. You can install & configure it after first reboot & upgrade.
   * SWAP-Size: I set the SWAP-size to RAM-size multiplied 1.5, for 32 GB RAM is SWAP equal 48 GiB.
   * Partition-Label: `sgdisk -c 1:EFI-0003` set the partition-label only!
   * Labels for EFI & SWAP: EFI & SWAP don't need necessarily a label, hence omit in case `-c 1:EFI-0003` & `-c 3:SWAP-0003`.
   * First used Sector: The first writable sector on '512' DC is `2048`, the first writable sector on 4Kn is `256` equal 2048/8.
   
* Once you’ve created partition successfully, you need to update the partition-table-changes to kernel, for that let us run the `partprobe -s` command to add the disk information to kernel and after that, list the new partition.


* Other information and man-page of 'sgdisk`
   * [SGDISK](https://www.rodsbooks.com/gdisk/sgdisk-walkthrough.html)
   * [SGDISK Man-Page](https://man.archlinux.org/man/sgdisk.8.en)
   * SGDISK is for scripting, you can use [CGDISK](https://man.archlinux.org/man/cgdisk.8.en) or [CFDISK](https://linux.die.net/man/8/cfdisk) with a CLI-GUI instead.
   * CGDISK usage: `cgdisk /dev/nvme0n1`

#### 5.3. Formatting
- **Note 1**: In case you get error messages by formatting... again, the Kernel not yet recognized your changes. Use command-s:<br/>
   `kpartx -u /dev/nvme0n1` or `partprobe -s` or reboot.
1. Formatting [EFI-partition](https://man7.org/linux/man-pages/man8/mkfs.vfat.8.html) in/with 4Kn/4KiB
```
mkfs.vfat -F32 -s 2 -S 4096 -n EFI -v /dev/nvme0n1p1
```
-  This Volume-Label don't appear in your "File-Manager", but e.g., permit "rEFInd" to detect & use the right logo for your OS (Opearatings-System-s).
2. Formatting [EXT4-ROOT](https://linux.die.net/man/8/mkfs.ext4) in/with 4Kn/4KiB
```
mkfs.ext4 -F -b 4096 -L ARTIX-0003 -F /dev/nvme0n1p2
```
3. Formatting [SWAP-part](https://man7.org/linux/man-pages/man8/mkswap.8.html) in 4Kn/4KiB.
   * It's recommended to use [ZRAM](https://github.com/Advantaged/ZRAM) instead of SWAP. SWAP in contrary to "EXT4", use by default 4K & don't need any label, but, just for completness…:
```
mkswap -f -p 4096 -L SWAP-0003 /dev/nvme0n1p3
```
4. Formatting [BTRFS-ROOT](https://man7.org/linux/man-pages/man8/mkfs.btrfs.8.html).
```
   mkfs.btrfs -s 4096 -f -L ARCOL-D /dev/nvme0n1p2
```

* **Note:**
   * Here are other options (`-O`) , RAID-possibility & Subvolume to be created, more informations under [Archlinux-Wiki](https://wiki.archlinux.org/title/btrfs).
   * Some Linux-OS, like CachyOS, can hadle perfectly BTRFS in/with 4K
5. "Formatting" ZFS-ROOT.
* Basic Information:
   * Here we cannot talk anymore about formatting, because ZFS is completely different from other file systems & is much more than a file system alone.
   * For desktops i recommend warmly [CachyOS](https://cachyos.org/) handling perfectly 4Kn, ZFS, BTRFS & all other file systems without any manual intervention.
   * In ZFS you (or your OS) build a `zpool` containing one (stripe-mode) or more partitions of the same size on different DC (all kind of RAIDs ➕ Z-RAIDs) &|| data-carriers (DC). The name of `zpool` is arbitrary (as in other so-called file systems too).
   * By creating a `zpool` you will automatically assign a name like `zpool1` , `tank1` or others at your pleasure.
   * Underneath the `zpool` will be created the `zvol`-s also known as 'data-sets', that represent the folders & sub-folders. Those names are NOT arbitrary if the OS is "sitting" just on them.
   * More information in the [Archlinux-Wiki](https://wiki.archlinux.org/title/ZFS) & [OpenZFS](https://openzfs.github.io/openzfs-docs/Getting%20Started/index.html) & in chapter **5.6.**. 
* Create a `zpool` manually:
```
  zpool create firstzp /dev/nvme0n1p2
```

6. These are the results (without Swap) of my 'nvme' with Arcolinux-D:
```
fdisk -l /dev/nvme0n1
Disk /dev/nvme0n1: 1.82 TiB, 2000398934016 bytes, 488378646 sectors
Disk model: Force MP600                             
Units: sectors of 1 * 4096 = 4096 bytes
Sector size (logical/physical): 4096 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: <my hidden disk-identifier>

Device           Start       End   Sectors  Size Type
/dev/nvme0n1p1     256   1048831   1048576    4G EFI System
/dev/nvme0n1p2 1048832 486015231 484966400  1.8T Linux filesystem
```
7. These are the results (with swap = RAM x 1.5 = 96 GiB) of my 'nvme' with Artix-Linux-Plasma:
```
fdisk -l /dev/nvme0n1
Disk /dev/nvme0n1: 1,82 TiB, 2000398934016 bytes, 488378646 sectors
Disk model: Force MP600                             
Units: sectors of 1 * 4096 = 4096 bytes
Sector size (logical/physical): 4096 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: <my hidden disk-identifier>

Device             Start       End   Sectors  Size Type
/dev/nvme0n1p1       256    262399    262144    1G EFI System
/dev/nvme0n1p2    262400 461635839 461373440  1,7T Linux filesystem
/dev/nvme0n1p3 461635840 486801663  25165824   96G Linux swap
```
7. Explanation-Table of partitioning & formatting

| Partition N° | Dimension | Type | Description |
| --- | --- | ---- | ---- |
| 1 | 4 GiB | EFI (ef00) | large enough for 'rEFInd' and many other 'OS' |
| 2 | 1760 GiB | Linux (8304) | 32 GiB x 55 |
| 3 | 96 GiB | Swap (8200) | 32 GiB x 3 |
| empty | 6 GiB | none | overprovisioning: 3 Gib by no swap 6 Gib with swap |

- **Notes** for formatting:
  - a) 1. EFI: Use exact above parameter, unless you use CachyOS.
  - b) 2. Ext4: We have to force twice this operation '-F' in order to take effect.
  - c) 3. Swap: Normally the 'page-size' with the option `-f -p 4096` [is not necessary](https://www.computerhope.com/unix/mkswap.htm) as well the volume-labelling.

#### 5.4. Additional steps in case of need
1. Labelling Partitions even after partitioning:
```
sgdisk -c 1:EFI-0003 /dev/nvme0n1
sgdisk -c 2:ARTIX-0003 /dev/nvme0n1
sgdisk -c 3:SWAP-0003 /dev/nvme0n1
```
- **Note**: EFI & SWAP normally don't need a partition-label at all, but the ROOT-Partition-label is very important, e.g. for command: `blkid` .

2. Labelling Volume even after formatting:
* [VFAT](https://unix.stackexchange.com/questions/59751/set-a-vfat-volume-name-non-destructively):
```
   dosfslabel /dev/nvme0n1p1 EFI-NEW
```

* [EXT4](https://www.cyberciti.biz/faq/linux-modify-partition-labels-command-to-change-diskname/#:~:text=You%20need%20to%20use%20the,partition%20label%20for%20security%20reasons.):
```
   e2label /dev/nvme0n1p2 ARCOL-D
```

* [SWAP-Label](https://man7.org/linux/man-pages/man8/swaplabel.8.html):
```
   swaplabel -L LAZY-SW /dev/nvme0n1p3
```

* [BTRFS](https://man7.org/linux/man-pages/man8/mkfs.btrfs.8.html):
   * [By unmounted filesystem](https://askubuntu.com/questions/236681/filesystem-label-rename) `btrfs filesystem label <device> <newlabel>` :

```
   btrfs filesystem ARCOL-D /dev/nvme0n1p2 ARCOL-L
```

* [ZFS](https://docs.oracle.com/cd/E19253-01/819-5461/gaypf/) `zfs rename [actual name] [new name]`:
```
   zfs rename zwol1 tank1
```
  
#### 5.5 Format BTRFS  
1. Format OS-part to [btrfs](https://wiki.archlinux.org/title/btrfs)
- Install in case `btrfs-progs`. In most of the today’s latest Linux distributions, btrfs package comes as pre-installed. If not, install btrfs package using the following command. 
```
pacman -S --needed --noconfirm btrfs-progs
```
- Enable `btrfs` in the Kernel. After btrfs package has been installed on the system, now we need to enable the Kernel module for btrfs using below command.
```
modprobe btrfs
```
- Format finally the OS-part, partitioned as `8304` Linux-fs as `btrfs`
```
mkfs.btrfs -s 4096 -f -L ARCOL-D /dev/nvme0n1p2
```
- Other information on ['btfrs'](https://www.tecmint.com/create-btrfs-filesystem-in-linux/)
4. Mount options `btrfs` of OS-part on 'nvme' in `/etc/fstab`
```
UUID=<uuid-of-dev-nvme0n1p2>  / noatime,space_cache=v2,compress=zstd,ssd,discard=async,subvol=@ 0 0
UUID=<uuid-of-dev-nvme0n1p2>  /home noatime,space_cache=v2,compress=zstd,ssd,discard=async,subvol=@home 0 0
```
5. Backup & restore with `btrfs`, for this install `timeshift` & `timeshift-autosnap`. 
```
5 aur/timeshift-bin 22.06.1-1 [+11 ~1.91]
    A system restore utility for Linux
4 aur/timeshift-autosnap 0.9-1 [+39 ~2.50]
    Timeshift auto-snapshot script which runs before package upgrade using Pacman hook.
3 aur/timeshift 22.06.5-1 [+308 ~15.72]
    A system restore utility for Linux
```
#### 5.6. "Format" [ZFS](https://docs.oracle.com/cd/E19253-01/819-5461/gaynr/index.html)
* **Very inmportasnt note-s:**
   * The command [`zpool create`](https://openzfs.github.io/openzfs-docs/man/master/8/zfs-create.8.html) make both "pools" and "volumes" also called "Datasets" underneath the pool-s.
   * Both `zpool` & `zvol`-s have many [options](https://www.funtoo.org/ZFS_as_Root_Filesystem) that must be set/configurated. That less a job for newbie.  
* Install in case `zfs-dkms` , `zfs-utils` &|| other packages like `zrepl zfs-auto-snapshot` etc.. In some of the today’s latest Linux distributions, like CachyOS, are all ZFS-components already installed. If not, install at least these ZFS packages using the following command. 
```
paru -S --needed --noconfirm zfs-dkms zfs-utils
```
- Enable `zfs` in the Kernel. After zfs package has been installed on the system, now we need to enable the Kernel module for zfs using below command.
```
modprobe zfs
```
- "Format" finally (create a `zpool`) on the OS-part, partitioned as `bf00` 'Solaris Root', creating 'zfs-spool' called 'firstzp' with only one partition (stripe).
```
zpool create firstzp /dev/nvme0n1p2
```
- now we will create the most complicated "constellation" (RAID 10) out of my 4xNVME-Es each 2 TB
```
zpool create firstzp mirror /dev/nvme0n1p2 /dev/nvme1n1p2 mirror /dev/nvme2n1p2 /dev/nvme3n1p2
```
   * here the two mirrors are autoimatically striped with a resulting RAID-10 = two striped mirrors.
### 6. Convert/switch an SSD to 4Kn
- Following this [Server-Forum](https://forums.servethehome.com/index.php?threads/how-to-reformat-hdd-ssd-to-512b-sector-size.4968/page-20) & this Video [Art of Server](https://www.youtube.com/watch?v=DAaTfv96V9w) we need an additional package to (`sg3_utils`) do this.

1. Install program:

```
pacman -S --needed --noconfirm sg3_utils hdparm
```
2. Show details of a SSD, e.g. `sdb` with `fdisk -l` (see above) or fresh installed `sg_` or `hdparm`:

```
sg_readcap -l /dev/sdb
```
or
```
hdparm -I /dev/sdb | grep 'Sector size:'
```

3. Format logicaal block size to 4K=4096:

```
sg_format --format --size=4096 /dev/sdb
```
* **Note:**
   * It takes some minutes to format the SSD in this way.
   * Some Manufacturer offer Win-program to manage LBS of SSD, see chapter **3.**.

### 7. Convert/switch an HDD to 4Kn
- Following these threads: [Check/Switch LBA](https://unix.stackexchange.com/questions/562571/switching-hdd-sector-size-to-4096-bytes),
[Arch-Linux](https://wiki.archlinux.org/index.php/Advanced_Format), [WD](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/collateral/white-paper/white-paper-advanced-format.pdf), [HGST](https://documents.westerndigital.com/content/dam/doc-library/en_us/assets/public/western-digital/collateral/tech-brief/tech-brief-advanced-format.pdf).
- **Note-s**:
  - a) This conversion can cause unexpected results. Use it only if you know what you are doing.
  - b) This action destroy/delete all data inside the HDD, hence backup your data before beginning.
  - c) It works only if the `physical_block_size` (PBS, setled by manufacturer) is already 4Kn, it changes only the LBS.
1. Install program:
```
pacman -S --needed --noconfirm hdparm sg3_utils
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

- ✅ **Done & Enjoy**❗️ :wink:
