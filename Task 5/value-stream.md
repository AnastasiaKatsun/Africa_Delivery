@startuml
title Value Stream — Africa Delivery MVP (с цветовой кодировкой Build/Buy/Partner)

' Цветовая легенда
legend right
  |= Тип |= Цвет |
  | Build (собственная разработка) | ##32CD32 |
  | Buy (покупка) | ##87CEEB |
  | Partner (партнёрский API) | ##FFA500 |
  | Внешний субъект / роль | ##D3D3D3 |
  | Решение / ветвление | Белый |
endlegend

' Стили
skinparam rectangle {
  BackgroundColor #FFFFFF
  BorderColor #333333
  FontColor #333333
}

' ============ ПОТОК ПОКУПАТЕЛЯ ============

rectangle "Покупатель" as customer #D3D3D3

rectangle "Вход: Реклама / Маркетинг" as marketing #87CEEB

rectangle "Открытие PWA / Приложения" as pwa_open #87CEEB

rectangle "Просмотр каталога" as catalog_view #32CD32
rectangle "Поиск и фильтрация" as search #87CEEB
rectangle "Карточка товара" as product_card #32CD32

rectangle "Оформление заказа (корзина)" as checkout #32CD32

' Платёжное ветвление
rectangle "Выбор способа оплаты" as payment_choice #FFFFFF

rectangle "M-Pesa (Кения)" as mpesa #FFA500
rectangle "Flutterwave (Нигерия)" as flutterwave #FFA500
rectangle "Paystack (ЮАР)" as paystack #FFA500

rectangle "Подтверждение оплаты" as payment_confirm #32CD32

' Логистика
rectangle "Создание заявки на доставку" as delivery_create #FFA500
rectangle "Sendbox (Нигерия)" as sendbox #FFA500
rectangle "Sendy (Кения)" as sendy #FFA500

rectangle "Отправка уведомлений" as notify #FFA500
rectangle "SMS / WhatsApp / USSD" as sms_whatsapp #FFA500

rectangle "Отслеживание доставки (track & trace)" as tracking #FFA500

rectangle "Получение заказа покупателем" as receive #D3D3D3

' ============ ПОТОК ПРОДАВЦА ============

rectangle "Продавец (МСБ)" as seller #D3D3D3

rectangle "Онбординг через WhatsApp / Менеджер" as seller_onboarding #32CD32

rectangle "Загрузка товаров (CSV / вручную)" as seller_upload #32CD32

rectangle "Модерация каталога" as moderation #32CD32

rectangle "Получение выплаты (T+1)" as seller_payout #FFA500

rectangle "Банковский счёт продавца" as seller_bank #D3D3D3

' ============ АНАЛИТИКА ДЛЯ CEO ============

rectangle "Дашборд CEO\n(регистрации, заказы, выручка)" as ceo_dashboard #87CEEB

' ============ СВЯЗИ ============

customer --> marketing
marketing --> pwa_open
pwa_open --> catalog_view
pwa_open --> search
search --> product_card
catalog_view --> product_card
product_card --> checkout

checkout --> payment_choice

payment_choice --> mpesa
payment_choice --> flutterwave
payment_choice --> paystack

mpesa --> payment_confirm
flutterwave --> payment_confirm
paystack --> payment_confirm

payment_confirm --> delivery_create

delivery_create --> sendbox
delivery_create --> sendy

payment_confirm --> notify
sendbox --> notify
sendy --> notify

notify --> sms_whatsapp
sms_whatsapp --> tracking

tracking --> receive

' Поток продавца
seller --> seller_onboarding
seller_onboarding --> seller_upload
seller_upload --> moderation
moderation --> catalog_view

payment_confirm --> seller_payout
seller_payout --> seller_bank

' Аналитика
checkout --> ceo_dashboard
payment_confirm --> ceo_dashboard
receive --> ceo_dashboard

@enduml

| **Цвет** | **Значение** | **Примеры на диаграмме** |
| --- | --- | --- |
| **Зелёный** | **Build** — собственная разработка | Каталог, корзина, онбординг продавца |
| **Голубой** | **Buy** — покупка готового решения | Поиск, PWA, дашборд CEO |
| **Оранжевый** | **Partner** — партнёрский API | M-Pesa, Sendbox, SMS/WhatsApp |
| **Серый** | Внешний субъект | Покупатель, продавец, банк |
| **Белый** | Решение / ветвление | Выбор способа оплаты |