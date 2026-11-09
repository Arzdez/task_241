# Журналы

### 1. Просмотр журнала службы SSH

Для просмотра журнала службы SSH использовалась команда:

```bash
sudo journalctl -u sshd.service --no-pager
```

Пример полученных записей:

```text
Nov 07 19:49:53 student-04 systemd[1]: Starting sshd.service - OpenSSH server daemon...
Nov 07 19:49:53 student-04 systemd[1]: Started sshd.service - OpenSSH server daemon.
Nov 07 19:49:53 student-04 sshd[822]: Server listening on 0.0.0.0 port 22.
Nov 07 19:49:53 student-04 sshd[822]: Server listening on :: port 22.
Nov 08 17:02:00 student-04 sshd[4818]: Accepted password for student from 192.168.6.83 port 57644 ssh2
Nov 08 17:02:00 student-04 sshd[4818]: pam_tcb(sshd:session): Session opened for student by (uid=0)
```

Таким образом, журнал содержит события запуска службы, прослушивания порта 22 и подключения пользователей.

### 2. Просмотр журнала системы в реальном времени

Для просмотра новых сообщений журнала в реальном времени использовалась команда:

```bash
sudo journalctl -f
```

В результате отображались новые события системы, например:

```text
Nov 08 17:02:01 student-04 systemd[1]: Started user@1000.service - User Manager for UID 1000.
Nov 08 17:02:01 student-04 systemd[1]: Started session-65.scope - Session 65 of User student.
Nov 08 17:02:42 student-04 sudo[4865]: student : HOST=student-04 ; TTY=pts/0 ; PWD=/home/student ; USER=root ; COMMAND=/bin/journalctl -u sshd.service --no-pager
```

Ключ `-f` (`--follow`) переводит `journalctl` в режим ожидания и показывает новые записи по мере их появления.

### 3. Просмотр журнала только службы SSH в реальном времени

Использовалась команда:

```bash
sudo journalctl -u sshd.service -f
```

Были получены записи только от службы `sshd.service`:

```text
Nov 08 17:01:50 student-04 sshd[4818]: pam_tcb(sshd:auth): Authentication failed for student from (uid=0)
Nov 08 17:01:53 student-04 sshd[4818]: Failed password for student from 192.168.6.83 port 57644 ssh2
Nov 08 17:02:00 student-04 sshd[4818]: pam_tcb(sshd:auth): Authentication passed for student from (uid=0)
Nov 08 17:02:00 student-04 sshd[4818]: Accepted password for student from 192.168.6.83 port 57644 ssh2
Nov 08 17:02:00 student-04 sshd[4818]: pam_tcb(sshd:session): Session opened for student by (uid=0)
```

После проверки режим реального времени был остановлен комбинацией `Ctrl+C`.

### 4. Журнал SSH за текущую загрузку системы

Для получения журнала SSH только за текущую загрузку использовалась команда:

```bash
sudo journalctl -u sshd.service -b --no-pager
```

Текущая загрузка началась 7 ноября в 19:49:53:

```text
Nov 07 19:49:53 student-04 systemd[1]: Starting sshd.service - OpenSSH server daemon...
Nov 07 19:49:53 student-04 systemd[1]: Started sshd.service - OpenSSH server daemon.
Nov 07 19:49:53 student-04 sshd[822]: Server listening on 0.0.0.0 port 22.
Nov 07 19:49:53 student-04 sshd[822]: Server listening on :: port 22.
```

Ключ `-b` ограничивает вывод журналом текущей загрузки системы.

### 5. Сообщения уровня warning и выше

Для вывода сообщений с уровнем `warning` и более высоким использовалась команда:

```bash
sudo journalctl -p warning..emerg --no-pager -n 20
```

Получены, например, следующие сообщения:

```text
Nov 08 12:33:20 student-04 systemd-sysv-generator[3536]: SysV service '/etc/rc.d/init.d/cloud-init' lacks a native systemd unit file, automatically generating a unit file for compatibility.
Nov 08 12:33:20 student-04 systemd-sysv-generator[3536]: Please update package to include a native systemd unit file.
Nov 08 13:20:16 student-04 systemd[1]: unit-script.service: Failed with result 'exit-code'.
Nov 08 13:20:16 student-04 systemd[1]: Failed to start unit-script.service - Create system information files.
```

Таким образом, фильтр `warning..emerg` позволяет получить только предупреждения и более серьёзные сообщения.

### 6. Доступ к журналам без `journalctl` и место хранения данных

Получить доступ к журналам systemd можно не только с помощью `journalctl`. Для программного доступа используется API `systemd-journald` (`sd-journal`), а другие системные программы могут обращаться к журналу через соответствующие интерфейсы.

В данной системе обнаружены следующие каталоги:

```text
/run/log/journal
/var/log/journal
```

Полученный результат:

```text
drwxr-sr-x+ 2 root systemd-journal   40 Nov  7 19:49 /run/log/journal
drwxr-sr-x+ 3 root systemd-journal 4096 Sep 14 22:34 /var/log/journal
```

`/run/log/journal` используется для журналов текущего сеанса работы системы, а `/var/log/journal` - для постоянного хранения журналов.

`journald` не использует обычный текстовый формат, а хранит записи в бинарных структурированных файлах журнала. Это позволяет сохранять вместе с сообщением различные метаданные: время, приоритет, идентификатор процесса, пользователя, службу, идентификатор загрузки и другие поля. Благодаря этому журнал можно быстро фильтровать и искать по различным параметрам.

### 7. Собственное подключение по SSH

Собственное подключение было найдено в журнале SSH:

```bash
sudo journalctl -u sshd.service --since "17:00:00" --until "17:05:00" --no-pager
```

Соответствующая запись:

```text
Nov 08 17:02:00 student-04 sshd[4818]: Accepted password for student from 192.168.6.83 port 57644 ssh2
Nov 08 17:02:00 student-04 sshd[4818]: pam_tcb(sshd:session): Session opened for student by (uid=0)
```

Запись показывает успешную авторизацию пользователя `student` по SSH с адреса `192.168.6.83`.

Перед успешной авторизацией была одна неудачная попытка:

```text
Nov 08 17:01:53 student-04 sshd[4818]: Failed password for student from 192.168.6.83 port 57644 ssh2
```

После неё в 17:02:00 авторизация была успешно выполнена.

### 8. Сколько будет 2 - 2?

```text
2 - 2 = НОЛЬ!!!
```