@startuml C4_Context
!include <C4/C4_Context>

LAYOUT_TOP_DOWN()

Person(customer, "Покупатель", "Городские и пригородные пользователи 22–40 лет, смартфоны < $150")
Person(seller, "Продавец", "Малый и средний бизнес, часть без интернета")
Person(admin, "Администратор", "Модерация, KYC, разбор споров")

System(africadelivery, "Africa Delivery Platform", "MVP: каталог, заказ, оплата, логистика, аналитика")

System_Ext(mpesa, "M-Pesa API", "Платёжный провайдер (Нигерия)")
System_Ext(flutterwave, "Flutterwave API", "Мультивалютные платежи и переводы")
System_Ext(paystack, "Paystack API", "Приём карт и банковских переводов")
System_Ext(sendbox, "Sendbox API", "Логистика и пункты выдачи (Нигерия)")
System_Ext(sendy, "Sendy API", "Доставка последней мили (Кения)")
System_Ext(whatsapp, "WhatsApp Business API", "Канал для продавцов без личного кабинета")
System_Ext(smsussd, "SMS/USSD-шлюз", "Заказы и уведомления в зонах без интернета")

Rel(customer, africadelivery, "Просматривает каталог, оформляет заказ, оплачивает, отслеживает доставку", "HTTPS, PWA, нативное приложение")
Rel(seller, africadelivery, "Регистрируется, загружает товары, получает выплаты, отслеживает заказы", "WhatsApp, USSD, веб-интерфейс")
Rel(admin, africadelivery, "Модерирует контент, управляет пользователями, разбирает споры", "Веб-панель администратора")

Rel(africadelivery, mpesa, "Отправляет запросы на оплату, получает статусы транзакций", "REST/JSON, webhook")
Rel(africadelivery, flutterwave, "Интеграция мультивалютных платежей и переводов", "REST/JSON, webhook")
Rel(africadelivery, paystack, "Приём карт и переводов, обработка возвратов", "REST/JSON, webhook")
Rel(africadelivery, sendbox, "Передача заказов, получение статусов доставки, ETA", "REST/JSON")
Rel(africadelivery, sendy, "Передача заказов, отслеживание курьера, зоны покрытия", "REST/JSON")
Rel(africadelivery, whatsapp, "Отправка уведомлений, приём заявок от продавцов", "WhatsApp Cloud API")
Rel(africadelivery, smsussd, "Отправка SMS-уведомлений, приём USSD-заказов", "SMS/USSD API")

@enduml