# Arch VMware Install

Reference: https://wiki.archlinux.org/index.php/installation_guide

- Firmware type: EFI

```bash
fdisk -l

fdisk /dev/sda
  n
  p <default>
  2048 <default>
  +512M
  t
  ef
  n
  p <default>
  <default>
  <default>
  w

mkfs.ext4 /dev/sda2
mount /dev/sda2 /mnt
pacstrap /mnt base linux linux-firmware
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
ln -sf /usr/share/zoneinfo/America/New_York /etc/localtime
hwclock --systohc
locale-gen
pacman -Sy dosfstools grub efibootmgr
mkfs.fat -F32 /dev/sda1
mount /dev/sda1 /boot/efi
grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
passwd
exit
reboot
```
