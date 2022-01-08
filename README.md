Домашнее задание  
1.Sparse файлы - изучил.  
2.Жесткие ссылки на один объект не могут иметь разные права.  У файла и жесткой ссылки одинаковые inode, поэтому жесткая ссылка имеет те же права доступа.  
3.Создал новую VM с конфигурацией из задания.  
4.`sudo fdisk /dev/sdb` - создаем разделы для первого диска sdb.  

	Command (m for help): g  
	Created a new GPT disklabel (GUID: 55598F41-4285-5D44-AA4D-EE59AA1E1083).  
	Command (m for help): p  
	Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors  
	Disk model: VBOX HARDDISK   
	Units: sectors of 1 * 512 = 512 bytes  
	Sector size (logical/physical): 512 bytes / 512 bytes  
	I/O size (minimum/optimal): 512 bytes / 512 bytes  
	Disklabel type: gpt  
	Disk identifier: 55598F41-4285-5D44-AA4D-EE59AA1E1083  

	Device       Start     End Sectors  Size Type  
	/dev/sdb1     2048 3907583 3905536  1.9G Linux filesystem  
	/dev/sdb2  3907584 5242846 1335263  652M Linux filesystem  
	
	Command (m for help): O       

	Enter script file name: partitions_sdb                   

	Script successfully saved.

	Command (m for help): w
	The partition table has been altered.
	Calling ioctl() to re-read partition table.
	Syncing disks.

5.`sfdisk -d /dev/sdb | sfdisk /dev/sdc` - создаем разделы для второго диска sdc.  
  `sudo fdisk /dev/sdс` - альтернативный вариант.
  	
	Command (m for help): I   

	Enter script file name: partitions_sdb  

	Created a new GPT disklabel (GUID: 55598F41-4285-5D44-AA4D-EE59AA1E1083).  
	Created a new partition 1 of type 'Linux filesystem' and of size 1.9 GiB.  
	Created a new partition 2 of type 'Linux filesystem' and of size 652 MiB.  
	Script successfully applied.  

	Command (m for help): p  
	Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors  
	Disk model: VBOX HARDDISK   
	Units: sectors of 1 * 512 = 512 bytes  
	Sector size (logical/physical): 512 bytes / 512 bytes  
	I/O size (minimum/optimal): 512 bytes / 512 bytes  
	Disklabel type: gpt  
	Disk identifier: 55598F41-4285-5D44-AA4D-EE59AA1E1083  

	Device       Start     End Sectors  Size Type  
	/dev/sdc1     2048 3907583 3905536  1.9G Linux filesystem  
	/dev/sdc2  3907584 5242846 1335263  652M Linux filesystem  

	Command (m for help): w  
	The partition table has been altered.  
	Calling ioctl() to re-read partition table.  
	Syncing disks.  
	
6.`sudo mdadm --create device --level=1 --name=RAID1 --raid-devices=2 /dev/sdb1 /dev/sdc1sudo mdadm --create device --level=1 --name=RAID1 --raid-devices=2 /dev/sdb1 /dev/sdc1` - создаем RAID1. 
	
	mdadm: Note: this array has metadata at the start and may not be suitable as a boot device.  If you plan to store '/boot' on this device please ensure that your boot-loader understands md/v1.x metadata, or use --metadata=0.90  
	Continue creating array? y
	mdadm: Defaulting to version 1.2 metadata
	mdadm: array /dev/md/device started.
7.`sudo mdadm --create device2 --level=0 --name=RAID0 --raid-devices=2 /dev/sdb2 /dev/sdc2` - создаем RAID0. 
	
	mdadm: Defaulting to version 1.2 metadata
	mdadm: array /dev/md/device2 started.
8.`sudo pvcreate /dev/md/device /dev/md/device2` - создаем два независимых PV из RAID-массивов.
	
	Physical volume "/dev/md/device" successfully created.
	Physical volume "/dev/md/device2" successfully created.
9.`sudo vgcreate volume-group /dev/md126 /dev/md127` - создаем группу для PV.  
	
	Volume group "volume-group" successfully created  
10.`sudo lvcreate -L 100Mmd126 volume-group /dev/md126`  - создаем раздел 100Мб на PV c RAID 0.  
	
	Logical volume "lvol0" created.	
`sudo lvs -a -o +devices` - проверка.  
	
	LV        VG           Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert Devices        
	ubuntu-lv ubuntu-vg    -wi-ao----  31.50g                                                     /dev/sda3(0)   
	lvol0     volume-group -wi-a----- 100.00m                                                     /dev/md126(0)
	
11.`sudo mkfs.ext4 /dev/volume-group/lvol0`  
	
	mke2fs 1.45.5 (07-Jan-2020)  
	Creating filesystem with 25600 4k blocks and 25600 inodes  

	Allocating group tables: done                            
	Writing inode tables: done                            
	Creating journal (1024 blocks): done  
	Writing superblocks and filesystem accounting information: done  
12.`mkdir /tmp/new && sudo mount /dev/volume-group/lvol0 /tmp/new` - монтируем файловую систему.  
13.`sudo wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz`  - копируем тестовый файл.
	
	--2022-01-08 17:09:15--  https://mirror.yandex.ru/ubuntu/ls-lR.gz  
	Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183  
	Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.  
	HTTP request sent, awaiting response... 200 OK  
	Length: 21595359 (21M) [application/octet-stream]  
	Saving to: ‘/tmp/new/test.gz’  

	/tmp/new/test.gz                                     100%[=====================================================================================================================>]  20.59M  5.74MB/s    in 3.5s    

	2022-01-08 17:09:18 (5.94 MB/s) - ‘/tmp/new/test.gz’ saved [21595359/21595359]  
14.`lsblk`  
	
	NAME                      MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT  
	loop0                       7:0    0 55.4M  1 loop  /snap/core18/2128  
	loop1                       7:1    0 70.3M  1 loop  /snap/lxd/21029  
	loop2                       7:2    0 32.3M  1 loop  /snap/snapd/12704  
	loop3                       7:3    0 43.3M  1 loop  /snap/snapd/14295  
	loop4                       7:4    0 55.5M  1 loop  /snap/core18/2253  
	loop5                       7:5    0 61.9M  1 loop  /snap/core20/1270  
	loop6                       7:6    0 67.2M  1 loop  /snap/lxd/21835  
	sda                         8:0    0   64G  0 disk   
	├─sda1                      8:1    0    1M  0 part  
	├─sda2                      8:2    0    1G  0 part  /boot  
	└─sda3                      8:3    0   63G  0 part  
  	└─ubuntu--vg-ubuntu--lv 253:0    0 31.5G  0 lvm   /  
	sdb                         8:16   0  2.5G  0 disk  
	├─sdb1                      8:17   0  1.9G  0 part  
	│ └─md127                   9:127  0  1.9G  0 raid1  
	└─sdb2                      8:18   0  652M  0 part  
  	└─md126                   9:126  0  1.3G  0 raid0  
    	└─volume--group-lvol0 253:1    0  100M  0 lvm   /tmp/new  
	sdc                         8:32   0  2.5G  0 disk  
	├─sdc1                      8:33   0  1.9G  0 part  
	│ └─md127                   9:127  0  1.9G  0 raid1  
	└─sdc2                      8:34   0  652M  0 part  
  	└─md126                   9:126  0  1.3G  0 raid0  
    	└─volume--group-lvol0 253:1    0  100M  0 lvm   /tmp/new  
15.`gzip -t /tmp/new/test.gz` - тестируем архив.  
`echo $?` - статус выхода последней выполненной команды. 
	
	0
16.`sudo pvmove /dev/md126 /dev/md127`- переносим с RAID0 на RAID1.

	/dev/md126: Moved: 16.00%  
	/dev/md126: Moved: 100.00%  
17.`sudo mdadm /dev/md/device -f /dev/sdb1` - помечаем один диск в RAID как плохой.  
	
	mdadm: set /dev/sdb1 faulty in /dev/md/device
18.`dmesg` - смотрим в логах.
	
	[ 6444.518109] md/raid1:md127: Disk failure on sdb1, disabling device.  
                       md/raid1:md127: Operation continuing on 1 devices.  

`sudo mdadm --detail /dev/md/device` - проверяем статус массива. degraded.  

	/dev/md/device:  
           Version : 1.2  
     Creation Time : Sat Jan  8 16:16:43 2022  
        Raid Level : raid1  
        Array Size : 1950720 (1905.00 MiB 1997.54 MB)  
     Used Dev Size : 1950720 (1905.00 MiB 1997.54 MB)  
      Raid Devices : 2  
     Total Devices : 2  
       Persistence : Superblock is persistent  

       Update Time : Sat Jan  8 17:31:20 2022  
             State : clean, degraded  
    Active Devices : 1  
    Working Devices : 1  
    Failed Devices : 1  
     Spare Devices : 0  

    Consistency Policy : resync  

              Name : vagrant:RAID1  (local to host vagrant)  
              UUID : ddd680b0:355e8d71:078f91ac:8498e7e6  
            Events : 19  

    Number   Major   Minor   RaidDevice State  
       -       0        0        0      removed  
       1       8       33        1      active sync   /dev/sdc1  

       0       8       17        -      faulty   /dev/sdb1  	
19.`gzip -t /tmp/new/test.gz` - тестируем архив.  
`echo $?` - статус выхода последней выполненной команды. 
	
	0
20. `vagrant destroy`
