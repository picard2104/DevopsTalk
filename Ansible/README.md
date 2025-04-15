
# 🚀 Ansible

![Ansible](https://img.shields.io/badge/Ansible-2.10%2B-red?logo=ansible)

---

## 🛠️ Установка
```bash
# Для Ubuntu/Debian
sudo apt update && sudo apt install ansible -y

# Для CentOS/RHEL
sudo yum install epel-release -y
sudo yum install ansible -y

# Проверка
ansible --version
```

---

## 📁 Инвентарь (inventory.ini)
```ini
[web]
server1 ansible_host=192.168.1.10
server2 ansible_host=192.168.1.11

[db]
dbserver ansible_host=192.168.1.20

[all:vars]
ansible_user=admin
ansible_ssh_private_key_file=~/.ssh/key.pem
```

---

## 🎯 Ad-Hoc Команды
```bash
# Проверка доступности
ansible all -m ping

# Получить информацию о дисках
ansible web -a "df -h"

# Установка пакета
ansible db -m apt -a "name=nginx state=present" --become

# Перезапуск сервиса
ansible web -m service -a "name=nginx state=restarted"

# Сбор всей информации о хосте
ansible all -m setup

# Использование модуля shell
ansible all -m shell -a "ls -lah /etc"

# Копирование файлов
ansible all -m copy -a "src=/home/ubuntu/file dest=/home/ubuntu/file" -b #-b это sudo

# Скачаивание из интернета
ansible all -m get_url -a "url=https://example.com/file.txt dest=/home/ubuntu/file.txt" -b
```

---

## 📜 Плейбук (deploy.yml)
```yaml
---
- name: Установка Nginx
  hosts: web
  become: yes
  
  tasks:
    - name: Обновить кэш пакетов
      apt:
        update_cache: yes

    - name: Установить Nginx
      apt:
        name: nginx
        state: latest

    - name: Запустить Nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

**Запуск плейбука:**
```bash
ansible-playbook deploy.yml -i inventory.ini
```

---

## 🔑 Ansible Vault
```bash
# Зашифровать файл
ansible-vault encrypt secrets.yml

# Редактировать зашифрованный файл
ansible-vault edit secrets.yml

# Запустить плейбук с vault
ansible-playbook site.yml --ask-vault-pass
```

---

## 🚦 Полезные команды
```bash
# Проверка синтаксиса
ansible-playbook deploy.yml --syntax-check

# Тестовый запуск (без изменений)
ansible-playbook deploy.yml --check

# Показать документацию модуля
ansible-doc service

# Показать информацию о хосте
ansible-inventory --host server1
```

---

## 📚 Ресурсы
- [Официальная документация](https://docs.ansible.com)
- [Ansible Galaxy](https://galaxy.ansible.com)
- [Шпаргалка PDF]([https://cdn.com/ansible-cheat-sheet.pdf](https://www.edureka.co/blog/wp-content/uploads/2018/11/Ansible-Cheat_Sheet_Edureka.pdf))
