# Pr4-Cron

# Cron-Systemd

# 📋 ОТЧЕТ: Сравнение Cron и Systemd для планирования задач в Linux

## 📌 Информация о выполнении
- **Студент:** Швыркова Яна
- **Группа:** ПИ-430Б

---

## 🎯 Цель работы
Сравнить два метода планирования периодических задач в Linux: традиционный **Cron** и современный **Systemd Timer**. Реализовать один и тот же скрипт обоими способами.

---

## 📁 Структура проекта
```
/home/Shvyrkova/
├── cron_scripts/
│   └── send_request.sh      # Bash-скрипт для отправки HTTP-запросов
├── cron_requests.log        # Лог-файл выполнения скрипта через Cron
├── monitoring-stack/        # Дополнительные материалы
└── ansible-managed-host/    # Дополнительные материалы
```

---

## 📝 Часть 1: Реализация через Cron

### 1.1 Созданный bash-скрипт
**Файл:** `~/cron_scripts/send_request.sh`

```bash
#!/bin/bash
TARGET_URL="https://api.example.com/webhook"
curl -s -w "\nHTTP Status: %{http_code}\n" "$TARGET_URL" >> /home/Shvyrkova/cron_requests.log 2>&1
echo "[$(date '+%Y-%m-%d %H:%M:%S')] Request sent to $TARGET_URL" >> /home/Shvyrkova/cron_requests.log
exit 0

```

*(Скриншот 1: Содержимое скрипта send_request.sh)*
<img width="939" height="75" alt="image" src="https://github.com/user-attachments/assets/a5d1acc7-dd34-4373-bd21-7654365058e6" />




---

### 1.2 Настройка расписания в Crontab
**Команда для просмотра:** `crontab -l`

```
*/5 * * * * /home/Shvyrkova/cron_scripts/send_request.sh
```

**Объяснение формата:**
```
*/5  *    *    *    *    /home/Shvyrkova/cron_scripts/send_request.sh
 │    │    │    │    │      │
 │    │    │    │    │      └── Команда для выполнения
 │    │    │    │    └───────── День недели (0-7, 0=воскресенье)
 │    │    │    └────────────── Месяц (1-12)
 │    │    └─────────────────── День месяца (1-31)
 │    └──────────────────────── Час (0-23)
 └───────────────────────────── Минута (0-59) → каждые 5 минут
```

*(Скриншот 2: Вывод команды crontab -e)*<img width="640" height="527" alt="image" src="https://github.com/user-attachments/assets/6ff71f67-ae57-4191-8160-454fc5318b24" />



---

### 1.3 Проверка работы Cron
**Лог-файл выполнения:** `~/cron_requests.log`

```bash
# Команда для просмотра логов
tail -10 ~/cron_requests.log
```

**Пример вывода:**
```
[2025-12-10 12:00:02] Request sent to https://api.example.com/webhook
HTTP Status: 000
[2025-12-10 12:05:01] Request sent to https://api.example.com/webhook
HTTP Status: 000
[2025-12-13 00:05:03] Request sent to https://api.example.com/webhook
HTTP Status: 000
```

*(Скриншот 3: Содержимое лог-файла)*
<img width="627" height="81" alt="image" src="https://github.com/user-attachments/assets/5eed0406-1723-4729-bd3d-96dc38654eca" />




---

### 1.4 Статус Cron демона
```bash
sudo systemctl status cron --no-pager | head -10
```

*(Скриншот 4: Статус службы cron)*
<img width="763" height="203" alt="image" src="https://github.com/user-attachments/assets/5ae925d4-c109-43a7-b0ad-d188636417bd" />



---

