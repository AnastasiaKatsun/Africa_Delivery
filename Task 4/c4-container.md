@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

title Container Diagram — Africa Delivery (по регионам)

Person(покупатель_нг, "Покупатель (Нигерия)", "Пользователь PWA/USSD")
Person(покупатель_кн, "Покупатель (Кения)", "Пользователь PWA/USSD")
Person(покупатель_юар, "Покупатель (ЮАР)", "Пользователь PWA")
Person(продавец, "Продавец (МСБ)", "Управляет товарами через WhatsApp")

System_Boundary(платформа_нг, "Africa Delivery — Нигерия (VPS Лагос)") {
    Container(api_нг, "API Gateway", "Go", "REST API для заказов, каталога, оплаты")
    Container(app_нг, "Монолит (Go)", "Go", "Бизнес-логика: каталог, заказы, платежи, логистика")
    ContainerDb(db_нг, "PostgreSQL", "БД", "PII граждан Нигерии, заказы, платежи")
    Container(redis_нг, "Redis", "Кэш", "Сессии, корзина, кэш каталога")
}

System_Boundary(платформа_кн, "Africa Delivery — Кения (VPS Найроби)") {
    Container(api_кн, "API Gateway", "Go", "REST API для заказов, каталога, оплаты")
    Container(app_кн, "Монолит (Go)", "Go", "Бизнес-логика: каталог, заказы, платежи, логистика")
    ContainerDb(db_кн, "PostgreSQL", "БД", "PII граждан Кении, заказы, платежи")
    Container(redis_кн, "Redis", "Кэш", "Сессии, корзина, кэш каталога")
}

System_Boundary(платформа_юар, "Africa Delivery — ЮАР (GCP africa-south1)") {
    Container(api_юар, "API Gateway", "Go", "REST API для заказов, каталога, оплаты")
    Container(app_юар, "Монолит (Go)", "Go", "Бизнес-логика: каталог, заказы, платежи, логистика")
    ContainerDb(db_юар, "PostgreSQL (CMEK)", "БД", "PII граждан ЮАР, заказы, платежи")
    Container(redis_юар, "Redis", "Кэш", "Сессии, корзина, кэш каталога")
}

System_Ext(mpesa, "M-Pesa API", "Платежи (Кения)")
System_Ext(flutterwave, "Flutterwave API", "Платежи (Нигерия)")
System_Ext(paystack, "Paystack API", "Платежи (ЮАР)")
System_Ext(sendbox, "Sendbox API", "Логистика (Нигерия)")
System_Ext(sendy, "Sendy API", "Логистика (Кения)")
System_Ext(whatsapp, "WhatsApp Business API", "Уведомления, онбординг продавцов")
System_Ext(sms, "SMS/USSD Gateway", "Офлайн-канал")

Rel(покупатель_нг, api_нг, "HTTPS")
Rel(покупатель_кн, api_кн, "HTTPS")
Rel(покупатель_юар, api_юар, "HTTPS")
Rel(продавец, whatsapp, "Управляет товарами")

Rel(app_нг, db_нг, "SQL")
Rel(app_нг, redis_нг, "Redis Protocol")
Rel(app_нг, flutterwave, "REST API")
Rel(app_нг, sendbox, "REST API")
Rel(app_нг, sms, "REST API")

Rel(app_кн, db_кн, "SQL")
Rel(app_кн, redis_кн, "Redis Protocol")
Rel(app_кн, mpesa, "REST API")
Rel(app_кн, sendy, "REST API")
Rel(app_кн, sms, "REST API")

Rel(app_юар, db_юар, "SQL")
Rel(app_юар, redis_юар, "Redis Protocol")
Rel(app_юар, paystack, "REST API")
Rel(app_юар, sms, "REST API")

Rel(app_нг, whatsapp, "REST API")
Rel(app_кн, whatsapp, "REST API")
Rel(app_юар, whatsapp, "REST API")

@enduml