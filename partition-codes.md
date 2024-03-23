## partition-codes.md

* Use command as root `#` (`su` or `sudo`) to view in Terminal:

`    sgdisk --list-types`



```
0700 Microsoft basic data                0701 Microsoft Storage Replica
0702 ArcaOS Type 1                       0c01 Microsoft reserved
2700 Windows RE                          3000 ONIE boot
3001 ONIE config                         3900 Plan 9
4100 PowerPC PReP boot                   4200 Windows LDM data
4201 Windows LDM metadata                4202 Windows Storage Spaces
7501 IBM GPFS                            7f00 ChromeOS kernel
7f01 ChromeOS root                       7f02 ChromeOS reserved
7f03 ChromeOS firmware                   7f04 ChromeOS mini-OS
7f05 ChromeOS hibernate                  8200 Linux swap
8300 Linux filesystem                    8301 Linux reserved
8302 Linux /home                         8303 Linux x86 root (/)
8304 Linux x86-64 root (/)               8305 Linux ARM64 root (/)
8306 Linux /srv                          8307 Linux ARM32 root (/)
8308 Linux dm-crypt                      8309 Linux LUKS
830a Linux IA-64 root (/)                830b Linux x86 root verity
830c Linux x86-64 root verity            830d Linux ARM32 root verity
830e Linux ARM64 root verity             830f Linux IA-64 root verity
8310 Linux /var                          8311 Linux /var/tmp
8312 Linux user's home                   8313 Linux x86 /usr
8314 Linux x86-64 /usr                   8315 Linux ARM32 /usr
8316 Linux ARM64 /usr                    8317 Linux IA-64 /usr
8318 Linux x86 /usr verity               8319 Linux x86-64 /usr verity
831a Linux ARM32 /usr verity             831b Linux ARM64 /usr verity
831c Linux IA-64 /usr verity             831d Linux Alpha root (/)
831e Linux ARC root (/)                  831f Linux LoongArch root (/)
8320 Linux MIPS-32 BE root (/)           8321 Linux MIPS-64 BE root (/)
8322 Linux MIPS-32 LE root (/)           8323 Linux MIPS-64 LE root (/)
8324 Linux PA-RISC root (/)              8325 Linux PowerPC-32 root (/)
8326 Linux PowerPC-64 BE root (/)        8327 Linux PowerPC-64 LE root (/)
8328 Linux RISC-V-32 root (/)            8329 Linux RISC-V-64 root (/)
832a Linux s390 root (/)                 832b Linux s390x root (/)
832c Linux TILE-Gx root (/)              832d Linux Alpha /usr
832e Linux ARC /usr                      832f Linux LoongArch /usr
8330 Linux MIPS-32 BE /usr               8331 Linux MIPS-64 BE /usr
8332 Linux MIPS-32 LE /usr               8333 Linux MIPS-64 LE /usr
8334 Linux PA-RISC /usr                  8335 Linux PowerPC-32 /usr
8336 Linux PowerPC-64 BE /usr            8337 Linux PowerPC-64 LE /usr
8338 Linux RISC-V-32 /usr                8339 Linux RISC-V-64 /usr
833a Linux s390 /usr                     833b Linux s390x /usr
833c Linux TILE-Gx /usr                  833d Linux Alpha root verity
833e Linux ARC root verity               833f Linux LoongArch root verity
8340 Linux MIPS-32 BE root verity        8341 Linux MIPS-64 BE root verity
8342 Linux MIPS-32 LE root verity        8343 Linux MIPS-64 LE root verity
8344 Linux PA-RISC root verity           8345 Linux PowerPC-64 LE root verity
8346 Linux PowerPC-64 BE root verity     8347 Linux PowerPC-32 root verity
8348 Linux RISC-V-32 root verity         8349 Linux RISC-V-64 root verity
834a Linux s390 root verity              834b Linux s390x root verity
834c Linux TILE-Gx root verity           834d Linux Alpha /usr verity
834e Linux ARC /usr verity               834f Linux LoongArch /usr verity
8350 Linux MIPS-32 BE /usr verity        8351 Linux MIPS-64 BE /usr verity
8352 Linux MIPS-32 LE /usr verity        8353 Linux MIPS-64 LE /usr verity
8354 Linux PA-RISC /usr verity           8355 Linux PowerPC-64 LE /usr verity
8356 Linux PowerPC-64 BE /usr verity     8357 Linux PowerPC-32 /usr verity
8358 Linux RISC-V-32 /usr verity         8359 Linux RISC-V-64 /usr verity
835a Linux s390 /usr verity              835b Linux s390x /usr verity
835c Linux TILE-Gx /usr verity           835d Linux Alpha root verity signature
835e Linux ARC root verity signature     835f Linux ARM32 root verity signature
8360 Linux ARM64 root verity signature   8361 Linux IA-64 root verity signature
8362 Linux LoongArch root verity signat  8363 Linux MIPS-32 BE root verity signa
8364 Linux MIPS-64 BE root verity signa  8365 Linux MIPS-32 LE root verity signa
8366 Linux MIPS-64 LE root verity signa  8367 Linux PA-RISC root verity signatur
8368 Linux PowerPC-64 LE root verity si  8369 Linux PowerPC-64 BE root verity si
836a Linux PowerPC-32 root verity signa  836b Linux RISC-V-32 root verity signat
836c Linux RISC-V-64 root verity signat  836d Linux s390 root verity signature
836e Linux s390x root verity signature   836f Linux TILE-Gx root verity signatur
8370 Linux x86-64 root verity signature  8371 Linux x86 root verity signature
8372 Linux Alpha /usr verity signature   8373 Linux ARC /usr verity signature
8374 Linux ARM32 /usr verity signature   8375 Linux ARM64 /usr verity signature
8376 Linux IA-64 /usr verity signature   8377 Linux LoongArch /usr verity signat
8378 Linux MIPS-32 BE /usr verity signa  8379 Linux MIPS-64 BE /usr verity signa
837a Linux MIPS-32 LE /usr verity signa  837b Linux MIPS-64 LE /usr verity signa
837c Linux PA-RISC /usr verity signatur  837d Linux PowerPC-64 LE /usr verity si
837e Linux PowerPC-64 BE /usr verity si  837f Linux PowerPC-32 /usr verity signa
8380 Linux RISC-V-32 /usr verity signat  8381 Linux RISC-V-64 /usr verity signat
8382 Linux s390 /usr verity signature    8383 Linux s390x /usr verity signature
8384 Linux TILE-Gx /usr verity signatur  8385 Linux x86-64 /usr verity signature
8386 Linux x86 /usr verity signature     8400 Intel Rapid Start
8401 SPDK block device                   8500 Container Linux /usr
8501 Container Linux resizable rootfs    8502 Container Linux /OEM customization
8503 Container Linux root on RAID        8e00 Linux LVM
a000 Android bootloader                  a001 Android bootloader 2
a002 Android boot 1                      a003 Android recovery 1
a004 Android misc                        a005 Android metadata
a006 Android system 1                    a007 Android cache
a008 Android data                        a009 Android persistent
a00a Android factory                     a00b Android fastboot/tertiary
a00c Android OEM                         a00d Android vendor
a00e Android config                      a00f Android factory (alt)
a010 Android meta                        a011 Android EXT
a012 Android SBL1                        a013 Android SBL2
a014 Android SBL3                        a015 Android APPSBL
a016 Android QSEE/tz                     a017 Android QHEE/hyp
a018 Android RPM                         a019 Android WDOG debug/sdi
a01a Android DDR                         a01b Android CDT
a01c Android RAM dump                    a01d Android SEC
a01e Android PMIC                        a01f Android misc 1
a020 Android misc 2                      a021 Android device info
a022 Android APDP                        a023 Android MSADP
a024 Android DPO                         a025 Android recovery 2
a026 Android persist                     a027 Android modem ST1
a028 Android modem ST2                   a029 Android FSC
a02a Android FSG 1                       a02b Android FSG 2
a02c Android SSD                         a02d Android keystore
a02e Android encrypt                     a02f Android EKSST
a030 Android RCT                         a031 Android spare1
a032 Android spare2                      a033 Android spare3
a034 Android spare4                      a035 Android raw resources
a036 Android boot 2                      a037 Android FOTA
a038 Android system 2                    a039 Android cache
a03a Android user data                   a03b LG (Android) advanced flasher
a03c Android PG1FS                       a03d Android PG2FS
a03e Android board info                  a03f Android MFG
a040 Android limits                      a200 Atari TOS basic data
a500 FreeBSD disklabel                   a501 FreeBSD boot
a502 FreeBSD swap                        a503 FreeBSD UFS
a504 FreeBSD ZFS                         a505 FreeBSD Vinum/RAID
a506 FreeBSD nandfs                      a580 Midnight BSD data
a581 Midnight BSD boot                   a582 Midnight BSD swap
a583 Midnight BSD UFS                    a584 Midnight BSD ZFS
a585 Midnight BSD Vinum                  a600 OpenBSD disklabel
a800 Apple UFS                           a901 NetBSD swap
a902 NetBSD FFS                          a903 NetBSD LFS
a904 NetBSD concatenated                 a905 NetBSD encrypted
a906 NetBSD RAID                         ab00 Recovery HD
af00 Apple HFS/HFS+                      af01 Apple RAID
af02 Apple RAID offline                  af03 Apple label
af04 AppleTV recovery                    af05 Apple Core Storage
af06 Apple SoftRAID Status               af07 Apple SoftRAID Scratch
af08 Apple SoftRAID Volume               af09 Apple SoftRAID Cache
af0a Apple APFS                          af0b Apple APFS Pre-Boot
af0c Apple APFS Recovery                 b000 U-Boot boot loader
b300 QNX6 Power-Safe                     bb00 Barebox boot loader
bc00 Acronis Secure Zone                 be00 Solaris boot
bf00 Solaris root                        bf01 Solaris /usr & Mac ZFS
bf02 Solaris swap                        bf03 Solaris backup
bf04 Solaris /var                        bf05 Solaris /home
bf06 Solaris alternate sector            bf07 Solaris Reserved 1
bf08 Solaris Reserved 2                  bf09 Solaris Reserved 3
bf0a Solaris Reserved 4                  bf0b Solaris Reserved 5
c001 HP-UX data                          c002 HP-UX service
e100 ONIE boot                           e101 ONIE config
e900 Veracrypt data                      ea00 XBOOTLDR partition
eb00 Haiku BFS                           ed00 Sony system partition
ed01 Lenovo system partition             ef00 EFI system partition
ef01 MBR partition scheme                ef02 BIOS boot partition
f100 Fuchsia boot loader (slot A/B/R)    f101 Fuchsia durable mutable encrypted
f102 Fuchsia durable mutable boot loade  f103 Fuchsia factory ro system data
f104 Fuchsia factory ro bootloader data  f105 Fuchsia Volume Manager
f106 Fuchsia verified boot metadata (sl  f107 Fuchsia Zircon boot image (slot A/
f108 Fuchsia ESP                         f109 Fuchsia System
f10a Fuchsia Data                        f10b Fuchsia Install
f10c Fuchsia Blob                        f10d Fuchsia FVM
f10e Fuchsia Zircon boot image (slot A)  f10f Fuchsia Zircon boot image (slot B)
f110 Fuchsia Zircon boot image (slot R)  f111 Fuchsia sys-config
f112 Fuchsia factory-config              f113 Fuchsia bootloader
f114 Fuchsia guid-test                   f115 Fuchsia verified boot metadata (A)
f116 Fuchsia verified boot metadata (B)  f117 Fuchsia verified boot metadata (R)
f118 Fuchsia misc                        f119 Fuchsia emmc-boot1
f11a Fuchsia emmc-boot2                  f800 Ceph OSD
f801 Ceph dm-crypt OSD                   f802 Ceph journal
f803 Ceph dm-crypt journal               f804 Ceph disk in creation
f805 Ceph dm-crypt disk in creation      f806 Ceph block
f807 Ceph block DB                       f808 Ceph block write-ahead log
f809 Ceph lockbox for dm-crypt keys      f80a Ceph multipath OSD
f80b Ceph multipath journal              f80c Ceph multipath block 1
f80d Ceph multipath block 2              f80e Ceph multipath block DB
f80f Ceph multipath block write-ahead l  f810 Ceph dm-crypt block
f811 Ceph dm-crypt block DB              f812 Ceph dm-crypt block write-ahead lo
f813 Ceph dm-crypt LUKS journal          f814 Ceph dm-crypt LUKS block
f815 Ceph dm-crypt LUKS block DB         f816 Ceph dm-crypt LUKS block write-ahe
f817 Ceph dm-crypt LUKS OSD              fb00 VMWare VMFS
fb01 VMWare reserved                     fc00 VMWare kcore crash protection
fd00 Linux RAID

```
