@startuml
title Deployment Diagram — Africa Delivery (мульти-регион)

node "Нигерия — VPS Лагос (Layer3)" {
    node "K3s Cluster (2 ноды)" {
        component "API Gateway" as nginx_ng
        component "App (Go)" as app_ng
        component "Redis" as redis_ng
    }
    database "PostgreSQL (PII Нигерии)" as db_ng
}

node "Кения — VPS Найроби (Safaricom)" {
    node "K3s Cluster (2 ноды)" {
        component "API Gateway" as nginx_ke
        component "App (Go)" as app_ke
        component "Redis" as redis_ke
    }
    database "PostgreSQL (PII Кении)" as db_ke
}

node "ЮАР — GCP africa-south1 (Йоханнесбург)" {
    node "GKE Cluster (3 ноды)" {
        component "API Gateway" as nginx_za
        component "App (Go)" as app_za
        component "Redis" as redis_za
    }
    database "Cloud SQL PostgreSQL (PII ЮАР, CMEK)" as db_za
}

node "Глобально — Cloudflare CDN" {
    node "CDN Edge" as cdn
}

node "Резервный регион — AWS af-south-1 (Кейптаун)" {
    database "Резервная реплика БД ЮАР" as db_za_backup
}

node "S3 (Frankfurt, eu-central-1)" {
    database "Анонимизированная аналитика" as analytics
}

node "Внешние сервисы" {
    database "M-Pesa API" as mpesa
    database "Flutterwave API" as flutterwave
    database "Paystack API" as paystack
    database "Sendbox API" as sendbox
    database "Sendy API" as sendy
    database "WhatsApp Business API" as whatsapp
    database "SMS/USSD Gateway" as sms
}

cdn --> nginx_ng
cdn --> nginx_ke
cdn --> nginx_za

app_ng --> db_ng
app_ng --> redis_ng
app_ng --> flutterwave
app_ng --> sendbox
app_ng --> whatsapp
app_ng --> sms

app_ke --> db_ke
app_ke --> redis_ke
app_ke --> mpesa
app_ke --> sendy
app_ke --> whatsapp
app_ke --> sms

app_za --> db_za
app_za --> redis_za
app_za --> paystack
app_za --> whatsapp
app_za --> sms

db_za --> db_za_backup : "асинхронная репликация"
db_ng --> analytics : "анонимизированные данные (ETL)"
db_ke --> analytics : "анонимизированные данные (ETL)"
db_za --> analytics : "анонимизированные данные (ETL)"

@enduml