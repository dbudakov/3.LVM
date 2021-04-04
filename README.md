# LVM

Расширение LVM
```sh
yum install lvm2

fdisk /dev/sdb 
	n
	p
	3
	t
	8e
	w
partprobe
pvcreate /dev/sdb1
vgdisplay| awk '/Name/ {print $2}'
vgcreate vg_root /dev/sdb1
lvdisplay| awk '/Name/ {print $2}'
lvcreate -n lv_root -l100%FREE /dev/vg_root
mkfs.xfs /dev/vg_root/lv_root 
mount /dev/vg_root/lv_root /mnt

umount /mnt
fdisk /dev/sdb 
	n
	p
	3
	t
	8e
	w
partprobe 
pvcreate /dev/sdb2
vgdisplay| awk '/Name/ {print $2}'
vgextend vg_root /dev/sdb2
lvdisplay| awk '/Name/ {print $2}'
vgdisplay |awk '/Free/ {print $5}'
lvextend -l +$(vgdisplay |awk '/Free/ {print $5}') /dev/vg_root/lv_root

# if file system is xfs use xfs_growfs, check fs `blkid`
#resize2fs /dev/vg_root/lv_root 
xfs_growfs /dev/vg_root/lv_root 
df -h
```

начало переноса primary lvm

```sh
yum install lvm2 hfsdump
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

### Дополнительно
LVM snapshots explained [hier](https://www.it610.com/article/2406845.htm)  
BTRFS для самых маленьких [hier](https://habr.com/ru/company/veeam/blog/458250/)    
Файл дескриптор в Linux с примерами [hier](https://habr.com/ru/post/471038/)    
