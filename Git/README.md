
# 🚀 Git

![Git](https://img.shields.io/badge/Git-2.40%2B-orange?logo=git)

Основные команды Git для разработчика. Просто скопируйте весь код ниже в свой README.md!

---

## 🛠️ Установка
```bash
# Ubuntu/Debian
sudo apt update && sudo apt install git -y

# Windows
# Скачайте с https://git-scm.com/download/win

# Проверка версии
git --version
```

---

## 🏁 Начало работы
```bash
# Инициализация репозитория
git init

# Клонирование репозитория
git clone https://github.com/user/repo.git

# Показать статус
git status
```

---

## 📝 Основные команды
```bash
# Добавить все файлы
git add .

# Коммит изменений
git commit -m "Сообщение коммита"

# Отправить изменения в удаленный репозиторий
git push origin main

# Обновить локальный репозиторий
git pull origin main
```

---

## 🌿 Ветки
```bash
# Создать новую ветку
git branch feature/new-feature

# Переключиться на ветку
git checkout feature/new-feature

# Слить ветки (из ветки main)
git merge feature/new-feature

# Удалить ветку
git branch -d feature/new-feature
```

---

## 🔄 Работа с историей
```bash
# Показать историю коммитов
git log --oneline

# Отменить изменения в файле
git checkout -- file.txt

# Сбросить последний коммит
git reset HEAD~1

# Переместить последний коммит (amend)
git commit --amend -m "Новое сообщение"
```

---

## 🛠️ Продвинутые команды
```bash
# Создать тег
git tag v1.0.0

# Просмотр изменений
git diff

# Временно сохранить изменения
git stash

# Восстановить сохраненные изменения
git stash pop
```

---

## ⚙️ Конфигурация
```bash
# Установить имя пользователя
git config --global user.name "Ваше Имя"

# Установить email
git config --global user.email "ваш@email.com"

# Просмотр конфигурации
git config --list
```

---

## 🚨 Частые ошибки
```diff
- fatal: not a git repository (or any parent up to mount point /)
+ Решение: Выполните git init или перейдите в папку с репозиторием

- error: failed to push some refs to 'https://github.com/...'
+ Решение: Сначала выполните git pull --rebase
```

---

## 📚 Ресурсы
- [Официальная документация](https://git-scm.com/doc)
- [Git Book](https://git-scm.com/book/ru/v2)
- [Интерактивный тренажер](https://learngitbranching.js.org)

> **💡 Профессиональный совет**  
> Используйте `.gitignore` для исключения ненужных файлов. Пример:
> ```gitignore
> # .gitignore
> node_modules/
> *.log
> .env
> ```

---

## 🎉 Пример рабочего процесса
1. Создать ветку:
```bash
git checkout -b feature/add-login
```

2. Добавить изменения и сделать коммит:
```bash
git add .
git commit -m "Добавлена форма логина"
```

3. Отправить изменения:
```bash
git push origin feature/add-login
```

4. Создать Pull Request на GitHub!
```
