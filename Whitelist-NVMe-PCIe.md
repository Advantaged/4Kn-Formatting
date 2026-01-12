# Whitelist-NVMe-PCIe.md

(Alphabetical sorted)

## NVMe:

* **Note:** NVMe having SATA-controller are NOT LISTED❗️

### NVMe-PCIe:

- **Corsair MP600**, since at least ***MP5xx*** series.
- **Crucial T700**
- **Intel 750** (only series)
- **Kingston:**
  - **KC3000 and DC2000M** (enterprise) models support 4Kn formatting if firmware allows. Verified using `openSeaChest_Format`.
- KIOXIA:
  - **CD6-R** and **CM7** series (enterprise) support 4Kn formatting across various capacities, including 1.92 TB and 3.84 TB.
- **Micron:**
  - **7400/7450 series** (including models ≤3.84 TB) support switching from 512e to 4Kn via `nvme format -l 1`.
  - **The 9400/9550 series** also support configurable sector sizes, even on sub-4TB capacities.
- **Seagate:**
  - **Nytro 5000/7000 series** (enterprise NVMe) support 4Kn on capacities as low as 1.92 TB.
- **Solidigm (formerly Intel):**
  - **D5-P5316** and select **P5520/P5620** models support 4Kn, though most are ≥4TB. Some lower-capacity enterprise drives are configurable.


## HDD:

* **Note:** For avoiding data-loss choose only DC having 'Conventional Magnetic Recording (CMR)'.

### SAS-HDD:

* List/offers per 
[2025-04-01](https://geizhals.de/?cat=hde7s&xf=1080_SAS+12Gb%2Fs%7E3772_3.5%7E8457_Conventional+Magnetic+Recording+(CMR)
%7E8627_4KB+nativ+(4Kn)). For server-applications, click on 'Datacenter'.

### SATA-Express-HDD❓:

* List/offers not yet available.

### SATA-HDD:

* List/offers per 
[2025-04-01](https://geizhals.de/?cat=hde7s&xf=1080_SATA+6Gb%2Fs%7E3772_3.5%7E8457_Conventional+Magnetic+Recording+(CMR)
%7E8627_4KB+nativ+(4Kn)). For server-applications, click on 'Datacenter'.

## SSD:

* This is at moment still a 'black-box' 🟰 no information available.

### SATA-SSD:



