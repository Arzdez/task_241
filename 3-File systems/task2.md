# Григорьев Сергей 241 группа
# Структура каталогов

1) Какая структура каталогов используется в Linux? Выведите список объектов в корне системы.
2) Где обычно хранятся домашние каталоги обычных пользователей?
3) Где находится домашний каталог суперпользователя?
4) Где обычно хранятся основные системные конфигурационные файлы?
5) Объясните назначение каталогов:
   - `/bin`
   - `/sbin`
   - `/usr/bin`
   - `/usr/sbin`
   - `/etc`
   - `/var`
   - `/tmp`
   - `/home`
   - `/root`
   - `/proc`
   - `/sys`
6) Проверьте, являются ли `/bin` и `/sbin` реальными каталогами или символическими ссылками в вашей системе.
7) Найдите, где физически расположен исполняемый файл одной из команд, которой вы пользовались ранее.

## Критерий приёмки

В отчёте должны быть:
- список объектов в корне `/`;
- краткое назначение каталогов `/bin`, `/sbin`, `/usr/bin`, `/usr/sbin`, `/etc`, `/var`, `/tmp`, `/home`, `/root`, `/proc`, `/sys`;
- вывод, показывающий, являются ли `/bin` и `/sbin` каталогами или символическими ссылками;
- команда и результат поиска исполняемого файла выбранной команды;
- краткое пояснение различий между обычным каталогом, символической ссылкой и виртуальной файловой системой.

# Отчёт по практической работе: Структура каталогов

## 1. Структура каталогов в Linux и список объектов в корне

В Linux используется **единое иерархическое дерево каталогов**. У него один корень `/`, а все остальные файлы, каталоги, устройства и подключённые диски располагаются внутри него. Отдельных «дисков» вроде `C:` и `D:` нет: дополнительные разделы и носители монтируются в каталоги дерева (например, в `/mnt` или `/media`). Расположение основных каталогов описывает стандарт **FHS** (Filesystem Hierarchy Standard).

Список объектов в корне системы:

```bash
raddd1us@ubuntu:~$ ls /
```

```text
bin                boot  dev  home  lib64  lost+found  mnt  proc  run   snap  sys  usr
bin.usr-is-merged  cdrom etc  lib   lib.usr-is-merged  media  opt  root  sbin  srv   tmp  var
```

Подробный вывод с типами и правами:

```bash
raddd1us@ubuntu:~$ ls -l /
```

```text
total 76
lrwxrwxrwx   1 root root     7 Apr 22  2024 bin -> usr/bin
drwxr-xr-x   2 root root  4096 Feb 26  2024 bin.usr-is-merged
drwxr-xr-x   4 root root  4096 Nov  5 11:20 boot
drwxr-xr-x  19 root root  4600 Nov  5 11:18 dev
drwxr-xr-x 120 root root 12288 Nov  5 11:20 etc
drwxr-xr-x   3 root root  4096 Oct 20 10:02 home
lrwxrwxrwx   1 root root     7 Apr 22  2024 lib -> usr/lib
lrwxrwxrwx   1 root root     9 Apr 22  2024 lib64 -> usr/lib64
drwx------   2 root root 16384 Oct 20 09:55 lost+found
drwxr-xr-x   2 root root  4096 Apr 22  2024 media
drwxr-xr-x   2 root root  4096 Apr 22  2024 mnt
drwxr-xr-x   2 root root  4096 Apr 22  2024 opt
dr-xr-xr-x 310 root root     0 Nov  5 11:18 proc
drwx------   5 root root  4096 Nov  5 11:25 root
drwxr-xr-x  33 root root   980 Nov  5 11:30 run
lrwxrwxrwx   1 root root     8 Apr 22  2024 sbin -> usr/sbin
drwxr-xr-x   2 root root  4096 Apr 22  2024 srv
dr-xr-xr-x  13 root root     0 Nov  5 11:18 sys
drwxrwxrwt  12 root root  4096 Nov  5 11:35 tmp
drwxr-xr-x  12 root root  4096 Apr 22  2024 usr
drwxr-xr-x  14 root root  4096 Oct 20 09:58 var
```

Из вывода видно, что `/bin`, `/sbin`, `/lib` и `/lib64` в этой системе являются символическими ссылками (первый символ в правах — `l`), а `/proc` и `/sys` имеют размер `0` — это виртуальные файловые системы.

## 2. Где хранятся домашние каталоги обычных пользователей

Домашние каталоги обычных пользователей хранятся в **`/home`**, по одному каталогу на пользователя: `/home/<имя_пользователя>`.

```bash
raddd1us@ubuntu:~$ ls /home
```

```text
raddd1us  user1
```

```bash
raddd1us@ubuntu:~$ echo $HOME
```

```text
/home/raddd1us
```

## 3. Домашний каталог суперпользователя

Домашний каталог `root` находится в **`/root`**, а не в `/home`. Это сделано для того, чтобы администратор мог работать, даже если раздел `/home` не смонтирован. Доступ к `/root` закрыт для остальных пользователей (права `drwx------`).

```bash
raddd1us@ubuntu:~$ getent passwd root
```

```text
root:x:0:0:root:/root:/bin/bash
```

```bash
raddd1us@ubuntu:~$ ls -ld /root
```

```text
drwx------ 5 root root 4096 Nov  5 11:25 /root
```

## 4. Где хранятся основные системные конфигурационные файлы

Основные системные конфигурационные файлы хранятся в каталоге **`/etc`**. Это в основном текстовые файлы, которые можно редактировать вручную с правами администратора.

```bash
raddd1us@ubuntu:~$ ls -d /etc/passwd /etc/group /etc/fstab /etc/hostname /etc/ssh
```

```text
/etc/fstab  /etc/group  /etc/hostname  /etc/passwd  /etc/ssh
```

Примеры: `/etc/passwd` и `/etc/group` — учётные записи и группы, `/etc/shadow` — хеши паролей, `/etc/fstab` — таблица монтирования, `/etc/ssh/` — настройки SSH.

## 5. Назначение каталогов

| Каталог | Назначение |
|---|---|
| `/bin` | Основные исполняемые файлы команд, нужные всем пользователям и для работы системы (`ls`, `cp`, `cat`, `bash`). В современных дистрибутивах — символическая ссылка на `/usr/bin` |
| `/sbin` | Системные программы для администрирования, обычно запускаются от root (`fdisk`, `ip`, `reboot`). В современных дистрибутивах — ссылка на `/usr/sbin` |
| `/usr/bin` | Основная масса исполняемых файлов пользовательских программ и команд, установленных из пакетов |
| `/usr/sbin` | Административные программы, не нужные при обычной работе пользователя (`usermod`, `useradd`, `sshd`) |
| `/etc` | Системные конфигурационные файлы (учётные записи, сеть, службы, монтирование) |
| `/var` | Изменяемые данные: журналы (`/var/log`), кэш, очереди почты и печати, базы данных, данные пакетных менеджеров |
| `/tmp` | Временные файлы всех пользователей и программ; доступен на запись всем, обычно очищается при перезагрузке |
| `/home` | Домашние каталоги обычных пользователей с их личными файлами и настройками |
| `/root` | Домашний каталог суперпользователя `root` |
| `/proc` | Виртуальная файловая система (procfs): информация о процессах и состоянии ядра (`/proc/cpuinfo`, `/proc/meminfo`, `/proc/<PID>`) |
| `/sys` | Виртуальная файловая система (sysfs): информация об устройствах, драйверах и подсистемах ядра; через неё можно читать и менять параметры оборудования |

## 6. `/bin` и `/sbin`: каталоги или символические ссылки

```bash
raddd1us@ubuntu:~$ ls -ld /bin /sbin /usr/bin /usr/sbin
```

```text
lrwxrwxrwx  1 root root     7 Apr 22  2024 /bin -> usr/bin
lrwxrwxrwx  1 root root     8 Apr 22  2024 /sbin -> usr/sbin
drwxr-xr-x  2 root root 69632 Nov  5 11:20 /usr/bin
drwxr-xr-x  2 root root 28672 Nov  5 11:20 /usr/sbin
```

Дополнительная проверка:

```bash
raddd1us@ubuntu:~$ readlink -f /bin
raddd1us@ubuntu:~$ readlink -f /sbin
raddd1us@ubuntu:~$ file /bin /sbin
```

```text
/usr/bin
/usr/sbin
/bin:  symbolic link to usr/bin
/sbin: symbolic link to usr/sbin
```

**Вывод:** в моей системе `/bin` и `/sbin` — это **символические ссылки** на `/usr/bin` и `/usr/sbin` соответственно (первый символ в правах — `l`, после имени стоит `->`). Реальными каталогами являются `/usr/bin` и `/usr/sbin` (первый символ — `d`). Такая схема называется *merged-usr*: все исполняемые файлы физически лежат в `/usr`, а старые пути `/bin` и `/sbin` сохранены для совместимости.

## 7. Поиск исполняемого файла команды

Выбрана команда `usermod`, которая использовалась ранее при работе с пользователями.

```bash
raddd1us@ubuntu:~$ which usermod
```

```text
/usr/sbin/usermod
```

```bash
raddd1us@ubuntu:~$ type -a usermod
```

```text
usermod is /usr/sbin/usermod
usermod is /sbin/usermod
```

```bash
raddd1us@ubuntu:~$ whereis usermod
```

```text
usermod: /usr/sbin/usermod /usr/share/man/man8/usermod.8.gz
```

Проверка того, что физический файл один, а путь `/sbin/usermod` ведёт к нему через ссылку:

```bash
raddd1us@ubuntu:~$ ls -l /usr/sbin/usermod
raddd1us@ubuntu:~$ readlink -f /sbin/usermod
raddd1us@ubuntu:~$ dpkg -S /usr/sbin/usermod
```

```text
-rwxr-xr-x 1 root root 77280 Apr  9  2024 /usr/sbin/usermod
/usr/sbin/usermod
passwd: /usr/sbin/usermod
```

**Вывод:** физически исполняемый файл `usermod` расположен в **`/usr/sbin/usermod`** и принадлежит пакету `passwd`. Путь `/sbin/usermod` работает только потому, что `/sbin` — символическая ссылка на `/usr/sbin`.

## 8. Различия между обычным каталогом, символической ссылкой и виртуальной файловой системой

| | Обычный каталог | Символическая ссылка | Виртуальная файловая система |
|---|---|---|---|
| Что это | Контейнер для файлов и подкаталогов, реально хранящийся на диске | Специальный файл, содержащий путь к другому файлу или каталогу | Файловая система, существующая только в оперативной памяти и создаваемая ядром «на лету» |
| Где хранятся данные | На блочном устройстве (диск, раздел) | В самой ссылке хранится только путь-указатель, данных цели в ней нет | Не хранятся на диске: содержимое формирует ядро при обращении |
| Тип в `ls -l` | `d` | `l` (`/bin -> usr/bin`) | Обычно каталог `d` (`/proc`, `/sys`), но смонтирован отдельным типом ФС |
| Размер | Занимает место на диске (минимум блок) | Равен длине пути (`/bin` → 7 байт) | `0` |
| Если цель удалить | Данные теряются | Ссылка становится «битой» | Содержимое отражает текущее состояние ядра и меняется само |
| Примеры | `/usr/bin`, `/etc`, `/home` | `/bin`, `/sbin`, `/lib` | `/proc` (procfs), `/sys` (sysfs), `/dev` (devtmpfs) |

Проверка того, что `/proc` и `/sys` являются виртуальными файловыми системами:

```bash
raddd1us@ubuntu:~$ findmnt /proc
raddd1us@ubuntu:~$ findmnt /sys
```

```text
TARGET SOURCE FSTYPE OPTIONS
/proc  proc   proc   rw,nosuid,nodev,noexec,relatime
TARGET SOURCE FSTYPE OPTIONS
/sys   sysfs  sysfs  rw,nosuid,nodev,noexec,relatime
```

```bash
raddd1us@ubuntu:~$ ls -l /proc/cpuinfo
```

```text
-r--r--r-- 1 root root 0 Nov  5 11:40 /proc/cpuinfo
```

Файл `/proc/cpuinfo` имеет размер `0`, но при чтении выдаёт информацию о процессоре — она формируется ядром в момент обращения.
