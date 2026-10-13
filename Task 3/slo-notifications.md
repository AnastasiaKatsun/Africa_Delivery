**apiVersion**: openslo/v1
**kind**: SLO
**metadata**:
**name**: notification-service-availability
**service**: notification-api
**spec:
objectives**:
- **target**: 99.5
**window**: 30d

# SLO-контракт: Сервис «Уведомления»

## SLI (Service Level Indicators)

| SLI | Как измеряется | Метод сбора |
| --- | --- | --- |
| Доставка | % сообщений, доставленных за 60 сек | RabbitMQ/Redis: notification_delivered_total / notification_sent_total |
| Задержка | Время от события до доставки | Prometheus: notification_delivery_duration_seconds{quantile="0.99"} |
| Ошибки | % ошибок при отправке (SMS/WhatsApp) | Prometheus: notification_errors_total |

## SLO (Service Level Objectives)

| Показатель | Целевое значение | Окно |
| --- | --- | --- |
| Доставка за 60 сек | **≥ 99,5%** | 30 дней |
| Задержка P99 | **≤ 5 сек** | 30 дней |
| Ошибки при отправке | **≤ 0,5%** | 30 дней |

## SLA (Service Level Agreement)

* **Гарантия бизнесу:** пользователь получает уведомление о статусе заказа в течение 60 секунд.
* **При нарушении:** инцидент PA2 (высокий), эскалация на дежурного в течение 15 минут.

## Бюджет ошибок

* 100% − 99,5% = **0,5%** = **216 мин/мес**.
* Уведомления — **самый терпимый сервис** к отказам (пользователь может проверить статус сам), но критичный для доверия.