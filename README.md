# Microservices E-Commerce Platform

Современная микросервисная платформа интернет-магазина, построенная на событийно-ориентированной архитектуре (Event-Driven Architecture). Проект демонстрирует взаимодействие независимых сервисов через RabbitMQ и Kafka, использование API Gateway, Docker Compose и отдельных баз данных для каждого сервиса.

---



### Основные компоненты

| Компонент | Назначение |
|-----------|------------|
| Browser / Frontend | Пользовательский интерфейс |
| Nginx | Reverse Proxy, SSL, раздача статических файлов |
| API Gateway | Единая точка входа для клиентов |
| Catalog Service | Работа с каталогом товаров |
| Catalog DB | База данных каталога |
| Order Service | Создание и управление заказами |
| RabbitMQ | Обмен сообщениями между сервисами |
| Payment Service | Обработка платежей |
| Notification Service | Email / Telegram уведомления |
| Kafka | Поток событий для аналитики |
| Analytics Service | Анализ заказов и платежей |

---

# Возможности проекта

- Каталог товаров
- Создание заказов
- Асинхронная обработка заказов
- Обработка платежей
- Email/Telegram уведомления
- Событийная архитектура
- Потоковая аналитика
- Изолированные микросервисы
- Отдельные базы данных
- Контейнеризация Docker

---

# Архитектурная схема

```
Client
   │
   ▼
Nginx
   │
   ▼
API Gateway
   │
   ├──────────────► Catalog Service ───► Catalog DB
   │
   └──────────────► Order Service
                         │
                    Order Created
                         │
                         ▼
                     RabbitMQ
                    /         \
                   ▼           ▼
         Payment Service   Notification Service
               │                    │
               │                    ▼
               │            Email / Telegram
               │
      Payment Success / Failed
               │
               ▼
         Order Service

Payment Events
       │
       ▼
     Kafka
       │
       ▼
Analytics Service
```

---

# Используемые технологии

## Backend

- Java
- Spring Boot
- Spring Cloud Gateway
- Spring Data JPA
- Spring Security (при необходимости)

## Messaging

- RabbitMQ
- Apache Kafka

## Databases

- PostgreSQL
- Отдельная БД для каждого сервиса

## Infrastructure

- Docker
- Docker Compose
- Nginx

---

# Структура проекта

```
microservices-course/
│
├── api-gateway/
├── catalog-service/
├── order-service/
├── payment-service/
├── notification-service/
├── analytics-service/
│
├── docker-compose.yml
├── nginx/
├── docs/
│   └── architecture.png
│
└── README.md
```

---

# Поток обработки заказа

### 1. Просмотр товаров

Пользователь открывает каталог товаров через Frontend.

```
Frontend
    │
API Gateway
    │
Catalog Service
    │
Catalog DB
```

---

### 2. Создание заказа

После оформления заказа:

```
Frontend
      │
API Gateway
      │
Order Service
```

Order Service:

- сохраняет заказ;
- публикует событие **OrderCreated** в RabbitMQ.

---

### 3. Обработка платежа

RabbitMQ передает сообщение в Payment Service.

Payment Service:

- выполняет оплату;
- публикует событие

```
payment.success
или
payment.failed
```

---

### 4. Обновление заказа

Order Service получает результат оплаты.

Если успешно:

```
Order Status = PAID
```

Иначе

```
Order Status = FAILED
```

---

### 5. Отправка уведомлений

Notification Service подписан на события RabbitMQ.

Поддерживаются:

- Email
- Telegram
- Логирование

---

### 6. Аналитика

Payment Service отправляет события в Kafka.

Analytics Service собирает статистику:

- количество заказов;
- успешные платежи;
- неудачные платежи;
- оборот магазина.

---

# Запуск проекта

## Запуск Docker

```bash
docker-compose up --build
```

После запуска будут доступны:

- API Gateway
- RabbitMQ
- Kafka
- Все микросервисы
- PostgreSQL

---

# Преимущества архитектуры

✔ Независимое масштабирование сервисов

✔ Асинхронная обработка сообщений

✔ Отказоустойчивость

✔ Простое добавление новых сервисов

✔ Изоляция баз данных

✔ Высокая производительность

✔ Event-Driven Architecture

---

# Возможные улучшения

- JWT авторизация
- Kubernetes
- Prometheus
- Grafana
- ELK Stack
- Redis Cache
- CI/CD (GitHub Actions)
- OpenAPI / Swagger
- Distributed Tracing (Zipkin)

---

# Автор

Разработано в рамках учебного проекта по изучению микросервисной архитектуры.
