# Мониторинг Prometheus + Grafana + Node Exporter

Решение домашнего задания «Средство визуализации Grafana».

## Быстрый старт

```bash
# Клонировать репозиторий
git clone https://github.com/your-username/monitoring.git
cd monitoring

# Запустить стек
docker-compose up -d
```

**Доступ к сервисам:**
- **Grafana**: http://localhost:3000 (admin/admin)
- **Prometheus**: http://localhost:9090
- **Node Exporter**: http://localhost:9100/metrics
- **Alertmanager**: http://localhost:9093

## PromQL запросы для Dashboard

| Метрика | PromQL |
|---------|--------|
| CPU Usage (%) | `100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)` |
| Load Average 1/5/15 | `node_load1`, `node_load5`, `node_load15` |
| Free Memory (MB) | `node_memory_MemFree_bytes / 1024 / 1024` |
| Free Disk Space (GB) | `node_filesystem_avail_bytes{mountpoint="/"} / 1024 / 1024 / 1024` |

## Alert правила

Файл: `alerts/alert-rules.yml`

- **HighCPUUsage** (>80% за 5 мин)
- **HighLoadAverage** (LA1 > 70% от числа CPU)
- **LowMemory** (<512MB)
- **LowDiskSpace** (<10%)

## Telegram уведомления

Настройте `alertmanager/alertmanager.yml`:

```yaml
receivers:
  - name: 'telegram'
    telegram_configs:
      - bot_token: 'YOUR_BOT_TOKEN'
        chat_id: YOUR_CHAT_ID
```

## Структура репозитория

```
monitoring/
├── docker-compose.yml
├── prometheus/
│   └── prometheus.yml
├── alerts/
│   └── alert-rules.yml
├── alertmanager/
│   └── alertmanager.yml
└── grafana/
    └── provisioning/
        └── datasources/
            └── datasource.yml
```

## Команды управления

```bash
docker-compose up -d      # Запуск
docker-compose down       # Остановка
docker-compose logs -f    # Логи
```

## Скриншоты

- [Datasource в Grafana](screenshots/datasource.png)
- [Dashboard](screenshots/dashboard.png)
- [Telegram уведомление](screenshots/telegram.png)

## JSON Dashboard

Экспортированный дашборд: [dashboard/node-exporter-dashboard.json](dashboard/node-exporter-dashboard.json)