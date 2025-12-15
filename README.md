# 4Kn-Formatting
Convert & Format drives with 4096 PSS/LSS (phisical/logical-sector-size), LBS (logical-block-size) is also called LBA (logical-block-address).

## A minimalistic instruction-set for newbies & proofs, about switching of NVMe DC (Data-Carrier) to 4Kn (4K/4096 natural) LSS/LBS/LBA. 

* **Note:** For extensive instructions & more, see/read [Extensive-Readme](https://github.com/Advantaged/4Kn-Formatting/blob/main/Extensive-Readme.md) file.

### Step 1. Install missing programs if needed:

* **Archlinux: (using pacman)**

```
sudo pacman -S --needed --noconfirm nvme-cli gptfdisk multipath-tools
```

* **Debian/Ubuntu (using apt):**

```
sudo apt install --yes nvme-cli gptfdisk multipath-tools
```

* **Red Hat/Fedora (using dnf):**

```
sudo dnf install --assumeyes nvme-cli gptfdisk multipath-tools
```

* **openSUSE (using zypper):**

```
sudo zypper install --non-interactive nvme-cli gptfdisk multipath-tools
```

* **Gentoo Linux (using emerge):**

```
sudo emerge --ask=n nvme-cli gptfdisk multipath-tools
```

### Step 2. Check LBA-status/mode and possibility to 'format namespace':

* **-Code:**

```
sudo nvme id-ns /dev/nvme0n1 -H | grep LBA
```

* **-output:**

```
LBA Format  0 : Metadata Size: 0   bytes - Data Size: 512 bytes - Relative Performance: 0x2 Good (in use)
LBA Format  1 : Metadata Size: 0   bytes - Data Size: 4096 bytes - Relative Performance: 0x1 Better
```
### Step 3. Format or switch the drive:

* **-Code:**

```
sudo nvme format /dev/nvme0n1 -l 1
```

* **-output:**

```
Success formatting namespace:1
```

- **Note**: This tell the 'nvme'-controller to use the storage cells in '4KiB'-blocks. Of course, due change/switch of blocks-dimension are all data lost, hence is called 'formatting'.

### 4. Recheck LBA-status/mode:

* **-Code:**

```
sudo nvme id-ns /dev/nvme0n1 -H | grep LBA
```

* **-output:**

```
LBA Format  0 : Metadata Size: 0   bytes - Data Size: 512 bytes - Relative Performance: 0x2 Good 
LBA Format  1 : Metadata Size: 0   bytes - Data Size: 4096 bytes - Relative Performance: 0x1 Better (in use)
```


- ✅ **Done & Enjoy**❗️ :wink:
