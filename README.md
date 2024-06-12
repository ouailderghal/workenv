# Work Environment

## Arch Installation Guide (quick version)

```shell
gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
sudo dd if=~/Downloads/archlinux-version-x86_64.iso of=/dev/sdX status=progress bs=1M
```

```shell
loadkeys fr

fdisk -l
fdisk /dev/nvme0n1

mkfs.fat -F32 /dev/nvme0n1p1
mkfs.ext4 /dev/nvme0n1p2
mkfs.ext4 -F32 /dev/nvme0n1p3

mount /dev/nvme0n1p2 /mnt
mkdir -p /mnt/{boot,home}
mount /dev/nvme0n1p1 /mnt/boot
mount /dev/nvme0n1p3 /mnt/home

lsblk
```

```shell
pacstrap -K /mnt \
    base base-devel \
    linux linux-firmware sof-firmware \
    man-db man-pages man-info \
    vim networkmanager grub efibootmgr

genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

```shell
ln -sf /usr/share/zoneinfo/Europe/Paris /etc/localtime
hwclock --systohc

vim /etc/locale.gen
locale-gen

echo "LANG=en_US.UTF-8" > /etc/locale.conf
echo "KEYMAP=fr-latin9" > /etc/vconsole.conf
echo "arch" > /etc/hostname

mkinitcpio -P linux
passwd root

grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ARCH
grub-mkconfig -o /boot/grub/grub.cfg

systemctl enable NetworkManager
```
