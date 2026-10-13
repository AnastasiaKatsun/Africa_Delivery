## **3. Сервисы по Capability**

| **Capability** | **Продукт / Сервис** | **Владелец** | **Внешняя зависимость** |
| --- | --- | --- | --- |
| Управление каталогом | Модуль **catalog** (Go) | Go-разработчик 1 | Нет |
| Поиск | Elasticsearch/Meilisearch | Go-разработчик 1 | Нет |
| Оформление заказа | Модуль **order** (Go) | Go-разработчик 2 | Каталог |
| Платежи | Модуль **payment** (Go) + API M-Pesa/Flutterwave/Paystack | Go-разработчик 2 | M-Pesa, Flutterwave, Paystack |
| Выплаты продавцам | API Flutterwave (Payouts) | Go-разработчик 2 | Flutterwave |
| Логистика | Модуль **logistics** (Go) + API Sendbox/Sendy | Backend | Sendbox, Sendy |
| SMS-уведомления | API Africastalking/Twilio | DevOps | SMS-агрегатор |
| WhatsApp | WhatsApp Business API (Meta) | Backend | Meta |
| USSD | USSD-шлюз Africastalking | DevOps | Africastalking |
| Онбординг продавцов | Модуль **onboarding** (Go) | Backend | WhatsApp |
| ЛК продавца | Фронтенд (Vue/React) | Frontend | API каталога/заказов |
| Клиентский сервис | Zendesk/Intercom (SaaS) | CEO | SaaS |
| Дашборд CEO | Grafana + SQL | DevOps | PostgreSQL |
| IAM | Keycloak | DevOps | Нет |
| Consent Manager | Модуль **consent** (Go) | Backend | Нет |
| Мониторинг | Prometheus + Grafana + Loki + Jaeger | DevOps | Нет |
