
# 🐧 Ubuntu Linux

![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04%20LTS-orange?logo=ubuntu)

---

## 📦 Управление пакетами
```bash
# Обновить список пакетов
sudo apt update

# Установить пакет
sudo apt install nginx -y

# Удалить пакет
sudo apt remove nginx --purge

# Поиск пакета
apt search "python3.*dev"

# Обновить все пакеты
sudo apt upgrade -y && sudo apt autoremove
```

---

## 📂 Работа с файлами
```bash
# Просмотр содержимого
cat file.txt | less

# Поиск по содержимому
grep -Ri "error" /var/log/

# Мониторинг изменений
tail -f /var/log/syslog

# Архивирование
tar -czvf archive.tar.gz /path/to/folder

# Права доступа
chmod 600 private.key
chown www-data:www-data /var/www
```

---

## 🌐 Сеть
```bash
# Проверка подключения
ping 8.8.8.8

# Просмотр открытых портов
ss -tulpn

# DNS-запросы
dig example.com +short

# Сканирование сети
nmap -sV 192.168.1.0/24

# Маршрутизация
ip route show
```

---

## ⚙️ Systemd сервисы
```bash
# Запустить сервис
sudo systemctl start nginx

# Добавить в автозагрузку
sudo systemctl enable nginx

# Просмотр логов
journalctl -u nginx -f

# Создать свой сервис
sudo nano /etc/systemd/system/my-service.service
```

**Пример сервиса:**
```ini
[Unit]
Description=My Custom Service

[Service]
ExecStart=/usr/bin/python3 /opt/app/server.py
Restart=always

[Install]
WantedBy=multi-user.target
```

---

## 🖥️ Мониторинг
```bash
# Потребление CPU/Memory
top
htop

# Дисковое пространство
df -h

# IO операций
iostat -x 2

# Сетевой трафик
iftop -i eth0

# История процессов
sudo apt install nmon
nmon
```

---

## 🔐 Безопасность
```bash
# SSH-ключи
ssh-keygen -t ed25519

# Firewall (UFW)
sudo ufw allow 22/tcp
sudo ufw enable

# Проверка обновлений безопасности
sudo unattended-upgrades --dry-run

# Сканирование прав
sudo lynis audit system
```

---

## 🛠️ Полезные команды
```bash
# Поиск файлов
find / -name "*.conf" 2>/dev/null

# Просмотр запущенных процессов
ps aux | grep nginx

# Убить процесс
sudo kill -9 $(pidof bad-process)

# Работа с журналами
sudo journalctl --since "1 hour ago"

# Узнать версию ОС
lsb_release -a
```

---

## 📜 Bash-скрипты
**Пример скрипта для бэкапа:**
```bash
#!/bin/bash
BACKUP_DIR="/backups"
DATE=$(date +%Y-%m-%d)

tar -czf $BACKUP_DIR/backup-$DATE.tar.gz /var/www/html
find $BACKUP_DIR -type f -mtime +7 -delete
```

**Запуск скрипта:**
```bash
chmod +x backup.sh
./backup.sh
```

---

## 🚨 Частые проблемы
```diff
- bash: command not found
+ Решение: Установите пакет: sudo apt install missing-package

- Permission denied
+ Решение: Используйте sudo или chmod/chown

- Address already in use
+ Решение: Найдите процесс: sudo lsof -i :PORT
```

---

## 📚 Ресурсы
- [Официальная документация](https://help.ubuntu.com)
- [Linux Command](https://linuxcommand.org)
- [Explain Shell](https://explainshell.com)

> **💡 Профессиональный совет**  
> Настройте `.bashrc` для удобства:
> ```bash
> alias ll='ls -alhF'
> alias ports='ss -tulpn'
> export HISTTIMEFORMAT="%d/%m/%y %T "
> ```

---

## 🎯 Пример рабочего процесса
1. Обновить сервер:
```bash
sudo apt update && sudo apt upgrade -y
```

2. Проверить свободное место:
```bash
df -h | grep -v tmpfs
```

3. Найти ошибки в логах:
```bash
sudo journalctl -u nginx --since today | grep -i error
```

4. Создать задание в cron:
```bash
crontab -e
# Добавить строку: 0 3 * * * /path/to/backup.sh
```

