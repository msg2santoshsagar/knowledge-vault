# 📚 Book Tracker API – Standard Backend Evaluation Problem

A minimal yet extendable API problem for evaluating backend frameworks and tools.

---

## 🎯 Problem Statement

Design and implement a RESTful API for managing a personal book tracker. The system should allow users to add, update, delete, and view books they are reading, have read, or plan to read.

---

## 🧱 Entity: `Book`

| Field       | Type       | Description                              |
|-------------|------------|------------------------------------------|
| `id`        | UUID       | Auto-generated unique identifier         |
| `title`     | String     | Required, max 255 characters             |
| `author`    | String     | Optional                                 |
| `status`    | Enum       | One of `READING`, `COMPLETED`, `WISHLIST` |
| `rating`    | Integer    | Optional, allowed values: 1 to 5         |
| `notes`     | String     | Optional                                 |
| `createdAt` | Timestamp  | Auto-generated at creation               |
| `updatedAt` | Timestamp  | Auto-updated on modification             |

---

## 🔁 API Endpoints

### ➕ Create a Book
```
POST /api/books
```

### 📖 Get Book by ID
```
GET /api/books/{id}
```

### 📃 List Books (with filtering, sorting, pagination)
```
GET /api/books?status=READING&sort=createdAt,desc&page=0&size=10
```

### ✏️ Update a Book
```
PUT /api/books/{id}
```

### ❌ Delete a Book
```
DELETE /api/books/{id}
```

---

## 🧠 Optional Enhancements

- Add **User** entity and make books user-specific.
- Add **tagging** support for categorizing books.
- Export book data to **CSV** or **PDF**.
- Add support for **GraphQL** APIs.
- Add caching for list endpoints.
- Add soft-delete and recovery support.

---

## ⏱ Implementation Time

| Scope            | Estimated Time |
|------------------|----------------|
| Core APIs        | 2–3 hours      |
| Pagination + Sort| +1 hour        |
| Optional features| Modular        |

---

## 🛠 Suggested Usage

Use this problem to:
- Compare REST frameworks (e.g., Spring Boot vs Quarkus).
- Practice API versioning, validation, error handling.
- Learn ORM tools (Hibernate, Panache, etc.).
- Experiment with containers, CI/CD, monitoring, etc.

---

## 📁 File Info

- File: `book-tracker-api-problem.md`
- Location: `/backend` folder of this repository

---

## 🔗 License & Attribution

This document is part of the [Knowledge Vault](https://github.com/msg2santoshsagar/knowledge-vault) by Santosh Sagar. You are free to fork, extend, or reuse it.
