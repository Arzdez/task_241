# Григорьев Сергей 241 группа
# RAID-массивы

1) Что такое RAID и какие основные уровни RAID вы знаете?
2) Добавьте в виртуальную машину два дополнительных диска.
3) На двух дисках создайте RAID0-массив средствами Linux software RAID.
4) Проверьте состояние массива.
5) Создайте файловую систему ext4 уже на RAID-устройстве и примонтируйте её.
6) Запишите на массив тестовые файлы.
7) Удалите RAID0 и создайте RAID1.
8) Создайте файловую систему на RAID1 и снова проверьте чтение/запись.
9) В чём практическая разница между RAID0 и RAID1?
10) Имитируйте отказ одного диска RAID1:
    10.1) пометьте диск как неисправный;
    10.2) удалите его из массива;
    10.3) убедитесь, что данные остаются доступны.
11) Верните диск в массив или добавьте новый и наблюдайте процесс восстановления.
12) Какие файловые системы могут самостоятельно обеспечивать функции, похожие на RAID?
13) Можно ли создать RAID во время установки Linux?

## Критерий приёмки

В отчёте должны быть:
- вывод списка дисков, используемых для RAID;
- команды создания RAID0;
- вывод состояния RAID0;
- команда создания ext4 на RAID-устройстве;
- подтверждение монтирования и записи тестовых файлов;
- команды удаления RAID0 и создания RAID1;
- вывод состояния RAID1;
- команды имитации отказа одного диска;
- подтверждение доступности данных после отказа;
- вывод процесса восстановления массива после возврата или замены диска;
- краткое сравнение RAID0 и RAID1 по производительности, ёмкости и отказоустойчивости.

# Отчёт по практической работе: RAID-массивы

## 1. Что такое RAID и основные уровни

**RAID** (Redundant Array of Independent Disks — избыточный массив независимых дисков) — технология, которая объединяет несколько физических дисков в один логический том. Цели: повысить скорость работы, увеличить доступную ёмкость и/или обеспечить отказоустойчивость (сохранность данных при выходе из строя диска).

RAID бывает **аппаратный** (отдельный контроллер), **программный** (реализован средствами ОС — в Linux это подсистема `md` и утилита `mdadm`) и «фейковый» (BIOS/чипсет + драйвер). В работе используется программный RAID Linux.

Основные уровни:

| Уровень | Принцип | Мин. дисков | Полезная ёмкость | Отказоустойчивость |
|---------|---------|-------------|------------------|--------------------|
| RAID0 | чередование (striping) данных по дискам | 2 | сумма всех дисков | нет, отказ любого диска = потеря всех данных |
| RAID1 | зеркалирование (mirroring), на всех дисках одинаковые данные | 2 | размер одного (наименьшего) диска | переживает отказ всех дисков, кроме одного |
| RAID5 | чередование + распределённая чётность (parity) | 3 | (N − 1) дисков | 1 диск |
| RAID6 | чередование + двойная чётность | 4 | (N − 2) дисков | 2 диска |
| RAID10 (1+0) | чередование поверх зеркал | 4 | N / 2 | 1 диск в каждом зеркале (минимум 1) |

Также встречаются комбинированные уровни (RAID50, RAID60) и режим JBOD/linear (простое последовательное объединение дисков без избыточности).

> RAID защищает от отказа диска, но **не заменяет резервное копирование**: случайное удаление файла, вирус или ошибка ФС моментально отразятся на всех дисках массива.

## 2. Добавление двух виртуальных дисков

При выключенной виртуальной машине в VirtualBox (Настройки → Носители → контроллер SATA → «Добавить жёсткий диск») созданы и подключены два новых виртуальных диска по 2 ГБ каждый.

Список дисков, которые будут использоваться для RAID:

```bash
raddd1us@ubuntu:~$ lsblk
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0 63.9M  1 loop /snap/core20/2182
loop1    7:1    0   87M  1 loop /snap/lxd/29351
sda      8:0    0   25G  0 disk
├─sda1   8:1    0    1M  0 part
└─sda2   8:2    0   25G  0 part /
sdb      8:16   0    5G  0 disk
└─sdb1   8:17   0    5G  0 part /mnt/newdisk
sdc      8:32   0    2G  0 disk
sdd      8:48   0    2G  0 disk
sr0     11:0    1 1024M  0 rom
```

Новые диски определились как `/dev/sdc` и `/dev/sdd` (по 2 ГБ), разделов и файловых систем на них нет.

```bash
raddd1us@ubuntu:~$ sudo fdisk -l /dev/sdc /dev/sdd
Disk /dev/sdc: 2 GiB, 2147483648 bytes, 4194304 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

Disk /dev/sdd: 2 GiB, 2147483648 bytes, 4194304 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

Для RAID используются диски целиком, без создания разделов.

## 3. Создание RAID0

Установка утилиты `mdadm` (если не установлена):

```bash
raddd1us@ubuntu:~$ sudo apt install -y mdadm
raddd1us@ubuntu:~$ mdadm --version
mdadm - v4.2 - 2021-12-30
```

Создание массива RAID0 из двух дисков:

```bash
raddd1us@ubuntu:~$ sudo mdadm --create /dev/md0 --level=0 --raid-devices=2 /dev/sdc /dev/sdd
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
```

Параметры: `--create /dev/md0` — создать устройство массива, `--level=0` — уровень RAID0, `--raid-devices=2` — число дисков, далее перечислены сами диски.

## 4. Состояние RAID0

```bash
raddd1us@ubuntu:~$ cat /proc/mdstat
Personalities : [raid0]
md0 : active raid0 sdd[1] sdc[0]
      4190208 blocks super 1.2 512k chunks

unused devices: <none>
```

```bash
raddd1us@ubuntu:~$ sudo mdadm --detail /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Sun Nov  8 14:12:03 2026
        Raid Level : raid0
        Array Size : 4190208 (4.00 GiB 4.29 GB)
      Raid Devices : 2
     Total Devices : 2
       Persistence : Superblock is persistent

       Update Time : Sun Nov  8 14:12:03 2026
             State : clean
    Active Devices : 2
   Working Devices : 2
    Failed Devices : 0
     Spare Devices : 0

        Chunk Size : 512K

Consistency Policy : none

              Name : ubuntu:0  (local to host ubuntu)
              UUID : 5d3a9c1e:7b2f4e08:a1c6d93b:0e8f2a47
            Events : 0

    Number   Major   Minor   RaidDevice State
       0       8       32        0      active sync   /dev/sdc
       1       8       48        1      active sync   /dev/sdd
```

Массив `md0` активен (`State: clean`), уровень `raid0`, размер около 4 ГБ — сумма двух дисков по 2 ГБ.

## 5. Файловая система ext4 на RAID-устройстве и монтирование

Файловая система создаётся не на отдельных дисках, а на устройстве массива `/dev/md0`:

```bash
raddd1us@ubuntu:~$ sudo mkfs.ext4 /dev/md0
mke2fs 1.46.5 (30-Dec-2021)
Creating filesystem with 1047552 4k blocks and 262144 inodes
Filesystem UUID: 2c7e91a4-6f3b-4d08-b5a2-e90d14c8f736
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done
```

Монтирование:

```bash
raddd1us@ubuntu:~$ sudo mkdir /mnt/raid
raddd1us@ubuntu:~$ sudo mount /dev/md0 /mnt/raid
```

Подтверждение успешного монтирования:

```bash
raddd1us@ubuntu:~$ findmnt /mnt/raid
TARGET    SOURCE   FSTYPE OPTIONS
/mnt/raid /dev/md0 ext4   rw,relatime,stripe=256

raddd1us@ubuntu:~$ df -h /mnt/raid
Filesystem      Size  Used Avail Use% Mounted on
/dev/md0        3.9G   24K  3.7G   1% /mnt/raid

raddd1us@ubuntu:~$ lsblk /dev/sdc /dev/sdd
NAME MAJ:MIN RM SIZE RO TYPE  MOUNTPOINTS
sdc    8:32   0   2G  0 disk
└─md0  9:0    0   4G  0 raid0 /mnt/raid
sdd    8:48   0   2G  0 disk
└─md0  9:0    0   4G  0 raid0 /mnt/raid
```

(Параметр `stripe=256` ext4 подобрал автоматически под размер чередования RAID0.)

## 6. Тестовые файлы на RAID0

```bash
raddd1us@ubuntu:~$ echo "RAID0 test file 1" | sudo tee /mnt/raid/raid0_file1.txt
RAID0 test file 1
raddd1us@ubuntu:~$ echo "RAID0 test file 2" | sudo tee /mnt/raid/raid0_file2.txt
RAID0 test file 2
raddd1us@ubuntu:~$ sudo dd if=/dev/urandom of=/mnt/raid/raid0_data.bin bs=1M count=50
50+0 records in
50+0 records out
52428800 bytes (52 MB, 50 MiB) copied, 0.31 s, 169 MB/s
raddd1us@ubuntu:~$ ls -l /mnt/raid
total 51224
drwx------ 2 root root    16384 Nov  8 14:15 lost+found
-rw-r--r-- 1 root root 52428800 Nov  8 14:17 raid0_data.bin
-rw-r--r-- 1 root root       18 Nov  8 14:16 raid0_file1.txt
-rw-r--r-- 1 root root       18 Nov  8 14:16 raid0_file2.txt
```

Проверка чтения:

```bash
raddd1us@ubuntu:~$ cat /mnt/raid/raid0_file1.txt
RAID0 test file 1
```

## 7. Удаление RAID0 и создание RAID1

Удаление RAID0: размонтировать файловую систему, остановить массив и стереть служебные метаданные (суперблоки) `md` с дисков.

```bash
raddd1us@ubuntu:~$ sudo umount /mnt/raid
raddd1us@ubuntu:~$ sudo mdadm --stop /dev/md0
mdadm: stopped /dev/md0
raddd1us@ubuntu:~$ sudo mdadm --zero-superblock /dev/sdc /dev/sdd
raddd1us@ubuntu:~$ cat /proc/mdstat
Personalities : [raid0]
unused devices: <none>
```

Массивов в системе больше нет, данные RAID0 уничтожены.

Создание RAID1 на тех же двух дисках:

```bash
raddd1us@ubuntu:~$ sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdc /dev/sdd
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot loader understands md/v1.x metadata, or use
    --metadata=0.90
Continue creating array? y
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
```

## 8. Файловая система на RAID1 и проверка чтения/записи

Состояние RAID1. Сразу после создания идёт начальная синхронизация зеркал (`resync`):

```bash
raddd1us@ubuntu:~$ cat /proc/mdstat
Personalities : [raid0] [raid1]
md0 : active raid1 sdd[1] sdc[0]
      2094080 blocks super 1.2 [2/2] [UU]
      [=====>...............]  resync = 28.4% (595968/2094080) finish=0.2min speed=99328K/sec

unused devices: <none>
```

После завершения синхронизации:

```bash
raddd1us@ubuntu:~$ cat /proc/mdstat
Personalities : [raid0] [raid1]
md0 : active raid1 sdd[1] sdc[0]
      2094080 blocks super 1.2 [2/2] [UU]

unused devices: <none>

raddd1us@ubuntu:~$ sudo mdadm --detail /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Sun Nov  8 14:21:40 2026
        Raid Level : raid1
        Array Size : 2094080 (2045.00 MiB 2144.34 MB)
     Used Dev Size : 2094080 (2045.00 MiB 2144.34 MB)
      Raid Devices : 2
     Total Devices : 2
       Persistence : Superblock is persistent

       Update Time : Sun Nov  8 14:22:31 2026
             State : clean
    Active Devices : 2
   Working Devices : 2
    Failed Devices : 0
     Spare Devices : 0

Consistency Policy : resync

              Name : ubuntu:0  (local to host ubuntu)
              UUID : a94e1d70:3c58b2f6:8e07d1a3:62b9c4f0
            Events : 17

    Number   Major   Minor   RaidDevice State
       0       8       32        0      active sync   /dev/sdc
       1       8       48        1      active sync   /dev/sdd
```

Массив `raid1` в состоянии `clean`, `[UU]` — оба диска работают. Полезный размер около 2 ГБ, то есть равен одному диску.

Создание файловой системы и монтирование:

```bash
raddd1us@ubuntu:~$ sudo mkfs.ext4 /dev/md0
mke2fs 1.46.5 (30-Dec-2021)
Creating filesystem with 523520 4k blocks and 131072 inodes
Filesystem UUID: 7d03b5e8-91a4-4c2f-8e6a-f1b0c3d29a58
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912

Allocating group tables: done
Writing inode tables: done
Creating journal (16384 blocks): done
Writing superblocks and filesystem accounting information: done

raddd1us@ubuntu:~$ sudo mount /dev/md0 /mnt/raid
raddd1us@ubuntu:~$ findmnt /mnt/raid
TARGET    SOURCE   FSTYPE OPTIONS
/mnt/raid /dev/md0 ext4   rw,relatime
```

Запись и чтение тестовых файлов (контрольная сумма большого файла сохраняется в домашний каталог, вне массива):

```bash
raddd1us@ubuntu:~$ echo "RAID1 test file 1" | sudo tee /mnt/raid/raid1_file1.txt
RAID1 test file 1
raddd1us@ubuntu:~$ echo "RAID1 test file 2" | sudo tee /mnt/raid/raid1_file2.txt
RAID1 test file 2
raddd1us@ubuntu:~$ sudo dd if=/dev/urandom of=/mnt/raid/raid1_data.bin bs=1M count=20
20+0 records in
20+0 records out
20971520 bytes (21 MB, 20 MiB) copied, 0.09 s, 231 MB/s
raddd1us@ubuntu:~$ sha256sum /mnt/raid/raid1_data.bin | tee ~/raid1_data.sha256
9f2c4e7a1b8d3056ce41fa97d0b25e638a1c74f9e0d2b5a8736c1f4e9a0d7b32  /mnt/raid/raid1_data.bin
raddd1us@ubuntu:~$ ls -l /mnt/raid
total 20504
drwx------ 2 root root    16384 Nov  8 14:24 lost+found
-rw-r--r-- 1 root root 20971520 Nov  8 14:26 raid1_data.bin
-rw-r--r-- 1 root root       18 Nov  8 14:25 raid1_file1.txt
-rw-r--r-- 1 root root       18 Nov  8 14:25 raid1_file2.txt
raddd1us@ubuntu:~$ cat /mnt/raid/raid1_file2.txt
RAID1 test file 2
```

Чтение и запись на RAID1 работают.

## 9. Практическая разница между RAID0 и RAID1

| Критерий | RAID0 (striping) | RAID1 (mirroring) |
|----------|------------------|-------------------|
| Производительность | Максимальная: данные чередуются по дискам, чтение и запись идут параллельно (до ×N). | Чтение может ускоряться (запросы распределяются между зеркалами), запись примерно как на одном диске (данные пишутся на все диски). |
| Ёмкость | Сумма дисков (в работе: 2 × 2 ГБ ≈ 4 ГБ). Потерь нет. | Размер одного диска (в работе ≈ 2 ГБ), остальное уходит на копии; эффективность 50 % для двух дисков. |
| Отказоустойчивость | Отсутствует. Отказ любого диска приводит к потере всех данных массива, риск выше, чем у одиночного диска. | Высокая. Данные сохраняются, пока жив хотя бы один диск. |

Практически: RAID0 подходит для временных данных, кэшей и задач, где важна скорость, а потеря данных не критична. RAID1 — для данных, которые нельзя потерять (системный диск, небольшие важные хранилища).

## 10. Имитация отказа одного диска RAID1

Перед отказом массив в состоянии `[UU]` (см. раздел 8).

### 10.1. Пометка диска как неисправного

```bash
raddd1us@ubuntu:~$ sudo mdadm /dev/md0 --fail /dev/sdd
mdadm: set /dev/sdd faulty in /dev/md0

raddd1us@ubuntu:~$ cat /proc/mdstat
Personalities : [raid0] [raid1]
md0 : active raid1 sdd[1](F) sdc[0]
      2094080 blocks super 1.2 [2/1] [U_]

unused devices: <none>

raddd1us@ubuntu:~$ sudo mdadm --detail /dev/md0 | tail -n 22
       Update Time : Sun Nov  8 14:31:12 2026
             State : clean, degraded
    Active Devices : 1
   Working Devices : 1
    Failed Devices : 1
     Spare Devices : 0

Consistency Policy : resync

              Name : ubuntu:0  (local to host ubuntu)
              UUID : a94e1d70:3c58b2f6:8e07d1a3:62b9c4f0
            Events : 19

    Number   Major   Minor   RaidDevice State
       0       8       32        0      active sync   /dev/sdc
       1       8       48        -      faulty   /dev/sdd
```

Диск `/dev/sdd` помечен `(F)` (faulty), массив перешёл в состояние `clean, degraded`, `[U_]` — работает один диск из двух.

### 10.2. Удаление диска из массива

```bash
raddd1us@ubuntu:~$ sudo mdadm /dev/md0 --remove /dev/sdd
mdadm: hot removed /dev/sdd from /dev/md0

raddd1us@ubuntu:~$ cat /proc/mdstat
Personalities : [raid0] [raid1]
md0 : active raid1 sdc[0]
      2094080 blocks super 1.2 [2/1] [U_]

unused devices: <none>

raddd1us@ubuntu:~$ sudo mdadm --detail /dev/md0 | tail -n 4
    Number   Major   Minor   RaidDevice State
       0       8       32        0      active sync   /dev/sdc
       -       0        0        1      removed
```

### 10.3. Проверка доступности данных

Файловая система остаётся смонтированной, файлы читаются, целостность подтверждается контрольной суммой, запись новых данных тоже работает:

```bash
raddd1us@ubuntu:~$ ls -l /mnt/raid
total 20504
drwx------ 2 root root    16384 Nov  8 14:24 lost+found
-rw-r--r-- 1 root root 20971520 Nov  8 14:26 raid1_data.bin
-rw-r--r-- 1 root root       18 Nov  8 14:25 raid1_file1.txt
-rw-r--r-- 1 root root       18 Nov  8 14:25 raid1_file2.txt

raddd1us@ubuntu:~$ cat /mnt/raid/raid1_file1.txt
RAID1 test file 1

raddd1us@ubuntu:~$ sha256sum -c ~/raid1_data.sha256
/mnt/raid/raid1_data.bin: OK

raddd1us@ubuntu:~$ echo "written in degraded mode" | sudo tee /mnt/raid/raid1_degraded.txt
written in degraded mode
raddd1us@ubuntu:~$ cat /mnt/raid/raid1_degraded.txt
written in degraded mode
```

Данные полностью доступны, несмотря на «отказ» одного диска: в этом и состоит смысл RAID1.

## 11. Возврат диска в массив и восстановление

Диск `/dev/sdd` возвращается в массив:

```bash
raddd1us@ubuntu:~$ sudo mdadm /dev/md0 --add /dev/sdd
mdadm: added /dev/sdd
```

Наблюдение за процессом восстановления (rebuild/recovery):

```bash
raddd1us@ubuntu:~$ cat /proc/mdstat
Personalities : [raid0] [raid1]
md0 : active raid1 sdd[2] sdc[0]
      2094080 blocks super 1.2 [2/1] [U_]
      [===========>.........]  recovery = 56.3% (1179648/2094080) finish=0.1min speed=85333K/sec

unused devices: <none>

raddd1us@ubuntu:~$ sudo mdadm --detail /dev/md0 | tail -n 20
       Update Time : Sun Nov  8 14:36:25 2026
             State : clean, degraded, recovering
    Active Devices : 1
   Working Devices : 2
    Failed Devices : 0
     Spare Devices : 1

Consistency Policy : resync

    Rebuild Status : 56% complete

              Name : ubuntu:0  (local to host ubuntu)
              UUID : a94e1d70:3c58b2f6:8e07d1a3:62b9c4f0
            Events : 38

    Number   Major   Minor   RaidDevice State
       0       8       32        0      active sync   /dev/sdc
       2       8       48        1      spare rebuilding   /dev/sdd
```

Во время восстановления диск `/dev/sdd` имеет статус `spare rebuilding`, данные копируются на него с `/dev/sdc`. Динамически следить за процессом можно командой `watch cat /proc/mdstat`.

После завершения восстановления:

```bash
raddd1us@ubuntu:~$ cat /proc/mdstat
Personalities : [raid0] [raid1]
md0 : active raid1 sdd[2] sdc[0]
      2094080 blocks super 1.2 [2/2] [UU]

unused devices: <none>

raddd1us@ubuntu:~$ sudo mdadm --detail /dev/md0 | tail -n 18
             State : clean
    Active Devices : 2
   Working Devices : 2
    Failed Devices : 0
     Spare Devices : 0

Consistency Policy : resync

              Name : ubuntu:0  (local to host ubuntu)
              UUID : a94e1d70:3c58b2f6:8e07d1a3:62b9c4f0
            Events : 56

    Number   Major   Minor   RaidDevice State
       0       8       32        0      active sync   /dev/sdc
       2       8       48        1      active sync   /dev/sdd

raddd1us@ubuntu:~$ ls /mnt/raid
lost+found  raid1_data.bin  raid1_degraded.txt  raid1_file1.txt  raid1_file2.txt
raddd1us@ubuntu:~$ sha256sum -c ~/raid1_data.sha256
/mnt/raid/raid1_data.bin: OK
```

Массив снова в состоянии `clean`, `[UU]` — оба диска синхронизированы. Файл `raid1_degraded.txt`, записанный в деградированном режиме, также попал на восстановленный диск.

## 12. Файловые системы с RAID-подобными функциями

Классические ФС (ext4, XFS, NTFS, FAT) работают на одном блочном устройстве и RAID самостоятельно не обеспечивают — для этого нужен отдельный слой (`md`, LVM, аппаратный контроллер). Но есть ФС, которые сами управляют несколькими дисками:

- **Btrfs** — встроенные профили для данных и метаданных: `single`, `dup`, `raid0`, `raid1`, `raid1c3`, `raid1c4`, `raid10`, а также `raid5/raid6` (последние до сих пор считаются проблемными из-за уязвимости write hole и не рекомендуются для важных данных). Есть контрольные суммы данных, снимки и самовосстановление (при наличии избыточности).
- **ZFS** — объединяет файловую систему и менеджер томов: пулы из `mirror` (аналог RAID1), `raidz1`, `raidz2`, `raidz3` (аналоги RAID5/6 и тройной чётности), чередование нескольких vdev, контрольные суммы всех блоков и автоматическое лечение повреждённых данных (self-healing).
- **Распределённые ФС** (Ceph, GlusterFS и др.) — хранят копии данных (репликацию) или кодирование со стиранием (erasure coding) на разных серверах.
- **ReFS** со Storage Spaces в Windows — зеркалирование и чётность на уровне системы хранения.

Отдельно стоит упомянуть **LVM** — это не файловая система, а менеджер логических томов, но он тоже умеет зеркалирование и чередование (`lvcreate --type raid1` и др.).

## 13. Можно ли создать RAID во время установки Linux?

Да. Большинство установщиков Linux позволяют создать программный RAID на этапе разметки диска:

- **Ubuntu Server** (установщик subiquity): в экране Storage configuration выбрать «Custom storage layout», отметить диски (кнопка «Use As Device» / «Create software RAID (md)»), выбрать уровень RAID и затем создать на нём разделы или LVM.
- **Debian**: в разметке — «Руководство — использовать весь диск» не подходит, нужен ручной режим: «Настроить программный RAID», затем создать разделы типа «физический том для RAID» и собрать из них md-устройство.
- **RHEL, CentOS, Fedora** (Anaconda): в «Custom» разметке при добавлении точки монтирования можно выбрать тип устройства RAID и его уровень.

Нюансы: для загрузки обычно оставляют `/boot` (и раздел EFI) на RAID1 с метаданными старого формата (`0.90` или `1.0`), которые лежат в конце диска и позволяют загрузчику видеть раздел как обычный; в ряде случаев раздел EFI дублируют вручную на каждый диск. Графический установщик Ubuntu Desktop возможностей создания md-RAID, как правило, не даёт, там нужен Server-установщик или предварительная подготовка в live-режиме.



















