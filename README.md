# School Management System

A comprehensive School Management System built with ASP.NET Core, following Clean Architecture principles. This system provides a robust API for managing students, teachers, academic records, financial transactions, and more.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Database](#database)
- [Authentication & Authorization](#authentication--authorization)
- [Development](#development)
- [Architecture Note](#Architecture-Note:-Distributed-Identity-Readiness)
- [Preview](#configuration)

## 🎯 Overview

The School Management System is a modern, scalable solution that handles all aspects of school administration. It provides RESTful APIs for managing:

- **Student Management**: Enrollment, attendance, academic records, and status tracking
- **Teacher Management**: Employee records, specializations, and teaching assignments
- **Academic Management**: Academic years, grades, subjects, and school stages
- **Financial Management**: Fees, payments, fines, and payment methods
- **Classroom Management**: Classrooms, sections, and teaching assignments
- **Examination System**: Exams, examiners, and results
- **Events & Awards**: School events and student awards
- **Guardian Management**: Student guardians and relationships

## ✨ Features

### Core Features

- ✅ **Student Management**
  - Student enrollment and registration
  - Student status tracking (Pending, Active, Graduated, etc.)
  - Student attendance tracking
  - Academic year enrollment management

- ✅ **Teacher Management**
  - Teacher registration and employee management
  - Specialization management
  - Teaching assignments to class sections
  - Job title and department management

- ✅ **Academic Management**
  - Academic year management
  - Grade and grade type management
  - Subject management
  - School stage management
  - Promotion status tracking

- ✅ **Financial Management**
  - Fee management and fee types
  - Payment processing with multiple payment methods
  - Fine management
  - Student fee tracking

- ✅ **Classroom Management**
  - Classroom and hall management
  - Class section management
  - Section teaching assignments
  - Weekly schedule management

- ✅ **Examination System**
  - Exam creation and management
  - Exam class section assignments
  - Examiner assignments
  - Exam results tracking

- ✅ **Additional Features**
  - Events and event participants
  - Awards and award types
  - Guardian management
  - Document management
  - Login audit tracking
  - School holidays management

## 🛠 Technology Stack

### Backend

- **.NET 10.0** - Latest .NET framework
- **ASP.NET Core** - Web API framework
- **Entity Framework Core 10.0.1** - ORM for database operations
- **SQL Server** - Database
- **MediatR** - CQRS pattern implementation
- **FluentValidation** - Input validation
- **ErrorOr** - Error handling pattern
- **Mapster** - Object mapping
- **JWT Bearer Authentication** - Authentication mechanism
- **ASP.NET Core Identity** - User management

### API Documentation

- **Scalar** - Interactive API documentation
- **OpenAPI** - API specification

### Architecture Patterns

- **Clean Architecture** - Separation of concerns
- **CQRS** - Command Query Responsibility Segregation
- **Repository Pattern** - Data access abstraction
- **Unit of Work Pattern** - Transaction management
- **Domain-Driven Design (DDD)** - Domain-centric design

## 🏗 Architecture

The project follows **Clean Architecture** principles with clear separation of concerns:

```
┌─────────────────────────────────────┐
│         API Layer                   │
│    (SchoolManagement.API)           │
│  - Controllers                       │
│  - Middleware                        │
│  - Mapping                           │
└──────────────┬──────────────────────┘
               │
┌──────────────·──────────────────────┐
│      Application Layer               │
│ (SchoolManagement.Application)       │
│  - Commands & Queries                │
│  - Handlers                          │
│  - Behaviors                         │
│  - Interfaces                        │
└──────────────┬──────────────────────┘
               │
┌──────────────·──────────────────────┐
│        Domain Layer                 │
│  (SchoolManagement.Domain)          │
│  - Entities                          │
│  - Value Objects                     │
│  - Domain Logic                      │
│  - Domain Errors                     │
└──────────────┬──────────────────────┘
               │
┌──────────────·──────────────────────┐
│     Infrastructure Layer             │
│ (SchoolManagement.Infrastructure)    │
│  - Data Persistence                  │
│  - Repositories                      │
│  - Security                          │
│  - External Services                 │
└─────────────────────────────────────┘
```

### Layer Responsibilities

- **API Layer**: Handles HTTP requests, routing, and API documentation
- **Application Layer**: Contains business logic, commands, queries, and validation
- **Domain Layer**: Core business entities, domain rules, and value objects
- **Infrastructure Layer**: Data access, external services, and infrastructure concerns

## 📁 Project Structure

```
SchoolManagement/
├── SchoolManagement.API/              # Web API project
│   ├── Controllers/                   # API controllers
│   ├── Middleware/                    # Custom middleware
│   ├── Mapping/                       # DTO mapping configurations
│   └── Program.cs                     # Application entry point
│
├── SchoolManagement.Application/      # Application layer
│   ├── Students/                      # Student-related commands/queries
│   ├── Employees/                     # Employee/Teacher commands/queries
│   ├── Token/                         # Authentication token queries
│   ├── Common/                        # Shared application logic
│   │   ├── Behaviors/                 # MediatR behaviors
│   │   ├── Interfaces/                # Application interfaces
│   │   └── Security/                  # Security-related code
│   └── Shared/                        # Shared application components
│
├── SchoolManagement.Domain/           # Domain layer
│   ├── Students/                      # Student domain entities
│   ├── Employees/                     # Employee/Teacher entities
│   ├── Academic/                      # Academic entities
│   ├── Financial/                     # Financial entities
│   ├── Classrooms/                    # Classroom entities
│   ├── Exams/                         # Examination entities
│   ├── Events/                        # Event entities
│   ├── Guardians/                     # Guardian entities
│   ├── Common/                        # Shared domain logic
│   └── Guards/                        # Domain validation guards
│
├── SchoolManagement.Infrastructure/   # Infrastructure layer
│   ├── Persistence/                   # EF Core configurations
│   ├── Repositories/                  # Repository implementations
│   ├── Security/                      # Security implementations
│   ├── Services/                      # External service implementations
│   └── Migrations/                    # Database migrations
│
└── SchoolManagement.Contracts/        # Shared contracts/DTOs
    ├── Student/                       # Student DTOs
    ├── Employee/                      # Employee/Teacher DTOs
    ├── Person/                        # Person DTOs
    └── Tokens/                        # Token DTOs
```

## 📚 API Documentation

### Available Endpoints

#### Student Management

- `POST /api/student` - Create a new student
- `GET /api/student?studentId={guid}` - Get student by ID

#### Teacher Management

- `POST /api/teacher` - Create a new teacher
- `GET /api/teacher?studentId={guid}` - Get teacher by ID

#### Authentication

- `POST /api/tokens/generate` - Generate JWT token

### API Documentation Tools

The project includes **Scalar** for interactive API documentation, which provides:

- Interactive API explorer
- Request/response examples
- Schema documentation
- Try-it-out functionality

## 🗄 Database

### Database Provider

- **SQL Server** - Primary database

### Entity Framework Core

- Uses EF Core 10.0.1 for ORM
- Repository pattern for data access

### Key Entities

- **Students**: Student records and enrollments
- **Teachers**: Teacher and employee records
- **Academic**: Academic years, grades, subjects
- **Financial**: Fees, payments, fines
- **Classrooms**: Classrooms, sections, schedules
- **Exams**: Exams, results, examiners
- **Events**: School events and participants
- **Guardians**: Student guardians



## 🔐 Authentication & Authorization

### Authentication
- **JWT Bearer Authentication** for API access
- Token generation endpoint: `POST /api/tokens/generate`

### Roles
The system supports the following roles:
- **Admin** - Full system access
- **Teacher** - Teacher-specific access
- **Student** - Student-specific access
- **Guardian** - Parent/guardian access

### Security Features
- JWT token-based authentication
- Role-based authorization
- Login audit tracking
- Secure password handling via ASP.NET Core Identity

## 💻 Development

### Code Style
- Follows C# coding conventions
- Uses nullable reference types
- Implements Clean Architecture principles

### Key Patterns Used
- **CQRS**: Commands and Queries separation
- **MediatR**: Mediator pattern for request handling
- **Repository Pattern**: Data access abstraction
- **Unit of Work**: Transaction management
- **Error Handling**: ErrorOr pattern for functional error handling
- **Validation**: FluentValidation for input validation
- **Mapping**: Mapster for object-to-object mapping


### Project Dependencies

- **SchoolManagement.API** depends on:
  - SchoolManagement.Application
  - SchoolManagement.Infrastructure
  - SchoolManagement.Contracts

- **SchoolManagement.Application** depends on:
  - SchoolManagement.Domain

- **SchoolManagement.Infrastructure** depends on:
  - SchoolManagement.Application
  - SchoolManagement.Domain

- **SchoolManagement.Domain** does not depend on any layer or external libraries (Pure C#).

## 👥 Contact info 

[hasan.raisann@gamil.com]


## Architecture Note: Distributed Identity Readiness

- The system implements a Strict Separation of Concerns between User Identity and Domain Entities. This decoupled design ensures the Identity module is functionally autonomous, enabling a seamless transition to a dedicated Identity Server or a Microservices architecture with zero refactoring of the core business logic.

---
---
## 🖼️ Preview
**This is a private project; you can explore these images.**
### Application Layer:
<img width="413" height="1135" alt="Screenshot 2026-01-25 174724" src="https://github.com/user-attachments/assets/1452f41f-fbd2-4910-8d79-7e0cab3e6d82" />
<img width="513" height="997" alt="Screenshot 2026-01-25 174751" src="https://github.com/user-attachments/assets/ca22a0c9-c45f-4c42-9758-0c966377b9ec" />

### Domain Layer:
<img width="389" height="1023" alt="Screenshot 2026-01-25 174836" src="https://github.com/user-attachments/assets/e8810cf7-997f-44cb-8e8b-73f6997ba9ef" />
<img width="398" height="1336" alt="Screenshot 2026-01-25 174934" src="https://github.com/user-attachments/assets/d39374e4-995b-479f-87f8-e058eb4a3c5e" />

### Infrastructure:
<img width="432" height="1263" alt="Screenshot 2026-01-25 175015" src="https://github.com/user-attachments/assets/d1382f54-ea0f-4d48-896f-0c2aa32db8ec" />

### Contracts:
<img width="345" height="221" alt="Screenshot 2026-01-25 174836439" src="https://github.com/user-attachments/assets/6e3c88f6-320b-4d0c-9ae0-8f16653ef534" />

