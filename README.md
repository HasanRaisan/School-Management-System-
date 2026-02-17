# School Management System

A comprehensive School Management System built with ASP.NET Core, following Clean Architecture principles. This system provides a robust API for managing students, teachers, academic records, financial transactions, and more.

# Table of Contents

# Overview

The School Management System is a modern, scalable solution that handles all aspects of school administration. It provides RESTful APIs for managing:

- **Student Management**: Enrollment, attendance, academic records, and status tracking
- **Teacher Management**: Employee records, specializations, and teaching assignments
- **Academic Management**: Academic years, grades, subjects, and school stages
- **Financial Management**: Fees, payments, fines, payment methods, Expenses, and teachers' salaries
- **Classroom Management**: Classrooms, sections, and teaching assignments
- **Examination System**: Exams, examiners, and results
- **Events & Awards**: School events and student awards
- **Guardian Management**: Student guardians and relationships

# Use Cases / Features

## Core Features

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

- **Additional Features**
  - Events and event participants
  - Awards and award types
  - Guardian management
  - Document management
  - Login audit tracking
  - School holidays management

# Technology Stack

## Backend

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

## Architecture Patterns

- **Clean Architecture** - Separation of concerns
- **CQRS** - Command Query Responsibility Segregation
- **Repository Pattern** - Data access abstraction
- **Unit of Work Pattern** - Transaction management
- **Domain-Driven Design (DDD)** - Domain-centric design

# Folder Structure

## Layer Responsibilities

- **API Layer**: Handles HTTP requests, routing, and API documentation
- **Application Layer**: Contains business logic, commands, queries, and validation
- **Domain Layer**: Core business entities, domain rules, and value objects
- **Infrastructure Layer**: Data access, external services, and infrastructure concerns

# Database

## Database Provider

- **SQL Server** - Primary database

## Entity Framework Core

- Uses EF Core 10.0.1 for ORM
- Repository pattern for data access

## Key Entities

- **Students**: Student records and enrollments
- **Teachers**: Teacher and employee records
- **Academic**: Academic years, grades, subjects
- **Financial**: Fees, payments, fines, expenses
- **Classrooms**: Classrooms, sections, schedules
- **Exams**: Exams, results, examiners
- **Events**: School events and participants
- **Guardians**: Student guardians

# Authorization 🔐

This project puts an emphasis on complex authorization scenarios and supports _role-based_, _permission-based_ and _policy-based_ authorization.

To applay any authorization type:

- Use the `Authorize` attribute with the parameter.
- Implement `IBaseAuthorizeableRequest` through one of the derived interfaces such as `IBasicAuthorizeableRequest`, `ITeacherRequest`, or others.
- Implementing `IBaseAuthorizeableRequest` ensures the request enters the Authorization Pipeline Behavior.

```csharp
public class AuthorizationBehavior<TRequest, TResponse>(
    IAuthorizationService _authorizationService)
    : IPipelineBehavior<TRequest, TResponse>
    where TRequest : IBaseAuthorizeableRequest<TResponse>
    where TResponse : IErrorOr
{
    // Execution logic
}
```

## Authorization Types

### Role-Based Authorization

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

### Permission-Based Authorization

To apply permission-based authorization, use the `Authorize` attribute with the `Permissions` parameter

For example:

```csharp
[Authorize(Permissions = Permission.Payment.Get)]
public record GetPaymentByIdQuery(Guid StudentId, int Id) : IBasicAuthorizeableRequest<ErrorOr<Payment>>;
```

Will only allow users with the `Permission.Payment.Get` permission to get a payment.

### Policy-Based Authorization

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

                return isAssigned ? Result.Success
                        : Error.Unauthorized(code: "Unauthorized", description: "Access denied: You are not assigned to this specific section and subject.");
            }

            return Error.Unexpected(code: "General.Unexpected", description: "Policy mismatch: TeacherOfClass requires a request implementing ITeacherRequest.");
        }
}
```

## Mixing Authorization Types

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

Will only allow users with the `Permission.Grades.List` permission, and who pass the `TeacherOfClassOrAdmin` policy, and who have the `GradeManager` role to list reminders.

Another option, is specifying the `Authorize` attribute multiple times:

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

# Development

## Code Style

- Follows C# coding conventions
- Uses nullable reference types
- Implements Clean Architecture principles

## Key Patterns Used

- **CQRS**: Commands and Queries separation
- **MediatR**: Mediator pattern for request handling
- **Repository Pattern**: Data access abstraction
- **Unit of Work**: Transaction management
- **Error Handling**: ErrorOr pattern for functional error handling
- **Validation**: FluentValidation for input validation
- **Mapping**: Mapster for object-to-object mapping

## Project Dependencies

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

## Generate a token

> Note: The system has an external identity provider, so the project uses a simple token generator endpoint that generates a token based on the provided details. This is a simple way to generate a token for testing purposes and is closer to how the system will likely be designed when using an external identity provider.

# Contact info

- [GitHub Profile](https://github.com/HasanRaisan)
- [LinkedIn Profile](https://www.linkedin.com/in/hasan-raisan)
