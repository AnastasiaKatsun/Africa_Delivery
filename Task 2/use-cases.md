## **5. Use Case Diagram (ключевые сценарии MVP)**

@startuml
left to right direction
actor "Покупатель" as Customer
actor "Продавец" as Seller
actor "CEO" as CEO
actor "Курьер" as Courier

rectangle "Africa Delivery MVP" {
    usecase "Просмотр каталога (5000 SKU)" as UC1
    usecase "Поиск и фильтрация" as UC2
    usecase "Оформление заказа" as UC3
    usecase "Оплата M-Pesa/Flutterwave" as UC4
    usecase "Получение уведомлений (SMS/WhatsApp)" as UC5
    usecase "Отслеживание доставки" as UC6
    usecase "Регистрация продавца через WhatsApp" as UC7
    usecase "Загрузка товаров (CSV/вручную)" as UC8
    usecase "Получение выплат (T+1)" as UC9
    usecase "Просмотр дашборда (заказы, выручка)" as UC10
    usecase "Подтверждение доставки (подпись/SMS)" as UC11
}

Customer --> UC1
Customer --> UC2
Customer --> UC3
Customer --> UC4
Customer --> UC5
Customer --> UC6
Seller --> UC7
Seller --> UC8
Seller --> UC9
CEO --> UC10
Courier --> UC11

@enduml