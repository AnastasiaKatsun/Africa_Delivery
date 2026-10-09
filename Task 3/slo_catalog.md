**apiVersion**: openslo/v1
**kind**: SLO
**metadata**:
**name**: catalog-service-availability
**service**: catalog-api
**spec**:
**objectives**:
- **target**: 99.9
**window**: 30d
**objectives**:
- **metric**: catalog_http_requests_total
**status**: "200"
**timeWindow**: 30d

# SLO-контракт: Сервис «Каталог»

## SLI (Service Level Indicators)

| SLI | Как измеряется | Метод сбора |
| --- | --- | --- |
| Доступность | % успешных HTTP-запросов (2xx/5xx) | Prometheus: `catalog_http_requests_total{status="2xx"}` / `{status="5xx"}` |
| Задержка P99 | 99-й перцентиль времени ответа | Prometheus: `catalog_http_request_duration_seconds{quantile="0.99"}` |
| Пропускная способность | Запросов/мин | Prometheus: `catalog_http_requests_total` / rate |

## SLO (Service Level Objectives)

| Показатель | Целевое значение | Окно |
| --- | --- | --- |
| Доступность | **99,9%** (≈ 43 мин простоя/мес) | 30 дней |
| Задержка P99 | **≤ 500 мс** | 30 дней |
| Пропускная способность | **≥ 500 запросов/мин** | 30 дней |

## SLA (Service Level Agreement)

* **Гарантия бизнесу:** каталог доступен для просмотра в любое время.
* **При нарушении:** инцидент PA1 (критический), дежурный инженер уведомляется в течение 5 минут, эскалация на CTO через 30 минут.

## Бюджет ошибок

* 100% − 99,9% = **0,1%** = **43,2 мин/мес**.
* Если бюджет ошибок исчерпан — запрет на релизы новых функций, только фиксы стабильности.