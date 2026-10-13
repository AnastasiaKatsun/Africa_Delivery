## **6. Стратегия наблюдаемости**

### **Четыре золотых сигнала**

| **Сигнал** | **Метрика** | **Инструмент** | **Связь с SLI** |
| --- | --- | --- | --- |
| **Задержка (Latency)** | **`http_request_duration_seconds`** (P50, P95, P99) | Prometheus + Grafana | P99 ≤ 500 мс (каталог), ≤ 800 мс (платежи) |
| **Трафик (Traffic)** | **`http_requests_total`** (rate/мин) | Prometheus | Пропускная способность ≥ 500 запросов/мин |
| **Ошибки (Errors)** | **`http_errors_total{status="5xx"}`** | Prometheus + Sentry | ≤ 0,1% ошибок 5xx |
| **Насыщение (Saturation)** | CPU, Memory, Disk I/O, Redis Memory | Prometheus + Node Exporter | ≥ 80% использования → алерт |

### **Схема сбора метрик**
@startuml
title Observability Architecture — Africa Delivery

[Browser / Mobile App] --> [CDN (Cloudflare)]
[CDN] --> [Kubernetes Cluster (K3s)]
[Kubernetes Cluster] --> [Application Pods (Go)]
[Application Pods] --> [PostgreSQL]
[Application Pods] --> [Redis]
[Application Pods] --> [External APIs (M-Pesa, Sendbox)]

[Application Pods] --> [OpenTelemetry SDK]
[OpenTelemetry SDK] --> [OpenTelemetry Collector]
[OpenTelemetry Collector] --> [Prometheus]
[OpenTelemetry Collector] --> [Grafana]
[OpenTelemetry Collector] --> [Loki]
[OpenTelemetry Collector] --> [Jaeger]

[Prometheus] --> [Grafana]
[Loki] --> [Grafana]
[Jaeger] --> [Grafana]
[Prometheus] --> [Alertmanager]
[Alertmanager] --> [Slack / SMS / PagerDuty]

@enduml

### **Инструменты и их назначение**

| **Инструмент** | **Назначение** | **Метрики/Логи/Трейсы** |
| --- | --- | --- |
| **Prometheus** | Сбор метрик, алертинг | Метрики: HTTP, БД, Redis, Go runtime |
| **Grafana** | Визуализация, дашборды | Дашборды: «Каталог», «Платежи», «Инфраструктура», «Бюджет ошибок» |
| **Loki** | Агрегация логов | Логи: приложения, nginx, PostgreSQL |
| **Jaeger** | Распределённая трассировка | Трейсы: запрос от API до БД и внешних вызовов |
| **OpenTelemetry** | Стандарт сбора телеметрии | SDK в Go-сервисах, экспорт в Collector |
| **Alertmanager** | Маршрутизация алертов | Алерты: критичность PA1/PA2, эскалация |
| **Sentry** | Отслеживание ошибок приложений | stack trace, контекст пользователя |

### **Связь метрик с SLI**

| **SLI** | **Метрика в Prometheus** | **Алерт при нарушении** | **Эскалация** |
| --- | --- | --- | --- |
| Доступность каталога (99,9%) | **catalog_http_requests_total{status="5xx"}** / **{status="2xx"}** \> 0,1% | PA1 | CTO, CEO |
| Задержка P99 каталога ≤ 500 мс | **catalog_http_request_duration_seconds{quantile="0.99"}** \> 0,5 | PA1 | CTO |
| Доставка уведомлений ≥ 99,5% | **notification_delivered_total** / **notification_sent_total** \< 0,995 | PA2 | Дежурный инженер |
| Ошибки платёжного шлюза ≤ 0,05% | **payment_gateway_errors_total** / **payment_gateway_requests_total** \> 0,0005 | PA1 | CTO, CFO |

## **Экономическое обоснование уровня доступности (для CFO)**

| **Уровень доступности** | **Простой в год** | **Потери (по оценке инвестора: $12 000/мин)** | **Стоимость обеспечения (резервирование)** | **Вывод** |
| --- | --- | --- | --- | --- |
| **99,9%** | 8,76 часов = 525,6 мин | **$6,3M/год** | $500–1 000/мес (резервный VPS + бэкапы) | Приемлемо для MVP |
| **99,95%** | 4,38 часов = 262,8 мин | **$3,15M/год** | $2 000–3 000/мес (активная реплика, multi-AZ) | Рассмотреть после роста |
| **99,99%** | 52,6 мин | **$631k/год** | $8 000–12 000/мес (active-active, мульти-регион) | Нерентабельно для старта |

**Вывод:** Останавливаемся на **99,9%** для MVP. Это соответствует рассчитанному бюджету ошибок и позволяет уложиться в лимит CFO ($5k/мес), не рискуя запуском. Переход на 99,95% и выше — после достижения 100k DAU, когда потери от простоя превысят стоимость резервирования.