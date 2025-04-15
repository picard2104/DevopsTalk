```markdown
# 🐳 Docker Cheat Sheet

![Docker Version](https://img.shields.io/badge/Docker-24.0-2496ED?logo=docker&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-brightgreen)


---

## 🚀 Основные команды

### 🐋 Работа с контейнерами
| Команда | Описание |
|---------|----------|
| `docker run -d --name my_container nginx` | Запустить контейнер в фоне |
| `docker stop my_container` | Остановить контейнер |
| `docker start my_container` | Запустить остановленный контейнер |
| `docker rm my_container` | Удалить контейнер |
| `docker ps -a` | Показать все контейнеры |

### 📦 Работа с образами
```bash
# Собрать образ из Dockerfile
docker build -t my_image:1.0 .

# Загрузить образ в реестр
docker push my_image:1.0

# Просмотреть слои образа
docker image history my_image

# Удалить неиспользуемые образы
docker image prune -a
```

---

## 🧩 Пример Dockerfile
```dockerfile
# Многоступенчатая сборка
FROM golang:1.20 as builder
WORKDIR /app
COPY . .
RUN go build -o myapp .

FROM alpine:3.18
WORKDIR /root/
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

---

## 🔄 Жизненный цикл контейнера
```bash
# Запуск с пробросом портов и volumes
docker run -d \
  -p 8080:80 \
  -v ./data:/app/data \
  --env-file .env \
  --name web_server \
  nginx:alpine

# Выполнить команду в запущенном контейнере
docker exec -it web_server sh

# Просмотр логов в реальном времени
docker logs -f --tail 100 web_server
```

---

## 🌐 Сети
```bash
# Создать сеть
docker network create my_network

# Подключить контейнер к сети
docker network connect my_network web_server

# Просмотр IP-адресов контейнеров
docker network inspect my_network
```

---

## 💾 Тома
```bash
# Создать именованный том
docker volume create db_data

# Просмотр информации о томах
docker volume inspect db_data

# Удалить неиспользуемые тома
docker volume prune
```

---

## 📌 Лучшие практики

### 🔒 Безопасность
```dockerfile
# Использовать непривилегированного пользователя
RUN groupadd -r appuser && useradd -r -g appuser appuser
USER appuser
```

### 🏗️ Оптимизация образов
```dockerfile
# Использовать .dockerignore
node_modules
.git
*.log
```

### 🚀 Производительность
```bash
# Ограничение ресурсов
docker run -d \
  --memory=512m \
  --cpus=1.5 \
  my_image
```

---

## 🗂️ Полезные советы

1. **Очистка системы**  
   ```bash
   docker system prune -af --volumes
   ```

2. **Интерактивный режим**  
   ```bash
   docker run -it --rm ubuntu bash
   ```

3. **Просмотр изменений в контейнере**  
   ```bash
   docker diff my_container
   ```

4. **Сохранение/загрузка образов**  
   ```bash
   docker save -o my_image.tar my_image:1.0
   docker load -i my_image.tar
   ```

---

## 📚 Ресурсы

- [Официальная документация](https://docs.docker.com/) 📄
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) 🏆
