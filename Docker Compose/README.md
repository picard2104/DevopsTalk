
```markdown
# 🐳 Docker Compose Cheat Sheet

![Docker Compose](https://img.shields.io/badge/Docker_Compose-2.18.1-2496ED?logo=docker&logoColor=white) 
![Version](https://img.shields.io/badge/Version-1.0.0-blueviolet)


## 🚀 Основные команды

### 🛠️ Управление сервисами
| Команда | Описание |
|---------|----------|
| `docker-compose up -d` | Запуск в фоновом режиме |
| `docker-compose down` | Остановка и очистка |
| `docker-compose restart` | Перезапуск сервисов |
| `docker-compose pause` | Приостановка контейнеров |

### 🔍 Инспекция
```bash
# Показать запущенные сервисы
docker-compose ps

# Логи в реальном времени
docker-compose logs -f --tail=100 [service]

# Проверить конфигурацию
docker-compose config
```

### ⚙️ Утилиты
```bash
# Выполнить команду в контейнере
docker-compose exec [service] sh

# Пересобрать образы
docker-compose build --no-cache

# Показать зависимости
docker-compose top
```

---

## 🧩 Пример docker-compose.yml
```yaml
version: '3.8'

services:
  webapp:
    build: ./app
    image: my-webapp:1.0
    ports:
      - "8080:80"
    environment:
      - NODE_ENV=production
    depends_on:
      - redis
      - db

  redis:
    image: redis:alpine
    volumes:
      - redis_data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s

  db:
    image: postgres:13
    env_file:
      - .db.env
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  redis_data:
  postgres_data:
```

---

## 📌 Лучшие практики

### 🌐 Сети
```yaml
networks:
  front-tier:
    driver: bridge
  back-tier:
    driver: bridge
```

### 🔄 Перезапуск
```yaml
restart_policy:
  condition: on-failure
  max_attempts: 3
```

### ⚠️ Ограничения ресурсов
```yaml
deploy:
  resources:
    limits:
      cpus: '0.50'
      memory: 512M
```

---

## 🗂️ Полезные советы

1. **Используйте override файлы**  
   Создавайте `docker-compose.override.yml` для разработки

2. **Очистка данных**  
   ```bash
   docker-compose down -v --rmi all
   ```

3. **Быстрое обновление**  
   ```bash
   docker-compose pull && docker-compose up -d
   ```

4. **Переменные окружения**  
   Используйте `.env` файл для общих настроек:
   ```ini
   COMPOSE_PROJECT_NAME=myapp
   TAG=latest
   ```

---

## 📚 Ресурсы

- [Официальная документация](https://docs.docker.com/compose/)
- [Compose Specification](https://compose-spec.io/)
- [Best Practices Guide](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

```
