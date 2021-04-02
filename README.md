# LVM

Создание lvm

```sh
yum install lvm2 hfsdump

lvm
vcreate /dev/sdb
vgcreate vg_root /dev/sdb
lvcreate -n lv_root -l100%FREE /dev/vg_root

mkfs.xfs /dev/vg_root/lv_root
mount /dev/vg_root/lv_root /mnt
xfsdump -J - /dev/sda1 |xfsrestore -J - /mnt
umount /dev/sda1
grub2-mkconfig -o /boot/grub2/grub.cfg
grub2-mkconfig /boot/grub2/grub.cfg /dev/vg_root/lv_root
telinit 6
```

Для запуска  скрипта:

```sh
scriptreplay <time.file> <log.file>  
```

# 3.LVM

### Дополнительно
LVM snapshots explained [hier](https://www.it610.com/article/2406845.htm)  
BTRFS для самых маленьких [hier](https://habr.com/ru/company/veeam/blog/458250/)    
Файл дескриптор в Linux с примерами [hier](https://habr.com/ru/post/471038/)    
