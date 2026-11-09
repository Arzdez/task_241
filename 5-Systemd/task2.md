# Пишем юниты

## 1. Создание скрипта

Создан скрипт `/usr/local/bin/unit_script.sh`, который формирует каталог `unit-files` в домашней директории пользователя, от имени которого выполняется.

Полный текст скрипта:

```bash
#!/usr/bin/env bash
set -euo pipefail

if [[ -z "${HOME:-}" || ! -d "$HOME" ]]; then
    echo "Ошибка: домашний каталог пользователя не найден." >&2
    exit 1
fi

cd "$HOME"

OUTPUT_DIR="$HOME/unit-files"

if [[ -e "$OUTPUT_DIR" && ! -d "$OUTPUT_DIR" ]]; then
    echo "Ошибка: путь $OUTPUT_DIR существует, но не является каталогом." >&2
    exit 1
fi

mkdir -p -- "$OUTPUT_DIR"

date '+%Y-%m-%d %H:%M:%S %Z' > "$OUTPUT_DIR/1"
uname -r > "$OUTPUT_DIR/2"
hostname > "$OUTPUT_DIR/3"
ls -la "$HOME" > "$OUTPUT_DIR/4"

echo "Файлы успешно обновлены в $OUTPUT_DIR"
```

Скрипт создаёт четыре файла:

- `1` - текущая дата и время;
- `2` - версия ядра операционной системы;
- `3` - имя компьютера;
- `4` - список содержимого домашнего каталога пользователя.

Для проверки существования файлов использовались команды:

```bash
sudo ls -la /home/unitrunner/unit-files
```

Результат проверки: все четыре файла созданы успешно.

Полученные данные:
- Дата и время: `2026-11-08 13:11:26 UTC`.
- Версия ядра: `6.12.51-6.12-alt1`.
- Имя компьютера: `student-04`.
- Список содержимого домашнего каталога записан в файл `4`.

## 2. Создание systemd-службы

Для запуска скрипта создан файл `/etc/systemd/system/unit-script.service`.

Содержимое:

```ini
[Unit]
Description=Create system information files

[Service]
Type=oneshot
User=unitrunner
WorkingDirectory=/
ExecStart=/usr/local/bin/unit_script.sh
```

Параметр `Type=oneshot` используется для службы, которая выполняет задачу и завершается.

Параметр `User=unitrunner` задаёт пользователя, от имени которого выполняется скрипт.

Параметр `WorkingDirectory=/` задаёт рабочую директорию службы. При этом сам скрипт самостоятельно переходит в домашний каталог пользователя.

Для применения конфигурации и запуска службы выполнены команды:

```bash
sudo systemctl daemon-reload
sudo systemctl start unit-script.service
systemctl status unit-script.service --no-pager
```

Проверка результата:

```bash
sudo systemctl show unit-script.service \
-p Result -p ExecMainCode -p ExecMainStatus
```

Полученный результат:

```text
Result=success
ExecMainCode=0
ExecMainStatus=0
```

Файлы успешно обновились, что подтверждено изменением времени их модификации.

## 3. Создание systemd-таймера

Создан файл `/etc/systemd/system/unit-script.timer`.

Содержимое:

```ini
[Unit]
Description=Run unit-script.service every five minutes

[Timer]
OnBootSec=1min
OnUnitActiveSec=5min
Unit=unit-script.service

[Install]
WantedBy=timers.target
```

Назначение параметров:
- `OnBootSec=1min` - первый запуск через минуту после загрузки системы.
- `OnUnitActiveSec=5min` - повторный запуск через пять минут после активации службы.
- `Unit=unit-script.service` - служба, которую запускает таймер.
- `WantedBy=timers.target` - подключение таймера к автозапуску.

Для применения конфигурации, включения и запуска таймера выполнены команды:

```bash
sudo systemctl daemon-reload
sudo systemctl enable unit-script.timer
sudo systemctl start unit-script.timer
```

Проверка:

```bash
systemctl is-enabled unit-script.timer
systemctl is-active unit-script.timer
systemctl list-timers --all --no-pager
```

Полученные результаты:

```text
enabled
active
```

В списке таймеров отображалось следующее расписание:

```text
Sun 2026-11-08 13:23:35 UTC
4min 56s
Sun 2026-11-08 13:18:35 UTC
unit-script.timer
unit-script.service
```

Автоматический запуск подтверждён журналом: служба была запущена таймером в `13:18:35 UTC`, после чего файлы обновились.

## 4. Пользователь системных служб по умолчанию

Системные службы systemd по умолчанию запускаются от имени пользователя `root`, если в конфигурации службы не задан параметр `User=`.

В созданной службе указан параметр:

```ini
User=unitrunner
```

Для проверки использовалась команда:

```bash
systemctl show unit-script.service -p User -p WorkingDirectory
```

Результат:

```text
WorkingDirectory=/
User=unitrunner
```

## 5. Создание отдельного пользователя

Создан пользователь `unitrunner` с домашним каталогом:

```bash
sudo /usr/sbin/useradd -m -s /bin/bash unitrunner
```

Проверка:

```bash
getent passwd unitrunner
ls -ld /home/unitrunner
```

Полученные данные:

```text
unitrunner:x:1002:1006::/home/unitrunner:/bin/bash

drwx------ 8 unitrunner unitrunner 4096 Nov 8 13:10 /home/unitrunner
```

## 6. Запуск скрипта от имени созданного пользователя

Для проверки запуска от имени `unitrunner` использовалась команда:

```bash
sudo -u unitrunner /usr/local/bin/unit_script.sh
```

Результат:

```text
Файлы успешно обновлены в /home/unitrunner/unit-files
```

Владельцем созданных файлов является пользователь `unitrunner`.

## 7. Работа скрипта независимо от текущей директории

Для выполнения этого требования в скрипте используется переменная `$HOME`:

```bash
cd "$HOME"
OUTPUT_DIR="$HOME/unit-files"
```

Таким образом, скрипт самостоятельно определяет домашнюю директорию пользователя, от имени которого запущен, и сохраняет результаты именно туда.

Дополнительно в конфигурации службы задано:

```ini
WorkingDirectory=/
```

Проверка показала, что файлы успешно создаются в `/home/unitrunner/unit-files`.

## 8. Определение PID процесса

PID процесса скрипта можно увидеть в журнале systemd.

Например, при запуске службы в `13:14:31 UTC` журнал содержал запись:

```text
unit_script.sh[3835]: Файлы успешно обновлены в /home/unitrunner/unit-files
```

При автоматическом запуске в `13:18:35 UTC`:

```text
unit_script.sh[3968]: Файлы успешно обновлены в /home/unitrunner/unit-files
```

Таким образом, PID процесса скрипта при выполнении составлял, например, `3835` и `3968`.

Скрипт выполняется быстро, поэтому длительно наблюдать его активный процесс не удалось.

## 9. Поиск записей о запуске службы

Для просмотра записей журнала использовалась команда:

```bash
sudo journalctl -u unit-script.service -n 15 --no-pager
```

Пример успешного запуска:

```text
Nov 08 13:14:31 student-04 systemd[1]: Starting unit-script.service - Create system information files...
Nov 08 13:14:31 student-04 unit_script.sh[3835]: Файлы успешно обновлены в /home/unitrunner/unit-files
Nov 08 13:14:31 student-04 systemd[1]: unit-script.service: Deactivated successfully.
Nov 08 13:14:31 student-04 systemd[1]: Finished unit-script.service - Create system information files.
```

Журнал позволяет отслеживать время запуска, результат выполнения и сообщения самого скрипта.

## 10. Искусственное создание ошибки и её исправление

Для проверки диагностики ошибок в скрипт была намеренно добавлена несуществующая команда:

```bash
this_command_does_not_exist
```

После этого выполнен запуск:

```bash
sudo systemctl start unit-script.service
```

Служба завершилась с ошибкой:

```text
Job for unit-script.service failed because the control process exited with error code.
```

Для определения причины использовались команды:

```bash
systemctl status unit-script.service --no-pager
sudo journalctl -u unit-script.service -n 15 --no-pager
```

В журнале обнаружены следующие сообщения:

```text
unit-script.service: Main process exited, code=exited, status=127/n/a
unit-script.service: Failed with result 'exit-code'.
Failed to start unit-script.service - Create system information files.

/usr/local/bin/unit_script.sh: line 25:
this_command_does_not_exist: command not found
```

Причина ошибки - вызов несуществующей команды. Код завершения `127` означает, что команда не найдена.

Для исправления удалена ошибочная строка:

```bash
sudo sed -i '/this_command_does_not_exist/d' /usr/local/bin/unit_script.sh
```

Затем сброшено состояние ошибки и повторно запущена служба:

```bash
sudo systemctl reset-failed unit-script.service
sudo systemctl start unit-script.service
```

Проверка результата:

```bash
sudo systemctl show unit-script.service \
-p Result -p ExecMainCode -p ExecMainStatus
```

Полученный результат:

```text
Result=success
ExecMainCode=1
ExecMainStatus=0
```

В журнале появились записи об успешном завершении:

```text
Nov 08 13:23:39 student-04 unit_script.sh[4108]: Файлы успешно обновлены в /home/unitrunner/unit-files
Nov 08 13:23:39 student-04 systemd[1]: unit-script.service: Deactivated successfully.
Nov 08 13:23:39 student-04 systemd[1]: Finished unit-script.service - Create system information files.
```

После исправления таймер также был повторно включён и запущен:

```bash
sudo systemctl enable --now unit-script.timer
```

Проверка подтвердила:

```text
enabled
active
```

Таким образом, ошибка была обнаружена по журналу systemd, её причина определена, скрипт исправлен, а служба и таймер восстановлены.