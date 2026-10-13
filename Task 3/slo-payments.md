**apiVersion**: openslo/v1
**kind**: SLO
**metadata**:
**name**: payment-service-availability
**service**: payment-api
**spec**:
**objectives**:
- **target**: 99.95
**window**: 30d

# SLO-контракт: Сервис «Платежи»

## SLI (Service Level Indicators)

| SLI | Как измеряется | Метод сбора |
| --- | --- | --- |
| Доступность | % успешных вызовов API M-Pesa/Flutterwave | Prometheus: payment_gateway_requests_total{status="success"} |
| Задержка P99 | Время подтверждения платежа | Prometheus: payment_gateway_request_duration_seconds{quantile="0.99"} |
| Процент отказов | % ошибок 5xx от платёжного шлюза | Prometheus: payment_gateway_errors_total |

## SLO (Service Level Objectives)

| Показатель | Целевое значение | Окно |
| --- | --- | --- |
| Доступность | **99,95%** (≈ 21,6 мин/мес) | 30 дней |
| Задержка P99 | **≤ 800 мс** | 30 дней |
| Процент отказов (5xx) | **≤ 0,05%** от всех вызовов | 30 дней |

## SLA (Service Level Agreement)

* **Гарантия бизнесу:** пользователь может совершить оплату в любое время.
* **При нарушении:** инцидент PA1 (критический), эскалация на CTO через 15 минут, на CEO — через 60 минут.

## Бюджет ошибок

* 100% − 99,95% = **0,05%** = **21,6 мин/мес**.
* Особый случай: сбой платёжного API (M-Pesa) — это **внешняя зависимость**. Мы не можем контролировать uptime провайдера. Поэтому вводим **компенсационный SLO**: если M-Pesa недоступен \> 5 минут, автоматически переключаем трафик на Flutterwave (или наоборот).