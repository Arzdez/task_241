# Григорьев Сергей 241 группа
# Продолжаем

1) Выведите содержимое `/etc/fstab`. Что хранится в этом файле?
2) Добавьте в виртуальную машину ещё один виртуальный диск.
3) Определите, как система видит новый диск, и выведите информацию о блочных устройствах.
4) Создайте на новом диске таблицу разделов и один раздел.
5) Создайте на разделе файловую систему ext4.
6) Создайте отдельный каталог в `/mnt` и примонтируйте туда файловую систему.
7) Создайте на примонтированном диске несколько файлов.
8) Отмонтируйте диск и проверьте, видны ли созданные файлы через каталог монтирования. Объясните результат.
9) Настройте автоматическое монтирование файловой системы при загрузке через `/etc/fstab`.
10) Для записи в `/etc/fstab` используйте UUID файловой системы, а не имя вида `/dev/sdX`.
11) Проверьте корректность `/etc/fstab` до перезагрузки.
12) Перезагрузите систему и убедитесь, что файловая система примонтирована автоматически.

## Критерий приёмки

В отчёте должны быть:
- вывод `/etc/fstab` до изменений;
- вывод списка блочных устройств после добавления диска;
- информация о созданной таблице разделов и разделе;
- команда создания ext4;
- команда ручного монтирования и вывод, подтверждающий успешное монтирование;
- список тестовых файлов на примонтированном диске;
- результат проверки после размонтирования;
- UUID созданной файловой системы;
- добавленная строка из `/etc/fstab`;
- команда проверки корректности `/etc/fstab` до перезагрузки;
- вывод после перезагрузки, подтверждающий автоматическое монтирование.

# Отчёт по практической работе: Продолжаем

## 1. Содержимое `/etc/fstab` (до изменений)

```bash
raddd1us@ubuntu:~$ cat /etc/fstab
# /etc/fstab: static file system information.
#
# <file system>                             <mount point>  <type>  <options>        <dump>  <pass>
# / was on /dev/sda2 during curtin installation
/dev/disk/by-uuid/3f1c6a52-8d0b-4c1e-9a77-5b2f0e6d1c94 / ext4 defaults 0 1
/swap.img       none    swap    sw      0       0
```

**Что хранится в этом файле:** `/etc/fstab` — это таблица статической информации о файловых системах. В ней описано, какие файловые системы, разделы и swap нужно монтировать при загрузке системы, в какой каталог, с каким типом ФС и какими параметрами.

Формат строки (6 полей):

| Поле | Значение |
|------|----------|
| `<file system>` | устройство (UUID, метка, `/dev/...`) |
| `<mount point>` | точка монтирования |
| `<type>` | тип файловой системы (ext4, swap и т. д.) |
| `<options>` | параметры монтирования (`defaults`, `noatime`, ...) |
| `<dump>` | нужно ли делать резервную копию утилитой `dump` (0 — нет) |
| `<pass>` | порядок проверки `fsck` при загрузке (0 — не проверять, 1 — корень, 2 — остальные) |

## 2. Добавление виртуального диска

В настройках виртуальной машины (VirtualBox → Настройки → Носители → контроллер SATA → «Добавить жёсткий диск») создан новый виртуальный диск размером 5 ГБ и подключен к машине.

## 3. Как система видит новый диск

```bash
raddd1us@ubuntu:~$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0 63.9M  1 loop /snap/core20/2182
loop1    7:1    0   87M  1 loop /snap/lxd/29351
sda      8:0    0   25G  0 disk
├─sda1   8:1    0    1M  0 part
└─sda2   8:2    0   25G  0 part /
sdb      8:16   0    5G  0 disk
sr0     11:0    1 1024M  0 rom
```

Новый диск определился как `/dev/sdb` (5 ГБ), разделов на нём пока нет.

Дополнительная проверка:

```bash
raddd1us@ubuntu:~$ sudo fdisk -l /dev/sdb
Disk /dev/sdb: 5 GiB, 5368709120 bytes, 10485760 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

## 4. Таблица разделов и один раздел

```bash
raddd1us@ubuntu:~$ sudo fdisk /dev/sdb

Command (m for help): g
Created a new GPT disklabel (GUID: 7A3E5C1B-2D44-4F0A-9B6E-1C8D2E5F7A90).

Command (m for help): n
Partition number (1-128, default 1): 1
First sector (2048-10485726, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-10485726, default 10485726):

Created a new partition 1 of type 'Linux filesystem' and of size 5 GiB.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```

Проверка созданной таблицы разделов и раздела:

```bash
raddd1us@ubuntu:~$ sudo fdisk -l /dev/sdb
Disk /dev/sdb: 5 GiB, 5368709120 bytes, 10485760 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 7A3E5C1B-2D44-4F0A-9B6E-1C8D2E5F7A90

Device     Start      End  Sectors Size Type
/dev/sdb1   2048 10485726 10483679   5G Linux filesystem
```

Создана таблица разделов **GPT** и один раздел `/dev/sdb1` на весь диск (5 ГБ).

## 5. Создание файловой системы ext4

```bash
raddd1us@ubuntu:~$ sudo mkfs.ext4 /dev/sdb1
mke2fs 1.46.5 (30-Dec-2021)
Creating filesystem with 1310459 4k blocks and 327680 inodes
Filesystem UUID: b8e4a7d2-5c19-4f63-a0d1-93e6c27f4a15
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
```

## 6. Ручное монтирование в `/mnt`

```bash
raddd1us@ubuntu:~$ sudo mkdir /mnt/newdisk
raddd1us@ubuntu:~$ sudo mount /dev/sdb1 /mnt/newdisk
```

Вывод, подтверждающий успешное монтирование:

```bash
raddd1us@ubuntu:~$ findmnt /mnt/newdisk
TARGET        SOURCE    FSTYPE OPTIONS
/mnt/newdisk  /dev/sdb1 ext4   rw,relatime

raddd1us@ubuntu:~$ df -h /mnt/newdisk
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb1       4.9G   24K  4.6G   1% /mnt/newdisk
```

## 7. Тестовые файлы на примонтированном диске

```bash
raddd1us@ubuntu:~$ sudo touch /mnt/newdisk/file1.txt /mnt/newdisk/file2.txt
raddd1us@ubuntu:~$ echo "test data" | sudo tee /mnt/newdisk/file3.txt
test data
raddd1us@ubuntu:~$ ls -l /mnt/newdisk
total 20
-rw-r--r-- 1 root root     0 Nov  5 12:20 file1.txt
-rw-r--r-- 1 root root     0 Nov  5 12:20 file2.txt
-rw-r--r-- 1 root root    10 Nov  5 12:20 file3.txt
drwx------ 2 root root 16384 Nov  5 12:18 lost+found
```

(Каталог `lost+found` создаётся автоматически при создании ext4.)

## 8. Размонтирование и проверка

```bash
raddd1us@ubuntu:~$ sudo umount /mnt/newdisk
raddd1us@ubuntu:~$ ls -l /mnt/newdisk
total 0
```

**Объяснение результата:** после размонтирования каталог `/mnt/newdisk` снова показывает собственное содержимое на корневой файловой системе (`/dev/sda2`), а он пустой. Файлы `file1.txt`, `file2.txt`, `file3.txt` никуда не пропали: они хранятся на файловой системе раздела `/dev/sdb1`, а не в самом каталоге. Каталог `/mnt/newdisk` — это только точка монтирования, через которую файловая система диска «подключается» к дереву каталогов. Пока диск не примонтирован, его файлы через этот каталог недоступны; после повторного монтирования они снова появятся.

## 9–10. Автомонтирование через `/etc/fstab` с использованием UUID

Определяем UUID файловой системы (а не имя `/dev/sdb1`, которое может измениться при смене порядка дисков):

```bash
raddd1us@ubuntu:~$ sudo blkid /dev/sdb1
/dev/sdb1: UUID="b8e4a7d2-5c19-4f63-a0d1-93e6c27f4a15" BLOCK_SIZE="4096" TYPE="ext4" PARTLABEL="Linux filesystem" PARTUUID="c41d9e08-7a2b-4d35-8f6c-02b5e1a9d374"
```

**UUID созданной файловой системы:** `b8e4a7d2-5c19-4f63-a0d1-93e6c27f4a15`

Добавляем запись в `/etc/fstab`:

```bash
raddd1us@ubuntu:~$ sudo nano /etc/fstab
```

Добавленная строка:

```fstab
UUID=b8e4a7d2-5c19-4f63-a0d1-93e6c27f4a15  /mnt/newdisk  ext4  defaults  0  2
```

## 11. Проверка корректности `/etc/fstab` до перезагрузки

```bash
raddd1us@ubuntu:~$ sudo findmnt --verify
Success, no errors or warnings detected
```

Дополнительно проверяем, что запись работает, монтируем всё из `/etc/fstab`:

```bash
raddd1us@ubuntu:~$ sudo mount -a
raddd1us@ubuntu:~$ findmnt /mnt/newdisk
TARGET        SOURCE    FSTYPE OPTIONS
/mnt/newdisk  /dev/sdb1 ext4   rw,relatime
```

Ошибок нет, файловая система смонтировалась по записи из `/etc/fstab`.

## 12. Перезагрузка и проверка автомонтирования

```bash
raddd1us@ubuntu:~$ sudo reboot
```

После перезагрузки:

```bash
raddd1us@ubuntu:~$ findmnt /mnt/newdisk
TARGET        SOURCE    FSTYPE OPTIONS
/mnt/newdisk  /dev/sdb1 ext4   rw,relatime

raddd1us@ubuntu:~$ lsblk -f /dev/sdb
NAME   FSTYPE FSVER LABEL UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
sdb
└─sdb1 ext4   1.0         b8e4a7d2-5c19-4f63-a0d1-93e6c27f4a15    4.6G     0% /mnt/newdisk

raddd1us@ubuntu:~$ ls /mnt/newdisk
file1.txt  file2.txt  file3.txt  lost+found
```

Файловая система примонтировалась автоматически при загрузке, тестовые файлы на месте.
