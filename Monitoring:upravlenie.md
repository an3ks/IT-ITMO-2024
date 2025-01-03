# Лабораторная работа: Контроль/мониторинг ресурсов 
### Выполнил Авдиенко Данила (464919) K3140

## Цель работы
Научиться использовать инструменты для мониторинга ресурсов системы, а также писать скрипты для взаимодействия с этой информацией.

## Описание
В данной лабораторной работе будут рассмотрены основы мониторинга ресурсов системы. Также вам предстоит написать Bash-скрипт, который собирает данны о ресурсах и сохраняет их в файл.

---

## Часть 1: Основы мониторинга
### Задание 1: Установка и использование `htop`
#### htop — это инструмент для мониторинга процессов в Linux, он показывает состояние CPU/RAM в реальном времени.
1. Установите `htop`:
   ```bash
   sudo apt update && sudo apt install htop
   ```
2. Запустите `htop`:
   ```bash
   htop
   ```
3. Изучите интерфейс:

**Вопросы:**
1. Как узнать, какой процесс потребляет больше всего ресурсов CPU?
2. Как завершить процесс через `htop`?

### Задание 2: Мониторинг производительности с помощью `vmstat`
1. Запустите `vmstat` с интервалом 2 секунды:
   ```bash
   vmstat 2
   ```
2. Обратите внимание на следующие метрики:
   - `r` — количество процессов в очереди выполнения.
   - `free` — доступная память.
   - `si` и `so` — свопинг.

---

## Часть 2: Автоматизация мониторинга

### Задание: Написание скрипта
Создайте Bash-скрипт, который:
1. Собирает данные о загрузке CPU, памяти и дисковом пространстве.
2. Сохраняет их в лог-файл `/var/log/system_monitor.log`.

#### Пример скрипта
```bash
LOGFILE="/var/log/system_monitor.log"
DATE=$(date "+%Y-%m-%d %H:%M:%S")
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2 + $4}')%
MEM_USAGE=$(free -m | awk 'NR==2{printf "%.2f%%", $3*100/$2 }')
DISK_USAGE=$(df -h / | awk 'NR==2{print $5}')
echo "$DATE | CPU: $CPU_USAGE | Memory: $MEM_USAGE | Disk: $DISK_USAGE" >> $LOGFILE
```

#### Настройка выполнения через cron
1. Откройте редактор crontab:
   ```bash
   crontab -e
   ```
2. Добавьте задачу для выполнения скрипта каждые 10 минут:
   ```bash
   */10 * * * * path_to_your_script
   ```

---

## Дополнительные материалы
- [Документация `htop`](https://htop.dev/)
- [Руководство по `vmstat`](https://man7.org/linux/man-pages/man8/vmstat.8.html)
