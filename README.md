# Barbershop Appointment System

A modular, microservices-based platform that enables barbers and clients to manage appointments efficiently. The system supports real-time notifications, smart ML-based predictions, and personalized barber recommendations.
---

## Architecture Diagram

```mermaid
graph TB
    subgraph Client["Client Layer"]
        Frontend[Frontend<br/>React / Web UI]
    end
    
    subgraph Services["Microservices Layer"]
        Core[Core Service<br/>Flask + JWT<br/>Auth & Bookings]
        Recommender[Recommender Service<br/>Python HTTP<br/>Barber Suggestions]
        ML[ML Service<br/>Python FastAPI<br/>Smart Predictions]
    end
    
    subgraph Data["Data & Messaging Layer"]
        MongoDB[(MongoDB<br/>Users, Barbers<br/>Appointments)]
        RabbitMQ{{RabbitMQ<br/>Event Queue<br/>Pub/Sub}}
    end
    
    subgraph Background["Background Services"]
        Notification[Notification Service<br/>Go<br/>Email/SMS/WebSocket]
    end
    
    Frontend -->|HTTP| Core
    Frontend -->|HTTP| Recommender
    Frontend -->|HTTP| ML
    
    Core -->|Store| MongoDB
    Core -->|Publish Events| RabbitMQ
    
    RabbitMQ -->|BOOKING_CONFIRMED<br/>BOOKING_CANCELED| Notification
    
    style Frontend fill:#61dafb,stroke:#333,stroke-width:3px,color:#000
    style Core fill:#000,stroke:#333,stroke-width:3px,color:#fff
    style Recommender fill:#3776ab,stroke:#333,stroke-width:3px,color:#fff
    style ML fill:#009688,stroke:#333,stroke-width:3px,color:#fff
    style MongoDB fill:#47a248,stroke:#333,stroke-width:3px,color:#fff
    style RabbitMQ fill:#ff6600,stroke:#333,stroke-width:3px,color:#fff
    style Notification fill:#00add8,stroke:#333,stroke-width:3px,color:#fff
```
---

## Components

### Core Service
- Built with **Flask**
- Responsibilities:
  - User & Barber Registration/Login
  - Availability Slot Management
  - Appointment Booking
  - JWT Authentication
  - Publishes events to **RabbitMQ**
  - Stores data in **MongoDB**

### Notification Service
- Written in **Go**
- Subscribes to booking events via **RabbitMQ**
- Sends real-time notifications (Email/SMS/WebSockets)

### ML Service
- Provides:
  - Smart suggestions (e.g., best slots)
- Consumed **directly by the Frontend** via HTTP

###  Recommender Service
- Suggests top barbers based on:
  - Estimated haircut type based on face features
  - Preferences
- Exposed over HTTP and **consumed by the Frontend**

---

## MongoDB

Stores collections for:
- Users
- Barbers
- Appointments
- Available Slots
- Embedded Notifications

---

## RabbitMQ (Pub/Sub)

- Core Service publishes:
  - `BOOKING_CONFIRMED`
  - `BOOKING_CANCELED`
- Notification Service consumes events

---

## 📁 Folder Structure

```
.
├── core-service/
├── notification-service/
├── ml-service/
├── recommender-service/
├── frontend/
├── tests/
└── README.md
```

---

## Getting Started

### Requirements

- Docker
- Python 3.10+
- Go 1.20+
- Node.js (for frontend, optional)

### ▶️ Run Services

```bash
cd into the service folder
make build
make run
```

---

## 👥 Contributors

- **Group 8**: Group 8

---
