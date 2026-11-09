# Юниты

### 1. Что такое systemd unit?

Systemd unit - это единица управления системой, описывающая определённый объект или действие, которым управляет система инициализации systemd.

С помощью юнитов можно управлять службами, точками монтирования, устройствами, таймерами и другими компонентами системы.

Основные типы юнитов:
- `.service` - службы и процессы;
- `.timer` - таймеры для запуска юнитов по расписанию;
- `.socket` - сокеты для межпроцессного взаимодействия;
- `.mount` - точки монтирования файловых систем;
- `.target` - группы юнитов, объединённые общей целью.

### 2. Проверка статуса systemd unit

Для работы был выбран юнит `crond.service` - служба планировщика заданий Cron.

Для проверки его состояния использовалась команда:

```bash
systemctl status crond.service --no-pager
```

Исходный результат:

```text
● crond.service - Vixie Cron Daemon
     Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; preset: enabled)
     Active: active (running) since Sat 2026-11-07 19:49:53 UTC; 16h ago
   Main PID: 815 (crond)
      Tasks: 1 (limit: 1728)
     Memory: 420K (peak: 1.2M)
```

Дополнительно для проверки автозагрузки использовалась команда:

```bash
systemctl is-enabled crond.service
```

Результат:

```text
enabled
```

Команда `systemctl status` выводит:
- название и описание юнита;
- состояние загрузки конфигурации (`Loaded`);
- текущее состояние работы (`Active`);
- время запуска;
- PID главного процесса;
- количество задач;
- используемую память и процессорное время;
- последние записи журнала службы.

### 3. Остановка выбранного сервиса

Для остановки службы использовалась команда:

```bash
sudo systemctl stop crond.service
```

После остановки была выполнена проверка:

```bash
systemctl status crond.service --no-pager
```

Результат:

```text
○ crond.service - Vixie Cron Daemon
     Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; preset: enabled)
     Active: inactive (dead)
```

Служба успешно остановлена. При этом автозагрузка осталась включённой.

Проверка:

```bash
systemctl is-enabled crond.service
```

Результат:

```text
enabled
```

### 4. Повторный запуск сервиса

Для повторного запуска использовалась команда:

```bash
sudo systemctl start crond.service
```

После этого выполнена проверка:

```bash
systemctl status crond.service --no-pager
```

Результат:

```text
● crond.service - Vixie Cron Daemon
     Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; preset: enabled)
     Active: active (running)
     Main PID: 3400 (crond)
```

Служба успешно запущена и вернулась в состояние `active (running)`.

### 5. Отключение сервиса из автозагрузки

Для отключения автоматического запуска службы использовалась команда:

```bash
sudo systemctl disable crond.service
```

Результат выполнения:

```text
Removed '/etc/systemd/system/multi-user.target.wants/crond.service'.
```

Для проверки использовалась команда:

```bash
systemctl is-enabled crond.service
```

Результат:

```text
disabled
```

При этом проверка статуса показала:

```text
Active: active (running)
Loaded: loaded (/usr/lib/systemd/system/crond.service; disabled; preset: enabled)
```

Таким образом, служба продолжила работать, но была исключена из автозагрузки.

### 6. Повторное включение сервиса в автозагрузку

Для восстановления автоматического запуска использовалась команда:

```bash
sudo systemctl enable crond.service
```

Результат:

```text
Created symlink '/etc/systemd/system/multi-user.target.wants/crond.service' → '/usr/lib/systemd/system/crond.service'.
```

Проверка:

```bash
systemctl is-enabled crond.service
```

Результат:

```text
enabled
```

Дополнительно проверено состояние службы:

```text
Active: active (running)
Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; preset: enabled)
```

Автозагрузка успешно восстановлена.

### 7. Различия между состояниями active, enabled, disabled, failed

- `active` - юнит в данный момент активен. Для службы обычно означает, что процесс работает.
- `enabled` - автоматический запуск юнита настроен.
- `disabled` - автоматический запуск юнита не настроен.
- `failed` - при запуске или работе юнита произошла ошибка.

Примеры состояний:
- `active` - служба `crond.service` после запуска;
- `enabled` - служба `crond.service` до отключения автозагрузки и после её восстановления;
- `disabled` - служба `crond.service` после выполнения команды `disable`;
- `failed` - состояние юнита, завершившего работу с ошибкой.

Состояния `active` и `enabled` не являются взаимоисключающими. Первое характеризует текущее состояние работы, второе - настройку автоматического запуска.

### 8. Что такое systemd timers?

Systemd timers - это специальные юниты типа `.timer`, предназначенные для запуска других юнитов по расписанию или через определённые промежутки времени.

Они используются для автоматизации периодических задач, например очистки временных файлов, обслуживания системы и выполнения резервного копирования.

Таймер обычно запускает соответствующий юнит типа `.service`.

### 9. Поиск существующих timers и определение запускаемого unit

Для поиска таймеров использовалась команда:

```bash
systemctl list-timers --all --no-pager
```

Результат:

```text
NEXT                        LEFT LAST                         PASSED UNIT                         ACTIVATES
Sun 2026-11-08 20:04:48 UTC   7h Sat 2026-11-07 20:04:48 UTC 16h ago systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service

1 timers listed.
```

Дополнительно список активных таймеров был получен командой:

```bash
systemctl list-units --type=timer --all --no-pager
```

Результат:

```text
UNIT                         LOAD   ACTIVE SUB     DESCRIPTION
systemd-tmpfiles-clean.timer loaded active waiting Daily Cleanup of Temporary Directories

1 loaded units listed.
```

Для изучения конфигурации таймера использовалась команда:

```bash
systemctl cat systemd-tmpfiles-clean.timer
```

Основная часть конфигурационного файла:

```ini
[Unit]
Description=Daily Cleanup of Temporary Directories

[Timer]
OnBootSec=15min
OnUnitActiveSec=1d
```

Параметры означают:
- `OnBootSec=15min` - запуск через 15 минут после загрузки системы;
- `OnUnitActiveSec=1d` - повторный запуск через сутки после предыдущей активации службы.

Для проверки состояния таймера использовалась команда:

```bash
systemctl status systemd-tmpfiles-clean.timer --no-pager
```

Результат:

```text
● systemd-tmpfiles-clean.timer - Daily Cleanup of Temporary Directories
     Loaded: loaded (/usr/lib/systemd/system/systemd-tmpfiles-clean.timer; static)
     Active: active (waiting)
    Trigger: Sun 2026-11-08 20:04:48 UTC; 7h left
   Triggers: ● systemd-tmpfiles-clean.service
```

Таким образом, таймер находится в состоянии `active (waiting)` и ожидает следующего запуска.

Для проверки связанной службы использовалась команда:

```bash
systemctl status systemd-tmpfiles-clean.service --no-pager
```

Результат:

```text
○ systemd-tmpfiles-clean.service - Cleanup of Temporary Directories
     Loaded: loaded (/usr/lib/systemd/system/systemd-tmpfiles-clean.service; static)
     Active: inactive (dead)
TriggeredBy: ● systemd-tmpfiles-clean.timer
   Main PID: 1021 (code=exited, status=0/SUCCESS)
```

В результате установлено, что `systemd-tmpfiles-clean.timer` запускает `systemd-tmpfiles-clean.service`, которая выполняет очистку временных каталогов.

### Разница между текущим состоянием сервиса и настройкой автозапуска

Текущее состояние сервиса показывает, работает ли он в данный момент. Настройка автозапуска определяет, будет ли он автоматически запускаться при загрузке системы или достижении соответствующей цели.

Для управления этими параметрами используются разные команды:
- `systemctl start` и `systemctl stop` - запуск и остановка службы;
- `systemctl enable` и `systemctl disable` - включение и отключение автоматического запуска.

Проведённая работа показала, что службу можно остановить, не отключая автозагрузку, а также отключить автозагрузку, не останавливая уже работающую службу.
