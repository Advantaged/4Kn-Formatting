#### Another important topic to be considered by choosing a file-system is the so called overhead, this primarily concerns `bcachefs`, `btrfs` & `zfs`.
we don't consider at this point the "old" standard `ext4` file-system, the reason for this is the lack of further development on this FS, e.g. the necessity to use twice the option force (`-F`) as you in [Extensive-Readme](https://github.com/Advantaged/4Kn-Formatting/blob/main/Extensive-Readme.md) can read.
we consider only the future-oriented FS line `bcachefs`, `btrfs` & `zfs`.

### Partitioning-Example based on a 2-TB (~1863 GiB) NVMe:
- 4 GiB `ef00` (EFI).
- 1850 GiB `bf00` (ZFS-ROOT).
- ~9 GiB free/empty @ end of DC (data-carrier), ca. 5 GiB on e.g. 500 GB DC.
- NO `swap` & NO `zswap` BUT `zram` configured according to physical RAM

### `zfs` Overhead

The ZFS overhead consists of:

- **~3.2% "slop space"** (reserved via `spa_slop_shift`, default value 1/32): ~59 GiB
- **Metadata & labels:** ~1–2 GiB (vdev labels, boot loader, alignment)

Total overhead: **~60–62 GiB**

So from 1850 GiB, expect **~1788–1790 GiB usable** after ZFS overhead.

### `btrfs` Overhead
- **Metadata allocation:** Dynamically allocated; can become full independently of data space.
- **No fixed slop reserve:** Unlike ZFS, no hardcoded percentage (e.g., 3.2%), but requires free space for rebalancing and metadata operations.
- **ENOSPC issues:** Common when metadata block groups are full, even if data space remains.
- **Recommended free space:** ~5–10% for healthy operation and balance tasks.
- **Copy-on-Write (CoW):** Increases write amplification and fragmentation over time.

### `bcachefs` Overhead
- **Self-managed allocation:** Uses flexible data placement across storage targets (foreground, background, promote).
- **Metadata & journaling:** Minimal fixed overhead; metadata scales with data.
- **No enforced slop space:** Efficient space utilization without reserved pools.
- **Encryption overhead:** If enabled (ChaCha20Poly1305), adds CPU and space for auth tags.
- **Typical usable space:** ~97–98% of raw, depending on replication and configuration.

### Final considerations & (individual) recommendations
- Having enough CPU-Cores, enough RAM & want trim the system for speed… use a mature FS like `zfs` with throughout `lz4` compression.
- `btrfs` have to much overhead, still need `grub` to boot snapshots… not so much experienced FS & uncertain need of space on DCs.
- `bcachrfs` should close the gap between `zfs` & `btrfs` but still too jung (for my taste) to be used in productional envirnment-s.
- The best Linux-Distro/OS in 2025 is [CachyOS](https://cachyos.org/download/) with `limine` bootloader & standard KDE-/Plasma-DE.



