# Григорьев Сергей 241 группа
# Файловые системы

1) Какие файловые системы вы знаете?
2) Как можно классифицировать файловые системы? В чём основные отличия?
3) Какие файловые системы обычно используются в Linux?
4) Как создать файловую систему на блочном устройстве?
5) Что такое монтирование и как подключить файловую систему в систему?
6) Рассмотрите `procfs`, `sysfs`, `tmpfs` и `cifs`.
   6.1) В чём особенности каждой из них?
   6.2) Найдите в своей системе каталоги, куда примонтированы эти файловые системы, если они используются.
7) Как получить информацию о системе, используя содержимое виртуальных файловых систем?
   7.1) выведите информацию о процессоре;
   7.2) выведите информацию о состоянии памяти.
8) Выведите список всех текущих точек монтирования минимум двумя разными способами.

## Критерий приёмки

В отчёте должны быть:
- перечень рассмотренных типов файловых систем;
- вывод, показывающий точки монтирования `procfs`, `sysfs`, `tmpfs` и `cifs`, если она используется;
- вывод информации о CPU через `/proc`;
- вывод информации о памяти через `/proc`;
- два разных способа вывести текущие точки монтирования;
- пример определения типа файловой системы для выбранной точки монтирования;
- краткое пояснение различий между обычной дисковой файловой системой и виртуальными файловыми системами `/proc` и `/sys`.

# Отчёт по практической работе: Файловые системы

## 1. Известные файловые системы

**Дисковые (локальные):**
- `ext2`, `ext3`, `ext4` — основное семейство Linux;
- `XFS`, `JFS`, `ReiserFS` — журналируемые файловые системы;
- `Btrfs`, `ZFS` — файловые системы с copy-on-write, снимками и контрольными суммами;
- `F2FS` — для флеш-накопителей;
- `FAT16/FAT32`, `exFAT`, `NTFS` — Windows и сменные носители;
- `APFS`, `HFS+` — macOS;
- `ISO 9660`, `UDF` — оптические диски.
**Сетевые:**
- `NFS`, `CIFS/SMB` — доступ к удалённым каталогам;
- `SSHFS` (на базе FUSE), `Ceph`, `GlusterFS` — распределённые и сетевые хранилища.
**Виртуальные (псевдо-):**
- `procfs` (`/proc`), `sysfs` (`/sys`), `tmpfs`, `devtmpfs` (`/dev`), `devpts`, `cgroup2`, `debugfs`, `securityfs`.
**Наслаиваемые и сжатые:**
- `overlayfs` (контейнеры), `squashfs` (образы Live-систем, snap).

## 2. Классификация файловых систем
 
| Признак | Классы | Отличия |
|---|---|---|
| Место хранения данных | дисковые, сетевые, виртуальные | дисковые хранят данные на блочном устройстве, сетевые обращаются к удалённому серверу, виртуальные существуют в ОЗУ или отражают структуры ядра |
| Отказоустойчивость | без журнала (`ext2`, `FAT`) и журналируемые (`ext4`, `XFS`, `NTFS`) | журнал позволяет быстро восстановить согласованность после сбоя |
| Способ записи | обычные и copy-on-write (`Btrfs`, `ZFS`) | CoW не перезаписывает блоки на месте, что даёт снимки и защиту от повреждений |
| Назначение | универсальные, для флеш-памяти, для носителей только для чтения | `F2FS` учитывает износ ячеек, `squashfs`/`ISO 9660` доступны только на чтение |
| Размещение | локальные и кластерные | кластерные и сетевые обслуживают одновременный доступ с нескольких узлов |
| Поддержка прав доступа | с правами POSIX (`ext4`, `XFS`) и без них (`FAT32`, `exFAT`) | на `FAT` нет владельцев и режимов, права задаются опциями монтирования |

## 3. Файловые системы, обычно используемые в Linux
 
- `ext4` — по умолчанию в Ubuntu и Debian;
- `XFS` — по умолчанию в RHEL и производных, хорошо работает с большими файлами и томами;
- `Btrfs` — по умолчанию в openSUSE и Fedora Workstation;
- `vfat` — системный раздел EFI (`/boot/efi`);
- `tmpfs`, `proc`, `sysfs`, `devtmpfs`, `cgroup2` — служебные виртуальные системы;
- `squashfs` — образы snap-пакетов;
- `nfs`, `cifs` — сетевые ресурсы.
Узнать, какие типы поддерживает текущее ядро:
 
```bash
cat /proc/filesystems
```
 
```text
nodev	sysfs
nodev	tmpfs
nodev	bdev
nodev	proc
nodev	cgroup2
nodev	devtmpfs
nodev	devpts
nodev	overlay
	ext4
	vfat
	squashfs
	iso9660
	xfs
	btrfs
```
 
Пометка `nodev` означает, что файловой системе не нужно блочное устройство.

## 4. Создание файловой системы на блочном устройстве
 
Определяем доступные устройства:
 
```bash
lsblk -f
```
 
Форматирование раздела (все данные на нём будут уничтожены, поэтому перед запуском нужно проверить имя устройства):
 
```bash
sudo mkfs.ext4 /dev/sdb1
```
 
Эквивалентная форма: `sudo mkfs -t ext4 /dev/sdb1`. Для других систем: `mkfs.xfs`, `mkfs.btrfs`, `mkfs.vfat`.
 
Чтобы не рисковать реальным диском, в работе используем файл-образ, который ядро подключит как блочное устройство (loop):
 
```bash
truncate -s 100M /tmp/disk.img
sudo mkfs.ext4 -F /tmp/disk.img
```
 
```text
mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done
Creating filesystem with 25600 4k blocks and 25600 inodes
 
Allocating group tables: done
Writing inode tables: done
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done
```
 
Проверка результата:
 
```bash
blkid /tmp/disk.img
```
 
```text
/tmp/disk.img: UUID="3f6c9a1e-5b2d-4c7a-9e84-1d0f7a6b2c35" BLOCK_SIZE="4096" TYPE="ext4"
```

## 5. Монтирование
 
**Монтирование** — это подключение файловой системы к дереву каталогов. Каталог, в который она подключается, называется **точкой монтирования**. Содержимое файловой системы становится доступным по этому пути, а прежнее содержимое каталога на время монтирования скрывается.
 
Подключение созданного образа:
 
```bash
sudo mkdir -p /mnt/test
sudo mount -o loop /tmp/disk.img /mnt/test
```
 
Проверка:
 
```bash
df -hT /mnt/test
```
 
```text
Filesystem     Type  Size  Used Avail Use% Mounted on
/dev/loop12    ext4   89M   24K   82M   1% /mnt/test
```
 
Запись файла для проверки:
 
```bash
echo "hello" | sudo tee /mnt/test/file.txt
ls -l /mnt/test
```
 
```text
hello
total 20
-rw-r--r-- 1 root root     6 Oct 31 12:10 file.txt
drwx------ 2 root root 16384 Oct 31 12:08 lost+found
```
 
Отключение:
 
```bash
sudo umount /mnt/test
```
 
Общий вид команды: `mount -t <тип> -o <опции> <устройство> <каталог>`.
 
**Постоянное монтирование** при загрузке настраивается в `/etc/fstab`:
 
```text
UUID=3f6c9a1e-5b2d-4c7a-9e84-1d0f7a6b2c35  /mnt/test  ext4  defaults  0  2
```
 
Проверка записи без перезагрузки: `sudo mount -a`.

## 6. Файловые системы `procfs`, `sysfs`, `tmpfs`, `cifs`
 
### 6.1 Особенности
 
| ФС | Особенности |
|---|---|
| `procfs` | Монтируется в `/proc`. Интерфейс к информации о процессах (`/proc/<PID>/`) и о состоянии ядра (`cpuinfo`, `meminfo`, `mounts`, `uptime`). Файлы создаются ядром «на лету», на диске их нет. Часть параметров можно менять через `/proc/sys` (`sysctl`). |
| `sysfs` | Монтируется в `/sys`. Структурированное представление устройств, шин, драйверов и модулей ядра (`/sys/class`, `/sys/block`, `/sys/devices`). Один файл содержит одно значение. Через него настраиваются отдельные параметры устройств. |
| `tmpfs` | Файловая система в оперативной памяти (и при нехватке в swap). Очень быстрая, размер ограничивается опцией `size=`, содержимое пропадает при размонтировании и перезагрузке. Используется для `/run`, `/dev/shm`, `/tmp` (в некоторых дистрибутивах). |
| `cifs` | Клиент сетевого протокола SMB/CIFS для доступа к общим папкам Windows и Samba. Требует пакет `cifs-utils`, адрес сервера, имя пользователя и пароль. Права доступа определяются сервером и опциями монтирования (`uid=`, `gid=`, `file_mode=`). |
 
### 6.2 Точки монтирования в системе
 
```bash
findmnt -t proc,sysfs,tmpfs,cifs
```
 
```text
TARGET          SOURCE FSTYPE OPTIONS
/sys            sysfs  sysfs  rw,nosuid,nodev,noexec,relatime
/proc           proc   proc   rw,nosuid,nodev,noexec,relatime
/dev/shm        tmpfs  tmpfs  rw,nosuid,nodev,inode64
/run            tmpfs  tmpfs  rw,nosuid,nodev,noexec,relatime,size=401512k,mode=755,inode64
/run/lock       tmpfs  tmpfs  rw,nosuid,nodev,noexec,relatime,size=5120k,inode64
/run/user/1000  tmpfs  tmpfs  rw,nosuid,nodev,relatime,size=401508k,nr_inodes=100377,mode=700,uid=1000,gid=1000,inode64
```
 
Отдельная проверка для `cifs`:
 
```bash
findmnt -t cifs
echo "код возврата: $?"
```
 
```text
код возврата: 1
```
 
Вывод пустой, поэтому `cifs` в системе не используется: сетевые ресурсы SMB не подключены. Для справки, подключение выглядело бы так:
 
```bash
sudo apt install cifs-utils
sudo mount -t cifs //192.168.1.10/share /mnt/smb -o username=user,uid=1000
```

## 7. Информация о системе из виртуальных файловых систем
 
### 7.1 Информация о процессоре
 
```bash
cat /proc/cpuinfo
```
 
Фрагмент вывода:
 
```text
processor	: 0
vendor_id	: GenuineIntel
cpu family	: 6
model		: 140
model name	: 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz
stepping	: 1
cpu MHz		: 2419.200
cache size	: 8192 KB
cpu cores	: 2
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep ... avx avx2 ...
```
 
Краткая сводка:
 
```bash
grep -m1 'model name' /proc/cpuinfo
grep -c '^processor' /proc/cpuinfo
```
 
```text
model name	: 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz
2
```
 
Первая команда выводит модель процессора, вторая — число логических процессоров. Версию ядра показывает `cat /proc/version`.
 
### 7.2 Состояние памяти
 
```bash
cat /proc/meminfo
```
 
Фрагмент вывода:
 
```text
MemTotal:        4015120 kB
MemFree:         2201344 kB
MemAvailable:    3174568 kB
Buffers:           64212 kB
Cached:           987456 kB
SwapTotal:       2097148 kB
SwapFree:        2097148 kB
```
 
Основные поля: `MemTotal` — всего ОЗУ, `MemFree` — полностью свободная, `MemAvailable` — реально доступная приложениям с учётом кэша, `SwapTotal` и `SwapFree` — раздел подкачки.
 
```bash
grep -E 'MemTotal|MemAvailable|SwapTotal' /proc/meminfo
```
 
```text
MemTotal:        4015120 kB
MemAvailable:    3174568 kB
SwapTotal:       2097148 kB
```
 
### Информация из `/sys`
 
```bash
cat /sys/class/net/*/address
ls /sys/block
cat /sys/devices/system/cpu/online
```
 
```text
00:00:00:00:00:00
08:00:27:4b:3a:91
loop0  loop1  sda  sr0
0-1
```
 
Первая команда выводит MAC-адреса сетевых интерфейсов, вторая — список блочных устройств, третья — номера работающих процессорных ядер.
 
## 8. Список текущих точек монтирования
 
**Способ 1. `findmnt`** (дерево монтирования):
 
```bash
findmnt
```
 
```text
TARGET                  SOURCE      FSTYPE   OPTIONS
/                       /dev/sda2   ext4     rw,relatime
├─/sys                  sysfs       sysfs    rw,nosuid,nodev,noexec,relatime
├─/proc                 proc        proc     rw,nosuid,nodev,noexec,relatime
├─/dev                  udev        devtmpfs rw,nosuid,relatime,size=1962448k,nr_inodes=490612,mode=755
│ ├─/dev/pts            devpts      devpts   rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000
│ └─/dev/shm            tmpfs       tmpfs    rw,nosuid,nodev,inode64
├─/run                  tmpfs       tmpfs    rw,nosuid,nodev,noexec,relatime,size=401512k,mode=755
└─/boot/efi             /dev/sda1   vfat     rw,relatime,fmask=0077,dmask=0077
```
 
**Способ 2. Чтение `/proc/mounts`:**
 
```bash
cat /proc/mounts | head -n 8
```
 
```text
sysfs /sys sysfs rw,nosuid,nodev,noexec,relatime 0 0
proc /proc proc rw,nosuid,nodev,noexec,relatime 0 0
udev /dev devtmpfs rw,nosuid,relatime,size=1962448k,nr_inodes=490612,mode=755 0 0
devpts /dev/pts devpts rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000 0 0
tmpfs /run tmpfs rw,nosuid,nodev,noexec,relatime,size=401512k,mode=755 0 0
/dev/sda2 / ext4 rw,relatime 0 0
tmpfs /dev/shm tmpfs rw,nosuid,nodev 0 0
/dev/sda1 /boot/efi vfat rw,relatime,fmask=0077,dmask=0077 0 0
```
 
**Способ 3. Команда `mount`:**
 
```bash
mount | grep -E '^/dev/|^proc|^sysfs'
```
 
```text
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
/dev/sda2 on / type ext4 (rw,relatime)
/dev/sda1 on /boot/efi type vfat (rw,relatime,fmask=0077,dmask=0077)
```
 
**Способ 4. `df -hT`** (только с учётом занятого места и типа):
 
```bash
df -hT
```
 
```text
Filesystem     Type      Size  Used Avail Use% Mounted on
tmpfs          tmpfs     393M  1.4M  392M   1% /run
/dev/sda2      ext4       24G  9.8G   13G  44% /
tmpfs          tmpfs     2.0G     0  2.0G   0% /dev/shm
/dev/sda1      vfat      511M  6.1M  505M   2% /boot/efi
```
 
## 9. Определение типа файловой системы для точки монтирования
 
Выбираем точку монтирования `/`:
 
```bash
findmnt -n -o SOURCE,FSTYPE /
```
 
```text
/dev/sda2 ext4
```
 
Другие способы для того же каталога:
 
```bash
df -T /
stat -f -c 'Тип: %T' /
lsblk -f /dev/sda2
```
 
```text
Filesystem     Type 1K-blocks     Used Available Use% Mounted on
/dev/sda2      ext4  24505644 10276892  12961000  45% /
 
Тип: ext2/ext3
 
NAME FSTYPE FSVER LABEL UUID                                 FSAVAIL FSUSE% MOUNTPOINTS
sda2 ext4   1.0         8c1d5e7f-2a9b-4f3e-b6d4-7e0a1c9f5b23   12.4G    44% /
```
 
`stat -f` показывает `ext2/ext3` потому, что `ext4` использует то же магическое число суперблока, что и его предшественники. Для точного определения типа лучше пользоваться `findmnt`, `df -T` или `lsblk -f`.
 
Для виртуальной системы:
 
```bash
findmnt -n -o SOURCE,FSTYPE /proc
```
 
```text
proc proc
```
 
## 10. Различия между дисковой и виртуальными файловыми системами
 
| Признак | Дисковая (`ext4`, `XFS`) | Виртуальные (`/proc`, `/sys`) |
|---|---|---|
| Где хранятся данные | на блочном устройстве (диск, раздел, образ) | в памяти ядра, на диске нет |
| Источник содержимого | записывают пользователи и программы | генерирует ядро в момент чтения |
| Блочное устройство | нужно (`/dev/sda2`) | не нужно (в `/proc/filesystems` помечены `nodev`) |
| Сохранение после перезагрузки | да | нет, содержимое создаётся заново при загрузке |
| Размер файлов | реальный | обычно показывается `0` байт, но при чтении выдают данные |
| Назначение | долговременное хранение файлов | интерфейс между пользователем и ядром: состояние системы, процессов, устройств |
| Создание | `mkfs` | не создаются, ядро монтирует их само |
| Можно ли записывать | свободно, в пределах прав | только в отдельные параметры (`/proc/sys`, часть файлов `/sys`) |
 
Пример: файл `/proc/cpuinfo` имеет нулевой размер, но при чтении выводит данные о процессоре.
 
```bash
ls -l /proc/cpuinfo
wc -c /proc/cpuinfo
```
 
```text
-r--r--r-- 1 root root 0 Oct 31 12:30 /proc/cpuinfo
2112 /proc/cpuinfo
```
 
`/proc` ориентирован на процессы и общую информацию о системе, `/sys` — на устройства, драйверы и их настройки. Одно значение хранится в одном файле, а иерархия каталогов отражает структуру объектов ядра.
 
## 11. Возврат системы в исходное состояние
 
```bash
sudo umount /mnt/test 2>/dev/null
sudo rmdir /mnt/test
rm /tmp/disk.img
findmnt /mnt/test
```
 
Последняя команда ничего не выводит, значит тестовая файловая система отмонтирована.
