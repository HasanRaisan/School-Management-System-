# School Management System
[![.NET](https://img.shields.io/badge/.NET-10.0-512BD4?logo=dotnet)](https://dotnet.microsoft.com/)
[![EF Core](https://img.shields.io/badge/EF%20Core-10.0-512BD4)](https://docs.microsoft.com/ef/core/)
[![SQL Server](https://img.shields.io/badge/SQL%20Server-2022-CC2927?logo=microsoft-sql-server&logoColor=white)](https://www.microsoft.com/sql-server/)
[![Keycloak](https://img.shields.io/badge/Keycloak-24.0-CH2927?logo=keycloak&logoColor=white)](https://www.keycloak.org/)
[![Architecture](https://img.shields.io/badge/Architecture-Clean%20Architecture-blue)](#architecture)
[![DDD](https://img.shields.io/badge/Pattern-Domain%20Driven%20Design-orange)](#design-patterns)
[![CQRS & MediatR](https://img.shields.io/badge/Pattern-CQRS%20%26%20MediatR-black)](#cqrs-mediatr)
[![Hangfire](https://img.shields.io/badge/Background%20Jobs-Hangfire-red?logo=hangfire)](https://www.hangfire.io/)

Multi-tenant system built using ASP.NET Core and Clean Architecture, serving multiple schools. Implemented CQRS with MediatR and role-, permission-, and policy-based authorization. Designed scalable REST APIs covering the students' lifecycle, teachers, guardians, examinations, financial operations, and more with secure JWT-based authentication.

---
## Front-End (ReactJS) README FILE. (COMING SOON)

## 🔒 Source Code Access Notice

> **Important Note:** To protect intellectual property, this public repository serves as an **Architecture Showcase**. It contains the complete architectural framework, layer separations, interfaces, and selected design patterns. The core production business logic and sensitive infrastructure configurations are withheld.

> **How to review the full source code?**
> If you are a hiring manager, technical recruiter, or software engineer reviewing my profile for employment opportunities, I will gladly grant you full access to the private production repository. 
> * **Request Access via Email:** [hasan.raisann@gmail.com]
> * Please include your GitHub username and the company you represent.

## 📺 Architecture & Technical Walkthrough (3-Min Video)

Before diving into the files, you can watch this quick technical walkthrough where I explain the core architecture, design decisions, and how data flows through the system:

👉 **[Watch the Technical Video Walkthrough on YouTube (COMING SOON)]()**

*In this video, I cover: Multi-tenancy isolation logic, Keycloak authorization integration, pipelines, and MediatR command handling.*


## Table of Contents

- [Overview](#overview)
- [Use Cases / Features](#use-cases--features)
  - [Core](#core)
- [Technology Stack](#technology-stack)
  - [Backend](#backend)
  - [Architecture Patterns](#architecture-patterns)
- [Folder Structure](#folder-structure)
  - [Layer Responsibilities](#layer-responsibilities)
- [Database](#database)
  - [Database Provider](#database-provider)
  - [Entity Framework Core](#entity-framework-core)
  - [Key Entities](#key-entities)
- [Authorization](#authorization)
  - [Authorization Types](#authorization-types)
    - [Role-Based Authorization](#role-based-authorization)
    - [Permission-Based Authorization](#permission-based-authorization)
    - [Policy-Based Authorization](#policy-based-authorization)
  - [Mixing Authorization Types](#mixing-authorization-types)
- [Multi Tenancy Architecture](#multi-tenancy-architecture)
  - [Tenancy Model](#tenancy-model)
  - [Tenant Isolation Strategy](#tenant-isolation-strategy)
  - [Tenant Resolution](#tenant-resolution)
  - [User Model in Multi-Tenant Context](#user-model-in-multi-tenant-context)
  - [Data Security](#data-security)
- [Development](#development)
  - [Code Style](#code-style)
  - [Key Patterns Used](#key-patterns-used)
  - [Project Dependencies](#project-dependencies)
  - [Generate a token](#generate-a-token)
- [Contact info](#contact-info)

## Overview

The School Management System is a modern, scalable solution that handles all aspects of school administration. It provides RESTful APIs for managing:

- **Student Management**: Enrollment, attendance, academic records, and status tracking
- **Teacher Management**: Employee records, specializations, and teaching assignments
- **Academic Management**: Academic years, grades, subjects, and school stages
- **Financial Management**: Fees, payments, fines, payment methods, Expenses, and teachers' salaries
- **Classroom Management**: Classrooms, sections, and teaching assignments
- **Examination System**: Exams, examiners, and results
- **Events & Awards**: School events and student awards
- **Guardian Management**: Student guardians and relationships

## Use Cases / Features

### Core 

- **Student Management**
  - Student enrollment and registration
  - Student status tracking (Pending, Active, Graduated, etc.)
  - Student attendance tracking
  - Academic year enrollment management

- **Teacher Management**
  - Teacher registration and employee management
  - Specialization management
  - Teaching assignments to class sections
  - Job title and department management
  - Teacher Available Days and periods with shift type 

- **Academic Management**
  - Academic year management
  - Grade and grade type management
  - Subject management
  - School stage management
  - Promotion status tracking

- **Financial Management**
  - Fee management and fee types
  - Payment processing with multiple payment methods
  - Fine management
  - Student fee tracking
  - Expenses and Teacher's Salary

- **Classroom Management**
  - Classroom and hall management
  - Class section management
  - Section teaching assignments
  - Weekly schedule management

- **Examination System**
  - Exam creation and management
  - Exam class section assignments
  - Examiner assignments
  - Exam results tracking

- **Additional**
  - Events and event participants
  - Awards and award types
  - Guardian management
  - Document management
  - Login audit tracking
  - School holidays management

## Technology Stack

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

### Architecture Patterns

- **Clean Architecture** - Separation of concerns
- **CQRS** - Command Query Responsibility Segregation
- **Repository Pattern** - Data access abstraction
- **Unit of Work Pattern** - Transaction management
- **Domain-Driven Design (DDD)** - Domain-centric design

## Folder Structure

<img width="1948" height="1600" alt="Clean Architecture" src="https://github.com/user-attachments/assets/cf0389a3-a7a3-408a-956d-859b9f80ff24" />

> **Note: This is a purposive sample, not all code.**


### Layer Responsibilities

- **API Layer**: Handles HTTP requests, routing, and API documentation
- **Application Layer**: Contains business logic, commands, queries, and validation
- **Domain Layer**: Core business entities, domain rules, and value objects
- **Infrastructure Layer**: Data access, external services, and infrastructure concerns

## Database

### Database Provider

- **SQL Server** - Primary database

### Entity Framework Core

- Uses EF Core 10.0.1 for ORM
- Repository pattern for data access

### Key Entities

- **Students**: Student records and enrollments
- **Teachers**: Teacher and employee records
- **Academic**: Academic years, grades, subjects
- **Financial**: Fees, payments, fines, expenses
- **Classrooms**: Classrooms, sections, schedules
- **Exams**: Exams, results, examiners
- **Events**: School events and participants
- **Guardians**: Student guardians

## Authorization

This project emphasizes complex authorization scenarios and supports _role-based_, _permission-based_, and _policy-based_ authorization.

To apply any authorization type:

- Use the `Authorize` attribute with the parameter.
- Implement `IBaseAuthorizeableRequest` through one of the derived interfaces such as `IBasicAuthorizeableRequest`, `ITeacherRequest`, or others.
- Implementing `IBaseAuthorizeableRequest` ensures the request enters the Authorization Pipeline Behavior.

```csharp
public class AuthorizationBehavior<TRequest, TResponse>(
    IAuthorizationService _authorizationService)
    : IPipelineBehavior<TRequest, TResponse>
    where TRequest: IBaseAuthorizeableRequest<TResponse>
    where TResponse: IErrorOr
{
    // Execution logic
}
```

### Authorization Types

#### Role-Based Authorization

To apply role-based authorization:

- Use the `Authorize` attribute with the `Roles` parameter.

For example:

```csharp
[Authorize(Roles = Role.Admin)]
public record CreateClassroomCommand(
    string Number,
    int Capacity,
    string? Location,
    bool IsAvailable = true
) : IBasicAuthorizeableRequest<ErrorOr<Classroom>>;
```

Will only allow users with the `Admin` role to cancel subscriptions.

#### Permission-Based Authorization

To apply permission-based authorization, use the `Authorize` attribute with the `Permissions` parameter

For example:

```csharp
[Authorize(Permissions = Permission.Payment.Get)]
public record GetPaymentByIdQuery(Guid StudentId, int Id) : IBasicAuthorizeableRequest<ErrorOr<Payment>>;
```

Will only allow users with the `Permission.Payment.Get` permission to get a payment.

#### Policy-Based Authorization

To apply policy-based authorization, use the `Authorize` attribute with the `Policy` parameter

For example:

```csharp
[Authorize(Policies = Policy.TeacherOfClassOrAdmin)]
public record CreateGradeCommand(
    int SectionId,
    int SubjectId,
    int GradeTypeId,
    IReadOnlyList<StudentGradeDto> Students
) : ITeacherRequest<ErrorOr<Success>>;
```

Will only allow users who pass the `TeacherOfClassOrAdmin` policy to create grades.

Each policy is implemented as a simple method in the `PolicyEnforcer` class.

The policy "TeacherOfClassOrAdmin" for example, can be implemented as follows:

```csharp
public class PolicyEnforcer : IPolicyEnforcer
{
        public async Task<ErrorOr<Success>> AuthorizeAsync<T>(IBaseAuthorizeableRequest<T> request,
            CurrentUser currentUser, string policy, CancellationToken ct = default)
        {
            return policy switch
            {
                Policy.SelfOrAdmin => SelfOrAdminPolicy(request, currentUser),
                Policy.TeacherOfClassOrAdmin => await TeacherOfClassAdminPolicy(request, currentUser, ct),
                Policy.GuardainOfStudentOrAdmin => await GuardianOfStudentAdminPolicy(request, currentUser, ct),
                _ => Error.Unexpected(code: "General.Unexpected", description: $"Authorization policy '{policy}' is not registered.")
            };
        }

        private async Task<ErrorOr<Success>> TeacherOfClassAdminPolicy<T>(
            IBaseAuthorizeableRequest<T> request,
            CurrentUser currentUser,
            CancellationToken ct)
        {
            if (currentUser.Roles.Contains(Role.Admin)) return Result.Success;

            if (request is ITeacherRequest<T> teacherRequest)
            {
                 // Use Cashe for this
                var isAssigned = await unitOfWork.SectionTeachingAssignments
                    .AnyAsync(sta => sta.TeacherId == currentUser.Id &&
                                     sta.SectionId == teacherRequest.SectionId &&
                                     sta.SubjectId == teacherRequest.SubjectId, ct);

                return isAssigned? Result.Success
                        : Error.Unauthorized(code: "Unauthorized", description: "Access denied: You are not assigned to this specific section and subject.");
            }

            return Error.Unexpected(code: "General.Unexpected", description: "Policy mismatch: TeacherOfClass requires a request implementing ITeacherRequest.");
        }
}
```

### Mixing Authorization Types

You can mix and match authorization types to create complex authorization scenarios.

For example:

```csharp
[Authorize(Permissions = Permission.Grades.List, Policies = Policy.TeacherOfClassOrAdmin, Roles = Role.GradeManager)]
public record ListGradesBySectionQuery(
int SectionId,
int SubjectId,
int GradeTypeId
) : ITeacherRequest<ErrorOr<Success>>;
```

Will only allow users with the `Permission.Grades.List` permission, who pass the `TeacherOfClassOrAdmin` policy, and who have the `GradeManager` role, to list reminders.

Another option is specifying the `Authorize` attribute multiple times:

```csharp
[Authorize(Permissions = Permission.Grades.List)]
[Authorize(Policies = Policy.TeacherOfClassOrAdmin)]
[Authorize(Roles = Role.GradeManager)]
public record ListGradesBySectionQuery(
int SectionId,
int SubjectId,
int GradeTypeId
) : ITeacherRequest<ErrorOr<Success>>;
```

## Multi Tenancy Architecture
This system is designed as a SaaS application supporting multiple schools using a **shared database and shared schema** model.



### Tenancy Model

- **Multi Tenant**
- **Same Database**
- **Same Schema**
- **Logical data isolation using TenantId**

Each school is treated as a separate tenant.  
All data is isolated logically using a `TenantId` column across all tenant-owned entities.



### Tenant Isolation Strategy

Every tenant-scoped entity includes `TenantId.`

All queries are filtered automatically by `TenantId` to prevent cross-school data access.
**Global Query Filter Example:**

```csharp
    var method = typeof(SchoolDbContext)
        .GetMethod(nameof(BuildSchoolFilter), BindingFlags.Instance | BindingFlags.NonPublic)!
        .MakeGenericMethod(clrType);
     var filter = (LambdaExpression)method.Invoke(this, null)!;
         modelBuilder.Entity(clrType).HasQueryFilter(filter);

    private Expression<Func<TEntity, bool>> BuildSchoolFilter<TEntity>()
    where TEntity: class, ISchoolId
    {
        return e => e.SchoolId == CurrentSchoolId;
    }
```
### Tenant Resolution
`TenantId` is resolved per request using JWT claim

Example: 
```json
{
  "sub": "user-id",
  "role": "teacher",
  "schoolId": "school-id"
}
```

### User Model in Multi-Tenant Context
Supported user roles per tenant:
- **Admin**
- **Teacher**
- **Student**
- **Guardian**

### Data Security
- All queries enforce `TenantId` filtering
- Authorization policies are tenant-aware
- No cross-tenant joins are allowed

## Development

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
  - SchoolManagement.Domain

- **SchoolManagement.Application** depends on:
  - SchoolManagement.Domain

- **SchoolManagement.Infrastructure** depends on:
  - SchoolManagement.Application
  - SchoolManagement.Domain

- **SchoolManagement.Domain** does not depend on any layer or external libraries (Pure C#).

### Generate a token

> Note: The system has an external identity provider, so the project uses a simple token generator endpoint that generates a token based on the provided details. This is a simple way to generate a token for testing purposes and is closer to how the system will likely be designed when using an external identity provider.

## Contact info

- [GitHub Profile](https://github.com/HasanRaisan)
- [LinkedIn Profile](https://www.linkedin.com/in/hasan-raisan)
