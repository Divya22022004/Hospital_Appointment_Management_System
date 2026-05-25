# 🏥 HAMS — Hospital Appointment Management System

A full-stack microservices-based Hospital Appointment Management System built with **Spring Boot** (backend) and **Angular 21** (frontend).

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Microservices](#microservices)
- [Getting Started](#getting-started)
- [Environment Setup](#environment-setup)
- [Running the Application](#running-the-application)
- [API Gateway](#api-gateway)
- [Project Structure](#project-structure)
- [Screenshots](#screenshots)

---

## Overview

HAMS is a comprehensive hospital appointment management system that allows patients to book appointments with doctors, doctors to manage their schedules and prescriptions, and admins to oversee the entire system.

---

## Features

### 👤 Patient
- Register and login securely
- Browse doctors by specialization with average ratings
- Book, reschedule, and cancel appointments
- View medical history and prescriptions
- Rate and review doctors after completed appointments
- Forgot password via OTP email verification
- View and edit personal profile

### 👨‍⚕️ Doctor
- Register (pending admin approval)
- Set availability schedules with time slots
- View and manage patient appointments
- Add/edit prescriptions and medical records
- Mark appointments as complete
- View patient ratings per appointment
- View and edit professional profile

### 🔧 Admin
- Approve or reject doctor registrations
- View all registered doctors and patients
- View doctor ratings
- Delete doctor/patient accounts (cascades across services)

---

## Tech Stack

### Backend
| Technology | Version |
|---|---|
| Java | 21 |
| Spring Boot | 3.4.3 |
| Spring Cloud | 4.2.0 |
| Spring Security | 6.4.3 |
| Netflix Eureka | Service Discovery |
| Spring Cloud Gateway | API Gateway |
| OpenFeign | Inter-service communication |
| Hibernate / JPA | ORM |
| MySQL | 8.0 |
| JWT | Authentication |
| JavaMailSender | OTP Email |
| Lombok | Boilerplate reduction |
| Maven | Build tool |

### Frontend
| Technology | Version |
|---|---|
| Angular | 21 |
| TypeScript | Latest |
| RxJS | Reactive programming |
| Angular Forms | Template-driven forms |
| Vitest | Unit testing |

---

## Architecture

```
                        ┌─────────────────┐
                        │   Angular 21    │
                        │   Frontend      │
                        │  localhost:4200 │
                        └────────┬────────┘
                                 │
                        ┌────────▼────────┐
                        │   API Gateway   │
                        │  localhost:8090 │
                        │  (JWT Filter)   │
                        └────────┬────────┘
                                 │
              ┌──────────────────┼──────────────────┐
              │                  │                   │
    ┌─────────▼──────┐  ┌───────▼────────┐  ┌──────▼──────────┐
    │  Auth Service  │  │ Patient Service│  │  Doctor Profile │
    │   port: 8091   │  │   port: 8081   │  │   port: 8082    │
    └────────────────┘  └────────────────┘  └─────────────────┘
              │                  │                   │
    ┌─────────▼──────┐  ┌───────▼────────┐  ┌──────▼──────────┐
    │ Doctor Service │  │   Appointment  │  │ Medical History │
    │   port: 8084   │  │   port: 8083   │  │   port: 8085    │
    └────────────────┘  └────────────────┘  └─────────────────┘
              │
    ┌─────────▼──────┐  ┌────────────────┐
    │  Notification  │  │     Eureka     │
    │   port: 8086   │  │   port: 8761   │
    └────────────────┘  └────────────────┘
```

---

## Microservices

| Service | Port | Description |
|---|---|---|
| `eureka-server` | 8761 | Service discovery and registry |
| `api-gateway` | 8090 | Central entry point, JWT authentication |
| `auth-service` | 8091 | User registration, login, OTP, JWT |
| `patient-service` | 8081 | Patient profile management |
| `doctor-profile-service` | 8082 | Doctor profiles and ratings |
| `doctor-service` | 8084 | Doctor schedules and time slots |
| `appointment-service` | 8083 | Booking, rescheduling, cancellation |
| `medical-history-service` | 8085 | Prescriptions and medical records |
| `notification-service` | 8086 | Email notifications |

---

## Getting Started

### Prerequisites

- Java 21+
- Node.js 18+ (use even-numbered LTS versions)
- MySQL 8.0+
- Maven 3.8+
- Angular CLI 21+

### Clone the Repository

```bash
git clone https://github.com/Divya22022004/Hospital_Appointment_Management_System.git
cd Hospital_Appointment_Management_System/Final_code_for_HAMS
```

---

## Environment Setup

### 1. MySQL — Create Databases

Run the following in MySQL:

```sql
CREATE DATABASE auth_db;
CREATE DATABASE patient_db;
CREATE DATABASE doctor_profile_db;
CREATE DATABASE doctor_db;
CREATE DATABASE appointment_db;
CREATE DATABASE medical_history_db;
CREATE DATABASE notification_db;
```

### 2. Configure `application.properties` for each service

Each service has its own `application.properties`. Update the database credentials:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/<service_db>
spring.datasource.username=root
spring.datasource.password=your_password
```

### 3. Configure Email (auth-service)

In `Final_code_for_HAMS/Hospital_Appointment_Management_System_Backend/auth-service/src/main/resources/application.properties`:

```properties
spring.mail.host=smtp.gmail.com
spring.mail.port=587
spring.mail.username=your-email@gmail.com
spring.mail.password=your-gmail-app-password
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
```

> **Note:** Use a Gmail App Password, not your regular password. Enable 2-Step Verification first, then generate an App Password from your Google Account settings.

### 4. Configure Frontend Environment

In `Final_code_for_HAMS/hospital-appointment-management-system-frontend/src/environments/environment.ts`:

```typescript
export const environment = {
  production: false,
  gatewayUrl: 'http://localhost:8090'
};
```

---

## Running the Application

### Backend — Start services in this order:

```bash
cd Final_code_for_HAMS/Hospital_Appointment_Management_System_Backend

# 1. Eureka Server
cd eureka-server && mvn spring-boot:run

# 2. API Gateway
cd ../api-gateway && mvn spring-boot:run

# 3. Auth Service
cd ../auth-service && mvn spring-boot:run

# 4. Patient Service
cd ../patient-service && mvn spring-boot:run

# 5. Doctor Profile Service
cd ../doctor-profile-service && mvn spring-boot:run

# 6. Doctor Service
cd ../doctor-service && mvn spring-boot:run

# 7. Appointment Service
cd ../appointment-service && mvn spring-boot:run

# 8. Medical History Service
cd ../medical-history-service && mvn spring-boot:run

# 9. Notification Service (optional)
cd ../notification-service && mvn spring-boot:run
```

### Frontend

```bash
cd Final_code_for_HAMS/hospital-appointment-management-system-frontend
npm install
ng serve
```

Open your browser at `http://localhost:4200`

---

## API Gateway

All requests go through the API Gateway at `http://localhost:8090`.

### Public Endpoints (no authentication required)
| Method | Endpoint |
|---|---|
| POST | `/auth-service/api/v1/auth/register` |
| POST | `/auth-service/api/v1/auth/login` |
| POST | `/auth-service/api/v1/auth/send-otp` |
| POST | `/auth-service/api/v1/auth/verify-otp` |
| POST | `/auth-service/api/v1/auth/verify-otp-reset` |

### Authentication
All other endpoints require a JWT Bearer token in the `Authorization` header:
```
Authorization: Bearer <jwt-token>
```

---

## Project Structure

```
Hospital_Appointment_Management_System/
└── Final_code_for_HAMS/
    ├── Hospital_Appointment_Management_System_Backend/
    │   ├── eureka-server/
    │   ├── api-gateway/
    │   ├── auth-service/
    │   ├── patient-service/
    │   ├── doctor-profile-service/
    │   ├── doctor-service/
    │   ├── appointment-service/
    │   ├── medical-history-service/
    │   └── notification-service/
    └── hospital-appointment-management-system-frontend/
        ├── src/
        │   ├── app/
        │   │   ├── core/          # Services, auth, interceptors
        │   │   ├── features/      # Feature modules (patient, doctor, admin)
        │   │   └── shared/        # Models, components
        │   └── environments/
        └── package.json
```

---

## Default Admin Account

The system requires a pre-created admin account in the `auth_db` database:

```sql
INSERT INTO user_entity (email, password, role, status)
VALUES ('admin@hams.com', '<bcrypt-encoded-password>', 'ROLE_ADMIN', 'ACTIVE');
```

---

## Security

- JWT-based stateless authentication
- Role-based access control (PATIENT, DOCTOR, ADMIN)
- Doctor registrations require admin approval before login
- OTP-based password reset via email
- All internal service headers stripped at gateway to prevent injection
- Passwords encoded with BCrypt

---

## Running Tests

### Backend (JUnit + Mockito)
```bash
cd Final_code_for_HAMS/Hospital_Appointment_Management_System_Backend/auth-service
mvn test
```

### Frontend (Vitest)
```bash
cd Final_code_for_HAMS/hospital-appointment-management-system-frontend
ng test
```

---

## License

This project is for educational purposes.

---

## Contributors

Built as part of a Hospital Management System project.
