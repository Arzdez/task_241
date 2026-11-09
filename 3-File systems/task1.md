# Файловые системы

## 1. Какие файловые системы вы знаете?

Существуют следующие типы файловых систем:

- **ext2, ext3, ext4** - файловые системы, широко используемые в Linux. `ext4` поддерживает журналирование и подходит для хранения данных на дисках.
- **XFS** - журналируемая файловая система, рассчитанная в том числе на работу с большими объёмами данных.
- **Btrfs** - файловая система с поддержкой снимков, контрольных сумм и других дополнительных возможностей.
- **FAT32, exFAT** - файловые системы, часто используемые на USB-накопителях и картах памяти.
- **NTFS** - файловая система, используемая преимущественно в Windows.
- **ISO 9660** - файловая система для оптических дисков.
- **SquashFS** - сжатая файловая система, предназначенная преимущественно для чтения.
- **procfs (`proc`)** - виртуальная файловая система, предоставляющая информацию о процессах и ядре.
- **sysfs (`sysfs`)** - виртуальная файловая система для получения информации об устройствах и ядре.
- **tmpfs** - файловая система для временных данных, обычно размещаемых в оперативной памяти и при необходимости использующих swap.
- **CIFS** - файловая система для доступа к сетевым ресурсам через протокол SMB/CIFS.
- **NFS** - файловая система для доступа к каталогам по сети.

## 2. Как можно классифицировать файловые системы? В чём основные отличия?

Файловые системы можно классифицировать по нескольким признакам.

1. **Дисковые файловые системы.** Хранят данные на постоянных носителях. Примеры: `ext4`, `XFS`, `Btrfs`, `NTFS`.
2. **Виртуальные файловые системы.** Предоставляют информацию о состоянии ядра, процессов и устройств, а не обычные пользовательские файлы на диске. Примеры: `procfs` и `sysfs`.
3. **Файловые системы в оперативной памяти.** Используются для временных данных. Пример: `tmpfs`.
4. **Сетевые файловые системы.** Позволяют обращаться к данным на удалённых компьютерах. Примеры: `NFS` и `CIFS`.
5. **Файловые системы для носителей и образов.** Примеры: `ISO 9660` для оптических дисков и `SquashFS` для сжатых образов, часто доступных только для чтения.

Основные отличия заключаются в способе хранения данных, назначении, поддержке записи, наличии журналирования, доступе через сеть и возможности предоставлять динамическую информацию о системе.

## 3. Какие файловые системы обычно используются в Linux?

В Linux часто используются `ext4`, `XFS` и `Btrfs` для хранения обычных файлов на дисках. Также поддерживаются другие дисковые файловые системы, например `ext3`, `FAT32`, `exFAT` и `NTFS`.

Кроме того, Linux использует специальные файловые системы:

- `proc` - для информации о процессах и ядре;
- `sysfs` - для информации об устройствах и подсистемах ядра;
- `tmpfs` - для временных файлов;
- `devtmpfs` - для файлов устройств;
- `cgroup2` - для организации и контроля ресурсов групп процессов.

Команда `cat /proc/filesystems` показала поддержку `ext3`, `ext4`, `squashfs`, `iso9660`, `proc`, `sysfs`, `tmpfs` и ряда других файловых систем.

## 4. Как создать файловую систему на блочном устройстве?

Для создания файловой системы используют команду `mkfs` или специализированные команды, например `mkfs.ext4`.

Пример создания файловой системы `ext4` на выбранном блочном устройстве:

```bash
sudo mkfs.ext4 /dev/sdX1
```

Здесь `/dev/sdX1` - условное имя раздела. Перед выполнением необходимо определить правильное устройство с помощью `lsblk` и убедиться, что оно не содержит нужных данных: создание файловой системы может уничтожить существующую информацию.

## 5. Что такое монтирование и как подключить файловую систему в систему?

**Монтирование** - это подключение файловой системы к определённому каталогу, называемому точкой монтирования. После подключения содержимое файловой системы становится доступно через этот каталог.

Например, для подключения существующей файловой системы:

```bash
sudo mkdir -p /mnt/data
sudo mount /dev/sdX1 /mnt/data
```

В этом примере `/dev/sdX1` - условное блочное устройство, а `/mnt/data` - точка монтирования.

Посмотреть подключённые файловые системы можно командами `findmnt` или `mount`. Для отключения файловой системы используется `umount`, например:

```bash
sudo umount /mnt/data
```

## 6. Рассмотрите `procfs`, `sysfs`, `tmpfs` и `cifs`

### 6.1. В чём особенности каждой из них?

**procfs (`proc`)**

Виртуальная файловая система, обычно смонтированная в `/proc`. Предоставляет информацию о процессах, процессоре, памяти и параметрах ядра. Например, `/proc/cpuinfo` содержит сведения о процессоре, а `/proc/meminfo` - сведения о памяти.

**sysfs (`sysfs`)**

Виртуальная файловая система, обычно смонтированная в `/sys`. Отображает информацию об устройствах, драйверах и подсистемах ядра Linux.

**tmpfs**

Файловая система для временных данных. Обычно использует оперативную память и может использовать swap при соответствующей настройке. У нас она смонтирована, например, в `/tmp`, `/run` и `/dev/shm`. Содержимое таких каталогов обычно не предназначено для постоянного хранения после перезагрузки.

**CIFS**

Сетевая файловая система, позволяющая обращаться к удалённым общим каталогам по протоколу SMB/CIFS. Она может использоваться для подключения сетевых папок, например с серверов или компьютеров Windows. В проверенных выводах текущих точек монтирования файловая система `cifs` не обнаружена.

### 6.2. Найти каталоги, куда примонтированы эти файловые системы

Для проверки использована команда:

```bash
findmnt
```

Основные результаты:

```console
TARGET                                        SOURCE     FSTYPE
/                                             /dev/sda1  ext4
├─/proc                                       proc       proc
├─/dev                                        udevfs     devtmpfs
│ └─/dev/shm                                  tmpfs      tmpfs
├─/run                                        runfs      tmpfs
├─/sys                                        sysfs      sysfs
└─/tmp                                        tmpfs      tmpfs
```

Также команда `findmnt` показала дополнительные точки монтирования `tmpfs` внутри `/run`, например `/run/user/1000` и каталоги учётных данных служб.

По результатам проверки:

- `procfs` смонтирована в `/proc`;
- `sysfs` смонтирована в `/sys`;
- `tmpfs` используется в `/tmp`, `/run`, `/dev/shm` и других каталогах;
- смонтированная `cifs` в текущем выводе отсутствует.

## 7. Как получить информацию о системе, используя содержимое виртуальных файловых систем?

Информацию о системе можно получать, читая специальные файлы в виртуальной файловой системе `/proc`. Эти файлы формируются ядром и позволяют изучать состояние системы без обращения к обычным файлам с данными на диске.

### 7.1. Вывести информацию о процессоре

Использована команда:

```bash
cat /proc/cpuinfo
```

Результат:

```console
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 44
model name      : Intel(R) Xeon(R) CPU           X5675  @ 3.07GHz
stepping        : 2
microcode       : 0x1f
cpu MHz         : 3066.774
cache size      : 16384 KB
physical id     : 0
siblings        : 1
core id         : 0
cpu cores       : 1
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 11
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon rep_good nopl xtopology tsc_known_freq pni pclmulqdq vmx ssse3 cx16 pdcm pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes hypervisor lahf_lm cpuid_fault pti ssbd ibrs ibpb stibp tpr_shadow flexpriority ept vpid tsc_adjust arat vnmi umip flush_l1d arch_capabilities
vmx flags       : vnmi preemption_timer invvpid ept_x_only ept_1gb flexpriority tsc_offset vtpr mtf vapic ept vpid unrestricted_guest shadow_vmcs
bugs            : cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf mds swapgs mmio_unknown bhi ibpb_no_ret
bogomips        : 6133.54
clflush size    : 64
cache_alignment : 64
address sizes   : 40 bits physical, 48 bits virtual
power management:
```

Согласно выводу, виртуальная машина видит процессор Intel Xeon X5675 с частотой около 3,07 ГГц. В выводе присутствует флаг `hypervisor`, указывающий на работу в виртуализированной среде.

### 7.2. Вывести информацию о состоянии памяти

Использована команда:

```bash
cat /proc/meminfo
```

Основные строки результата:

```console
MemTotal:        1502576 kB
MemFree:          209276 kB
MemAvailable:    1222784 kB
Buffers:           52936 kB
Cached:          1064344 kB
SwapCached:            0 kB
Active:           658740 kB
Inactive:         519544 kB
SwapTotal:             0 kB
SwapFree:              0 kB
```

По результатам:

- `MemTotal` - общий объём доступной системе оперативной памяти: 1 502 576 kB;
- `MemFree` - свободная память: 209 276 kB;
- `MemAvailable` - оценка памяти, которую можно использовать без активного обращения к swap: 1 222 784 kB;
- `SwapTotal` - общий объём swap: 0 kB;
- `SwapFree` - свободный swap: 0 kB.

Таким образом, системе предоставлено около 1,43 GiB оперативной памяти, а активный swap в выводе отсутствует.

## 8. Вывести список всех текущих точек монтирования минимум двумя разными способами

### Способ 1. Команда `findmnt`

```bash
findmnt
```

Основная часть результата:

```console
TARGET                                        SOURCE     FSTYPE     OPTIONS
/                                             /dev/sda1  ext4       rw,relatime,mb_optimize_scan=0
├─/proc                                       proc       proc       rw,nosuid,noexec,relatime,gid=19
│ └─/proc/sys/fs/binfmt_misc                  systemd-1  autofs     rw,relatime,fd=36,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=2550
├─/dev                                        udevfs     devtmpfs   rw,relatime,size=5120k,nr_inodes=184459,mode=755,inode64
│ ├─/dev/shm                                  tmpfs      tmpfs      rw,nosuid,nodev,inode64
│ ├─/dev/pts                                  devpts     devpts     rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=666
│ ├─/dev/mqueue                               mqueue     mqueue    rw,nosuid,nodev,noexec,relatime
│ └─/dev/hugepages                            hugetlbfs  hugetlbfs rw,nosuid,nodev,relatime,pagesize=2M
├─/run                                        runfs      tmpfs      rw,relatime,mode=755,inode64
├─/sys                                        sysfs      sysfs      rw,nosuid,nodev,noexec,relatime
├─/sys/fs/cgroup                              cgroup2    cgroup2    rw,nosuid,nodev,noexec,relatime
└─/tmp                                        tmpfs      tmpfs      rw,nosuid,relatime,inode64
```

### Способ 2. Команда `mount`

```bash
mount
```

Основная часть результата:

```console
udevfs on /dev type devtmpfs (rw,relatime,size=5120k,nr_inodes=184459,mode=755,inode64)
runfs on /run type tmpfs (rw,relatime,mode=755,inode64)
/dev/sda1 on / type ext4 (rw,relatime,mb_optimize_scan=0)
proc on /proc type proc (rw,nosuid,noexec,relatime,gid=19)
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
securityfs on /sys/kernel/security type securityfs (rw,nosuid,nodev,noexec,relatime)
tmpfs on /dev/shm type tmpfs (rw,nosuid,nodev,inode64)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=666)
cgroup2 on /sys/fs/cgroup type cgroup2 (rw,nosuid,nodev,noexec,relatime,nsdelegate,memory_recursiveprot)
pstore on /sys/fs/pstore type pstore (rw,nosuid,nodev,noexec,relatime)
bpf on /sys/fs/bpf type bpf (rw,nosuid,nodev,noexec,relatime,mode=700)
systemd-1 on /proc/sys/fs/binfmt_misc type autofs (rw,relatime,fd=36,pgrp=1,timeout=0,minproto=5,maxproto=5,direct,pipe_ino=2550)
mqueue on /dev/mqueue type mqueue (rw,nosuid,nodev,noexec,relatime)
hugetlbfs on /dev/hugepages type hugetlbfs (rw,nosuid,nodev,relatime,pagesize=2M)
debugfs on /sys/kernel/debug type debugfs (rw,nosuid,nodev,noexec,relatime)
tracefs on /sys/kernel/tracing type tracefs (rw,nosuid,nodev,noexec,relatime)
tmpfs on /tmp type tmpfs (rw,nosuid,relatime,inode64)
```

Команды `findmnt` и `mount` выводят текущие точки монтирования, но представляют данные в разных форматах.

### Пример определения типа файловой системы для выбранной точки монтирования

Для определения типа файловой системы корневого каталога выполнена команда:

```bash
findmnt -T /
```

Результат:

```console
TARGET
  SOURCE    FSTYPE OPTIONS
/ /dev/sda1 ext4   rw,relatime,mb_optimize_scan=0
```

Корневой каталог `/` расположен на устройстве `/dev/sda1`, тип файловой системы - `ext4`.

### Отличие обычной дисковой файловой системы от виртуальных `/proc` и `/sys`

Обычная дисковая файловая система, например `ext4`, хранит пользовательские данные на постоянном носителе и позволяет сохранять файлы между перезагрузками.

`/proc` и `/sys` - точки монтирования виртуальных файловых систем. Их содержимое формируется ядром Linux и отражает состояние процессов, устройств и системных подсистем. Эти каталоги не предназначены для обычного долговременного хранения пользовательских данных.


