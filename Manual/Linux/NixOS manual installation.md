
- partitioning and formatting
```bash
# partitoning using parted 
# create GPT partition table
parted /dev/sda -- mklabel gpt
# create root partition and fill the disk except last 8GB and 512MB for boot
parted /dev/sda -- mkpart primary 512MB -8GB
# add swap partition
parted /dev/sda -- mkpart primary linux-swap -8GB 100%
# create boot and give it the reserverd 512MB
parted /dev/sda -- mkpart ESP fat32 1MB 512MB
parted /dev/sda -- set 3 esp on
####
# create msdos partition table
parted /dev/sda -- mklabel msdos
# create root partition except last 8GB
parted /dev/sda -- mkpart primary 1MB -8GB
# create swap partition 
parted /dev/sda -- mkpart primary linux-swap -8GB 100%
# write to partitions
mkfs.ext4 -L nixos /dev/sda1
mkswap -L swap /dev/sda2
mkfs.fat -F 32 -n boot /dev/sda3
```

```bash
mount /dev/disk/by-label/nixos /mnt
# mount boot in UEFI
mkdir -p /mnt/boot
mount /dev/disk/by-label/boot /mnt/boot
```

```bash
nixos-generate-config --root /mnt
nano /mnt/etc/nixos/configuration.nix
```
- notes
```bash
# specify which disk the GRUB boot to be installed on
boot.loader.grub.device
# set to true if doing dualboot
boot.loader.grub.useOSProber
# set to true if using UEFI 
boot.loader.systemd-boot.enable

```