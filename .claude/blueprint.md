# MASTER PROMPT — Task Manager (Go + PostgreSQL + GORM)

## Who I am

I am a software engineering student learning full-stack development. I want to build production-style backend systems using Go, PostgreSQL, and ORM-based database design. My goal is to understand REST APIs, clean architecture, validation, and database relationships.

---

## What I want to develop

I want to build a Personal Task Manager (CRUD API system).

This system should allow users to manage tasks with full CRUD operations:

* Create tasks
* Read tasks (single + list)
* Update tasks
* Delete tasks

Optional enhancements:

* Mark task as completed/pending
* Filter tasks by status
* Search tasks by title

---

## Tech Stack

* Backend Language: Go (Golang)
* Framework: Gin (preferred) or Fiber
* Database: PostgreSQL
* ORM: GORM
* Architecture: Clean architecture (Controller → Service → Repository)
* API Type: REST API
* Validation: go-playground/validator

---

## Core Data Model

### Task Entity

```go
type Task struct {
    ID          uint      `gorm:"primaryKey" json:"id"`
    Title       string    `json:"title" validate:"required,min=3,max=100"`
    Description string    `json:"description" validate:"max=300"`
    Status      string    `json:"status" validate:"oneof=pending completed"`
    CreatedAt   time.Time
    UpdatedAt   time.Time
}
```

---

## Validation Rules

### Create / Update Task

Title

* Required
* Min length: 3 characters
* Max length: 100 characters

Description

* Optional
* Max length: 300 characters

Status

* Allowed values:

  * pending
  * completed

---

## API Endpoints

### Tasks

* POST /api/tasks → Create task
* GET /api/tasks → Get all tasks
* GET /api/tasks/:id → Get single task
* PUT /api/tasks/:id → Update task
* DELETE /api/tasks/:id → Delete task

---

## Folder Structure (Clean Architecture)

```bash
/task-manager
│
├── cmd/
│   └── main.go
│
├── config/
│   └── database.go
│
├── models/
│   └── task.go
│
├── controllers/
│   └── task_controller.go
│
├── services/
│   └── task_service.go
│
├── repositories/
│   └── task_repository.go
│
├── routes/
│   └── task_routes.go
│
├── middleware/
│   └── validation.go
│
├── utils/
│   └── response.go
│
└── go.mod
```

---

## Functional Requirements

### CRUD Flow

* Create task → saved in PostgreSQL via GORM
* Get tasks → fetch all from DB
* Get task by ID → return single record
* Update task → partial/full update
* Delete task → remove record

---

## Architecture Rules

* Controller handles HTTP requests
* Service contains business logic
* Repository handles database queries (GORM only)
* No direct database access in controller

---

## Error Handling

HTTP status codes:

* 200 OK
* 201 Created
* 400 Bad Request
* 404 Not Found
* 500 Internal Server Error

---

## Database Setup

* PostgreSQL database name: task_manager
* Auto migration using GORM:

```go
db.AutoMigrate(&Task{})
```

---

## API Response Format

### Success

```json
{
  "success": true,
  "message": "Task created successfully",
  "data": {}
}
```

### Error

```json
{
  "success": false,
  "message": "Validation error",
  "error": "Title is required"
}
```

---

## Bonus Features

* Pagination (limit, offset)
* Search by title
* Filter by status
* Swagger API documentation
* Environment configuration (.env support)

---

## Final Instruction

Generate a full backend implementation using Go, Gin, PostgreSQL, and GORM following clean architecture, including all layers, validations, and REST API endpoints.
