For USB drives or partitions that are not mounting correctly:
### Identify the Device

```bash
sudo fdisk -l
```

Install NTFS support:

```bash
sudo apt install ntfs-3g
```

Repair an NTFS partition:

```bash
sudo ntfsfix /dev/sdd1
```

Replace `/dev/sdd1` with the actual partition.
