
---

## 🚀 Установка и запуск
🚀🚀🚀 https://prometheus.io/download/?roistat_visit=1723043
```bash
# скачать последний релиз
wget https://github.com/prometheus/prometheus/releases/download/v2.43.0/prometheus-2.43.0.linux-amd64.tar.gz
tar xvf prometheus-*.tar.gz
cd prometheus-*.linux-amd64

# запустить с конфигом по умолчанию
./prometheus \
  --config.file=prometheus.yml \
  --storage.tsdb.path=data/

```
- ADVIT скрипты установки: [https://prometheus.io/  ](https://github.com/adv4000/prometheus)
  
По умолчанию веб‑интерфейс доступен на http://localhost:9090/.

---

## ⚙️ Структура конфигурации

```yaml
global:
  scrape_interval: 15s         # шаг сбора по умолчанию
  evaluation_interval: 30s     # шаг вычисления правил

scrape_configs:
  - job_name: 'node_exporter'   # произвольное имя задачи
    static_configs:
      - targets: ['localhost:9100']
```

- **global** — общие параметры  
- **scrape_configs** — разделы для задач сбора  
- **alerting** — настройки удалённых Alertmanager  
- **rule_files** — подключение файлов с правилами  

---

## 📈 Основные метрики

- `up` — статус доступности таргета (1 = up, 0 = down)  
- `node_cpu_seconds_total` — время работы CPU по состояниям  
- `node_memory_MemAvailable_bytes` — доступная память  
- `process_resident_memory_bytes` — память процесса  
- `http_requests_total` — счётчик HTTP‑запросов  

---

## 🧮 PromQL: ключевые операторы

| Оператор      | Описание                                     |
|---------------|----------------------------------------------|
| `rate(v[5m])` | скорость изменения счётчика за последние 5 мин |
| `increase(v[1h])` | приращение счётчика за час               |
| `sum by (job) (v)` | агрегировать по лейблу job             |
| `avg_over_time(v[30m])` | среднее значение за 30 мин         |
| `vector(42)`  | константный вектор                          |
| `label_replace()` | замена/добавление лейблов               |

---

## 🔍 Примеры запросов

```promql
# CPU загрузка всех ядер (в %)
100 - (avg by(instance)(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

# Ошибки HTTP (4xx+5xx) за 10 минут
sum(increase(http_requests_total{status=~"4..|5.."}[10m]))

# Использование памяти в процентах
(1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100

# Среднее время запроса по сервисам
histogram_quantile(
  0.95,
  sum(rate(http_request_duration_seconds_bucket[5m]))
    by (le, service)
)

# Топ-5 самых медленных хостов
topk(5, avg by(instance)(rate(node_cpu_seconds_total[mode!="idle"][5m])))
```

---

## 📝 Recording Rules (правила записи)

Запишите часто используемые запросы в кастомную метрику:

```yaml
groups:
  - name: recording_rules
    interval: 1m
    rules:
      - record: job:cpu_usage:avg5m
        expr: avg by(job)(rate(node_cpu_seconds_total[mode!="idle"][5m]))
      - record: job:request_errors:rate1m
        expr: sum(rate(http_requests_total{status=~"5.."}[1m])) by (job)
```

---

## 🚨 Alerting Rules (правила алертинга)

Пример алерта на высокую загрузку CPU:

```yaml
groups:
  - name: alert_rules
    rules:
      - alert: HighCPUUsage
        expr: job:cpu_usage:avg5m > 0.80
        for: 2m
        labels:
          severity: warning
        annotations:
          summary: "Высокая загрузка CPU на {{ $labels.instance }}"
          description: "Средняя загрузка CPU > 80% более 2 минут"
```

Не забудьте подключить Alertmanager:

```yaml
alerting:
  alertmanagers:
    - static_configs:
        - targets: ['alertmanager:9093']
```

---

## 📊 Интеграция с Grafana

1. Добавьте Prometheus как Data Source:  
   – URL: `http://<prometheus-host>:9090`  
   – Access: Server (default)  
2. Импортируйте готовые дашборды (ID из Grafana.com):  
   – [Node Exporter Full](https://grafana.com/grafana/dashboards/1860)  
   – [Linux System Monitoring](https://grafana.com/grafana/dashboards/1229)  
3. Создавайте собственные панели на основе PromQL-выражений.

---

## 🔗 Полезные ссылки

- Официальный сайт Prometheus: https://prometheus.io/  
- Документация PromQL: https://prometheus.io/docs/prometheus/latest/querying/basics/  
- Руководство по настройке Alertmanager: https://prometheus.io/docs/alerting/latest/alertmanager/  
- GitHub‑репозиторий: https://github.com/prometheus/prometheus  

---
