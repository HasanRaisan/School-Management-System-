# School Management System

A comprehensive School Management System built with ASP.NET Core, following Clean Architecture principles. This system provides a robust API for managing students, teachers, academic records, financial transactions, and more.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Architecture](#architecture)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [API Documentation](#api-documentation)
- [Database](#database)
- [Authentication & Authorization](#authentication--authorization)
- [Development](#development)

## 🎯 Overview

The School Management System is a modern, scalable solution designed to handle all aspects of school administration. It provides RESTful APIs for managing:

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
┌──────────────▼──────────────────────┐
│      Application Layer               │
│ (SchoolManagement.Application)       │
│  - Commands & Queries                │
│  - Handlers                          │
│  - Behaviors                         │
│  - Interfaces                        │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│        Domain Layer                 │
│  (SchoolManagement.Domain)          │
│  - Entities                          │
│  - Value Objects                     │
│  - Domain Logic                      │
│  - Domain Errors                     │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
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

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- [.NET 10.0 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)
- [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) (or SQL Server Express)
- [Visual Studio 2026](https://visualstudio.microsoft.com/) or [VS Code](https://code.visualstudio.com/) with C# extension
- [Git](https://git-scm.com/)

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

```

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

```

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

## 📝 License

[Specify your license here]

## 👥 Contributors

[hasan.raysann@gamil.com]

---

## Architecture Note: Distributed Identity Readiness

- The system implements a Strict Separation of Concerns between User Identity and Domain Entities. This decoupled design ensures that the Identity module is functionally autonomous, allowing for a seamless transition to a dedicated Identity Server or a Microservices architecture with zero refactoring of the core business logic.
