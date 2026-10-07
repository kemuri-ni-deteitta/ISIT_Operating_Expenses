<div align="center">

# Курсовая работа МГТУ "СТАНКИН" по предемету Информационные системы и технологии.
 
# ISIT — Operational Expenses Information System

### Информационная система анализа операционных затрат холдинга

Full-stack приложение для централизованного учёта, управления и анализа операционных расходов организации.

![Rust](https://img.shields.io/badge/Rust-1.81-000000?logo=rust&logoColor=white)
![Axum](https://img.shields.io/badge/Axum-0.7-000000)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-16-000000?logo=nextdotjs&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)

</div>

---

## О проекте

**ISIT** — информационная система для централизованного учёта и анализа операционных затрат организации.

Система предназначена для того, чтобы собрать данные о расходах подразделений в едином информационном пространстве и предоставить инструменты для их регистрации, классификации, контроля и анализа.

Основные задачи системы:

- централизованный учёт операционных затрат;
- распределение расходов по подразделениям;
- классификация затрат по категориям;
- ведение источников финансирования;
- управление пользователями;
- отслеживание статусов расходов;
- получение аналитики по накопленным данным;
- подготовка инфраструктуры для хранения документов и согласования расходов.

Общий поток данных:

```text
Пользователь
     │
     ▼
Next.js Frontend
     │
     │ REST API
     ▼
Rust / Axum Backend
     │
     ▼
Repository Layer
     │
     ▼
SQLx
     │
     ▼
PostgreSQL
```

---

# Основные возможности

## Учёт затрат

Система позволяет создавать и хранить записи об операционных расходах.

Для каждой записи могут храниться:

```text
Подразделение
Категория затрат
Сумма
Валюта
Дата
Описание
Источник финансирования
Исполнитель
Статус
Автор записи
```

Основные операции:

```text
Create
Read
Update
Delete
```

Поддерживаются статусы:

```text
pending
approved
rejected
paid
```

---

## Управление подразделениями

В системе реализован отдельный справочник подразделений.

Подразделение содержит:

```text
code
name
parent_id
active
```

Поле:

```text
parent_id
```

позволяет строить иерархическую структуру организации.

Например:

```text
Компания
│
├── Финансовый департамент
│   ├── Бухгалтерия
│   └── Планово-экономический отдел
│
├── IT-департамент
│   ├── Разработка
│   └── Инфраструктура
│
└── Отдел продаж
```

Для подразделений реализованы операции создания, редактирования, получения списка и удаления.

---

# Категории затрат

Расходы классифицируются при помощи отдельного справочника категорий.

Категория имеет структуру:

```text
code
name
parent_id
active
```

Как и подразделения, категории поддерживают иерархию.

Это позволяет создавать структуры вида:

```text
IT-расходы
│
├── Оборудование
├── Программное обеспечение
└── Облачные сервисы

Административные расходы
│
├── Аренда
├── Канцелярия
└── Коммунальные услуги
```

---

# Источники финансирования

В системе реализован отдельный справочник:

```text
funding_sources
```

Источник финансирования содержит:

```text
code
name
active
```

Например:

```text
Внутренний бюджет
Инвестиционный бюджет
Проектное финансирование
```

Источник может быть связан с конкретной записью о затратах.

---

# Управление пользователями

В системе реализован REST API для управления пользователями.

Поддерживаются:

- получение списка пользователей;
- создание;
- изменение;
- удаление.

В базе данных также предусмотрены:

```text
users
roles
user_roles
```

что позволяет в дальнейшем реализовать полноценную ролевую модель доступа.

---

# Аналитика и отчётность

Frontend содержит отдельный раздел:

```text
Аналитика и отчётность
```

Текущая реализация получает данные о затратах через REST API и агрегирует их по категориям.

Пользователь может выбрать тип визуализации:

```text
Круговая диаграмма
Столбчатая диаграмма
Линейный график
```

Для визуализации используется:

```text
Recharts
+
Chakra UI Charts
```

Поддерживается выбор периода:

```text
Последние 7 дней
Последние 30 дней
Текущий месяц
Всё время
```

Схема обработки:

```text
GET /api/v1/expenses
        │
        ▼
Получение расходов
        │
        ▼
Группировка по category_id
        │
        ▼
Суммирование расходов
        │
        ▼
Фильтрация по периоду
        │
        ▼
Chart
```

Можно включать и отключать отдельные категории непосредственно в легенде графика.

---

# Технологический стек

## Backend

| Технология | Назначение |
|---|---|
| **Rust** | основной язык backend |
| **Axum 0.7** | HTTP framework |
| **Tokio** | asynchronous runtime |
| **SQLx 0.8** | взаимодействие с PostgreSQL |
| **Serde** | сериализация JSON |
| **Tower HTTP** | CORS, tracing и HTTP middleware |
| **Tracing** | логирование |
| **Rust Decimal** | работа с денежными значениями |
| **UUID** | идентификаторы сущностей |

---

## Frontend

| Технология | Назначение |
|---|---|
| **Next.js 16** | frontend framework |
| **React 19** | UI |
| **TypeScript 5** | типизация |
| **Chakra UI 3** | UI-компоненты |
| **Redux Toolkit** | state management |
| **Axios** | HTTP client |
| **Recharts** | визуализация данных |
| **ApexCharts** | дополнительная библиотека графиков |
| **React Dropzone** | работа с загрузкой файлов |
| **date-fns** | работа с датами |

---

## Infrastructure

| Технология | Назначение |
|---|---|
| **PostgreSQL 16** | основная база данных |
| **Docker** | контейнеризация |
| **Docker Compose** | локальная инфраструктура |
| **MinIO** | S3-compatible хранилище |
| **MailHog** | локальная тестовая почта |

---

# Архитектура

Проект организован как monorepo:

```text
ISIT/
│
├── backend/
│   ├── migrations/
│   ├── src/
│   │   ├── db/
│   │   ├── domain/
│   │   ├── http/
│   │   ├── repositories/
│   │   └── main.rs
│   │
│   ├── Cargo.toml
│   └── Dockerfile
│
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   ├── components/
│   │   ├── shared/
│   │   ├── store/
│   │   └── styles/
│   │
│   ├── package.json
│   └── tsconfig.json
│
├── docker-compose.yml
└── README.md
```

---

# Backend Architecture

Backend разделён на несколько основных слоёв:

```text
HTTP Request
     │
     ▼
HTTP Handlers
     │
     ▼
Repository
     │
     ▼
SQLx
     │
     ▼
PostgreSQL
```

---

## Domain Layer

Директория:

```text
backend/src/domain/
```

содержит основные модели системы.

В том числе:

```text
Expense
User
Department
Category
FundingSource
```

Domain layer описывает структуры данных и DTO, используемые backend.

---

## HTTP Layer

Директория:

```text
backend/src/http/
```

содержит REST handlers.

Например:

```text
expense_handlers.rs
department_handlers.rs
category_handlers.rs
funding_source_handlers.rs
user_handlers.rs
```

Этот уровень отвечает за:

- получение HTTP request;
- десериализацию данных;
- вызов repository;
- формирование HTTP response;
- обработку HTTP status codes.

---

## Repository Layer

Работа с базой данных вынесена в:

```text
backend/src/repositories/
```

Например:

```text
ExpenseRepository
```

инкапсулирует SQL-запросы к таблице расходов.

Схема:

```text
Handler
   │
   ▼
ExpenseRepository
   │
   ▼
SQLx
   │
   ▼
PostgreSQL
```

Это позволяет отделить HTTP-логику от persistence layer.

---

# REST API

Backend работает по умолчанию на:

```text
http://localhost:8080
```

Основной prefix API:

```text
/api/v1
```

---

## Health Check

```http
GET /healthz
```

или:

```http
GET /api/v1/health
```

Пример ответа:

```json
{
  "status": "ok",
  "database": "ok"
}
```

Health endpoint проверяет не только работу HTTP-сервера, но и соединение с PostgreSQL.

---

# Expenses API

## Получение списка

```http
GET /api/v1/expenses
```

Поддерживается pagination:

```text
page
page_size
```

Пример:

```http
GET /api/v1/expenses?page=1&page_size=20
```

Response содержит:

```json
{
  "expenses": [],
  "total": 0,
  "page": 1,
  "page_size": 20
}
```

В HTTP-модели также предусмотрены параметры:

```text
department_id
category_id
status
date_from
date_to
```

Их полноценное применение на уровне repository находится в дальнейшей разработке.

---

## Получение расхода

```http
GET /api/v1/expenses/{id}
```

---

## Создание

```http
POST /api/v1/expenses
```

---

## Обновление

```http
PUT /api/v1/expenses/{id}
```

---

## Удаление

```http
DELETE /api/v1/expenses/{id}
```

При успешном удалении backend возвращает:

```http
204 No Content
```

---

# Reference Data API

## Departments

```text
GET    /api/v1/departments
POST   /api/v1/departments
PUT    /api/v1/departments/{id}
DELETE /api/v1/departments/{id}
```

## Categories

```text
GET    /api/v1/categories
POST   /api/v1/categories
PUT    /api/v1/categories/{id}
DELETE /api/v1/categories/{id}
```

## Funding Sources

```text
GET    /api/v1/funding-sources
POST   /api/v1/funding-sources
PUT    /api/v1/funding-sources/{id}
DELETE /api/v1/funding-sources/{id}
```

## Users

```text
GET    /api/v1/users
POST   /api/v1/users
PUT    /api/v1/users/{id}
DELETE /api/v1/users/{id}
```

---

# Структура базы данных

Миграции находятся в:

```text
backend/migrations/
```

и автоматически применяются при запуске backend:

```text
Backend start
     │
     ▼
Connect PostgreSQL
     │
     ▼
Run SQLx migrations
     │
     ▼
Start Axum Server
```

---

## Основные таблицы

```text
users
roles
user_roles

departments
categories
funding_sources

expenses

documents
approvals
audit_logs
```

---

## Основные связи

Упрощённая схема:

```text
users
  │
  │ created_by
  ▼
expenses
  │
  ├──────────────► departments
  │
  ├──────────────► categories
  │
  ├──────────────► funding source
  │
  ├──────────────► documents
  │
  └──────────────► approvals
```

---

# Expenses

Основная сущность системы:

```text
expenses
```

содержит информацию о расходе.

Основные поля:

```text
id
department_id
category_id
amount
currency
incurred_on
description
status
funding_source
performer
created_by
created_at
updated_at
```

Для денежных значений используется:

```sql
DECIMAL(15, 2)
```

что позволяет избежать ошибок округления, характерных для floating-point типов.

---

# Documents

Схема базы данных уже предусматривает хранение метаданных документов:

```text
documents
```

Основные поля:

```text
expense_id
filename
content_type
storage_key
size_bytes
sha256
uploaded_by
uploaded_at
```

Для непосредственного хранения файлов инфраструктурой проекта предусмотрен:

```text
MinIO
```

как S3-compatible object storage.

Полная интеграция загрузки документов через backend API является следующим этапом развития системы.

---

# Approvals

Таблица:

```text
approvals
```

предназначена для реализации процесса согласования затрат.

Она связывает:

```text
Expense
+
Approver
+
Decision
```

Decision может принимать значения:

```text
approved
rejected
```

Также предусмотрены комментарий и дата принятия решения.

---

# Audit Log

Для последующего аудита изменений предусмотрена таблица:

```text
audit_logs
```

Она может хранить:

```text
entity
entity_id
action
actor_id
diff_json
created_at
```

Это создаёт основу для отслеживания изменений бизнес-сущностей системы.

---

# Frontend

Frontend построен на:

```text
Next.js 16
+
React 19
+
TypeScript
+
Chakra UI
```

Основные разделы приложения:

```text
Главная

Учёт затрат

Аналитика и отчётность

Управление пользователями

Типы затрат

Источники финансирования

Подразделения
```

---

# Frontend Architecture

Основные директории:

```text
frontend/src/
│
├── app/
│   ├── expenses/
│   ├── analytics/
│   ├── departments/
│   ├── funding-sources/
│   ├── type_of_expenditure/
│   └── ...
│
├── components/
│   └── ui/
│
├── shared/
│   ├── api/
│   ├── constants/
│   ├── types/
│   └── utils/
│
├── store/
│
└── styles/
```

---

## API Layer

HTTP-взаимодействие вынесено в:

```text
frontend/src/shared/api/
```

Например:

```text
client.ts
expenses.ts
departments.ts
fundingSources.ts
reference.ts
users.ts
```

Базовый HTTP client создаётся через Axios.

По умолчанию frontend обращается к:

```text
http://localhost:8080
```

Адрес может быть изменён через:

```text
NEXT_PUBLIC_API_BASE_URL
```

---

# State Management

Для управления состоянием используется:

```text
Redux Toolkit
```

Store поддерживает динамическое подключение reducers через:

```text
ReducerManager
```

Общий подход:

```text
Component
    │
    ▼
Redux Action / Thunk
    │
    ▼
Redux Store
    │
    ▼
API Client
    │
    ▼
Backend
```

---

# Запуск проекта

## Требования

Для локального запуска понадобятся:

- Docker;
- Docker Compose;
- Rust toolchain;
- Node.js;
- npm.

---

# Клонирование

```bash
git clone https://github.com/kemuri-ni-deteitta/ISIT.git
```

Перейти в директорию:

```bash
cd ISIT
```

---

# Запуск инфраструктуры

PostgreSQL, MinIO и MailHog можно запустить через Docker Compose:

```bash
docker compose up -d postgres minio mailhog
```

Проверить состояние:

```bash
docker compose ps
```

---

## PostgreSQL

Доступен на:

```text
localhost:5432
```

Параметры локальной конфигурации:

```text
Database: isit
User:     isit
Password: isit
```

Подключение:

```bash
psql -h localhost -p 5432 -U isit -d isit
```

---

## MinIO

S3 API:

```text
http://localhost:9000
```

Management Console:

```text
http://localhost:9001
```

Локальные credentials:

```text
Username: admin
Password: adminadmin
```

---

## MailHog

Web UI:

```text
http://localhost:8025
```

SMTP:

```text
localhost:1025
```

---

# Запуск backend

Перейти в директорию:

```bash
cd backend
```

Установить переменную подключения к PostgreSQL:

```bash
export DATABASE_URL=postgres://isit:isit@localhost:5432/isit
```

Запустить:

```bash
cargo run
```

Backend будет доступен:

```text
http://localhost:8080
```

При запуске SQLx автоматически применит migrations.

Проверить:

```bash
curl http://localhost:8080/healthz
```

---

# Запуск frontend

Открыть отдельный terminal:

```bash
cd frontend
```

Установить зависимости:

```bash
npm install
```

При необходимости указать backend:

```bash
export NEXT_PUBLIC_API_BASE_URL=http://localhost:8080
```

Запустить Next.js development server:

```bash
npm run dev
```

Frontend будет доступен:

```text
http://localhost:3000
```

---

# Текущая Docker-конфигурация

В репозитории сохранилась `docker-compose.yml`, первоначально рассчитанная на предыдущую Vite-версию frontend.

В ней frontend всё ещё использует:

```text
VITE_API_BASE_URL
```

и port:

```text
5173
```

Текущая версия frontend использует:

```text
Next.js
NEXT_PUBLIC_API_BASE_URL
port 3000
```

Поэтому для текущего состояния проекта рекомендуется запускать инфраструктуру через Docker Compose, а frontend — отдельно через:

```bash
npm run dev
```

До синхронизации Docker-конфигурации с текущей Next.js-версией.

---

# Текущее состояние проекта

На данный момент реализована основная база информационной системы:

| Модуль | Состояние |
|---|---|
| PostgreSQL schema | Реализовано |
| Database migrations | Реализовано |
| Expenses CRUD API | Реализовано |
| Departments CRUD API | Реализовано |
| Categories CRUD API | Реализовано |
| Funding Sources CRUD API | Реализовано |
| Users CRUD API | Реализовано |
| Next.js frontend | Реализовано |
| Expense management UI | Реализовано |
| Reference management UI | Реализовано |
| Analytics UI | Реализовано |
| Health checks | Реализовано |
| Docker development infrastructure | Частично |
| JWT authentication | В разработке |
| Role-based access control | В разработке |
| File upload API | В разработке |
| MinIO integration | В разработке |
| Approval workflow | В разработке |
| Audit log integration | В разработке |
| Automated tests | Требуют расширения |
| OpenAPI / Swagger | В разработке |

---

# Дальнейшее развитие

Архитектура проекта предусматривает дальнейшее развитие следующих направлений:

```text
Authentication
       ↓
JWT + RBAC
       ↓
Protected API
```

```text
Expense
   ↓
Documents
   ↓
MinIO
```

```text
Expense
   ↓
Approval workflow
   ↓
Approved / Rejected
```

```text
Business operations
       ↓
Audit Log
       ↓
Change History
```

Также планируется:

- расширение серверной аналитики;
- экспорт отчётов;
- полноценная фильтрация расходов;
- загрузка документов;
- авторизация и authentication middleware;
- role-based permissions;
- API documentation;
- unit и integration tests;
- актуализация Docker Compose для Next.js frontend.

---

# Основная идея проекта

ISIT демонстрирует архитектуру полноценного full-stack приложения:

```text
Business Domain
       │
       ▼
Next.js UI
       │
       ▼
REST API
       │
       ▼
Axum Handlers
       │
       ▼
Repository Layer
       │
       ▼
SQLx
       │
       ▼
PostgreSQL
```

При этом проект включает не только CRUD-интерфейс, но и предметную модель для развития корпоративной системы управления расходами:

```text
Users
Departments
Categories
Funding Sources
Expenses
Documents
Approvals
Audit Logs
Analytics
```

Главная цель системы — обеспечить единое пространство для регистрации, контроля и анализа операционных расходов организации.

---

<div align="center">

### Operational Expenses Information System

**Rust · Axum · PostgreSQL · Next.js · TypeScript · Redux Toolkit · Docker**

</div>
