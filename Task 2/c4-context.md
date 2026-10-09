## **4. C4 Context Diagram**

@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

title System Context Diagram — Africa Delivery MVP

Person(покупатель, "Покупатель", "Пользователь с Android/iOS или кнопочным телефоном")
Person(продавец, "Продавец", "МСБ, работает через WhatsApp/USSD")

System(platform, "Africa Delivery Platform", "Маркетплейс: каталог, заказы, оплата, доставка, уведомления, аналитика")

System_Ext(mpesa, "M-Pesa API", "Мобильные деньги (Кения)")
System_Ext(flutterwave, "Flutterwave API", "Платёжный шлюз (Нигерия)")
System_Ext(paystack, "Paystack API", "Альтернативный платёжный шлюз")
System_Ext(sendbox, "Sendbox API", "Логистика (Нигерия)")
System_Ext(sendy, "Sendy API", "Логистика (Кения)")
System_Ext(whatsapp, "WhatsApp Business API", "Канал для продавцов и уведомления")
System_Ext(sms, "SMS/USSD Gateway", "Офлайн-канал для сельских районов")
System_Ext(live, "Live-vendor", "Стриминговая платформа для live-покупок")

Rel(покупатель, platform, "Использует PWA/App/USSD")
Rel(продавец, platform, "Управляет товарами, получает выплаты")
Rel(platform, mpesa, "Инициирует оплату, проверяет статус")
Rel(platform, flutterwave, "Инициирует оплату, проверяет статус")
Rel(platform, paystack, "Резервный платёжный шлюз")
Rel(platform, sendbox, "Создаёт доставку, получает статус")
Rel(platform, sendy, "Создаёт доставку, получает статус")
Rel(platform, whatsapp, "Отправляет уведомления, чаты")
Rel(platform, sms, "Отправляет SMS/USSD-запросы")
Rel(platform, live, "Запускает live-стримы")

@enduml