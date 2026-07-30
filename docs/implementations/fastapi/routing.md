# FastAPI Routing

This page demonstrates how FastAPI implements the concepts introduced in the Routing capability page.

Before reading this page, it is recommended that you first understand:

- What routing is
- Why routing matters
- Resources and endpoints
- Path parameters
- Query parameters
- Route design principles

Routing is a capability.

FastAPI is one way of implementing that capability.

---

## Why FastAPI Routing Is Powerful

FastAPI provides a clean and explicit way to define routes.

Instead of manually inspecting incoming requests and deciding what code should run, FastAPI maps requests directly to Python functions.

Benefits:

✅ Less boilerplate

✅ Readable code

✅ Automatic validation

✅ Automatic documentation

✅ Easy route organization

✅ Strong editor support

Routing is often one of the first FastAPI features developers encounter because nearly everything in an API begins with a route.

---

## Core Concepts

### Route Decorators

Route decorators connect URLs and HTTP methods to Python functions.

Examples:

```python
@app.get("/books")
```

```python
@app.post("/books")
```

```python
@app.put("/books/{book_id}")
```

```python
@app.delete("/books/{book_id}")
```

---

### Path Parameters

Path parameters allow routes to operate on specific resources.

Example:

```python
@app.get("/books/{book_id}")
```

When a request arrives:

```http
GET /books/123
```

FastAPI extracts:

```python
book_id = 123
```

automatically.

---

### Query Parameters

Query parameters modify how a route behaves.

Example:

```python
@app.get("/books")
def get_books(category: str):
    ...
```

Request:

```http
GET /books?category=python
```

FastAPI automatically extracts:

```python
category = "python"
```

---

### APIRouter

As APIs grow, placing every route in a single file becomes difficult to maintain.

FastAPI provides:

```python
APIRouter
```

to organize related routes.

Example:

```python
users_router
```

```python
books_router
```

```python
orders_router
```

This helps keep larger APIs maintainable.

---

## Discover

### Scenario

A colleague created the following route:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}
```

### Objective

Understand what the route does.

### Success Criteria

The learner should be able to explain:

- the HTTP method
- the endpoint path
- the purpose of the path parameter

### Hints

??? tip "Small Hint"

    Start by looking at the route decorator.

??? tip "Stronger Hint"

    Notice the value inside curly braces.

??? tip "Almost There"

    Requests such as:

    ```http
    GET /users/42
    ```

    will provide a value for user_id.

### Solution

??? success "Solution"

    This route:

    ```python
    @app.get("/users/{user_id}")
    ```

    responds to:

    ```http
    GET
    ```

    requests.

    The path parameter:

    ```python
    user_id
    ```

    identifies which user should be returned.

    Example:

    ```http
    GET /users/42
    ```

    produces:

    ```json
    {
      "user_id": 42
    }
    ```

### Why This Exercise Exists

Before creating routes, developers should understand how FastAPI maps URLs to Python functions.

---

## Apply

### Scenario

You are building a books API.

The API should allow clients to retrieve a specific book.

### Objective

Complete the route.

Starting point:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/books/{book_id}")
def get_book():
    return {}
```

### Success Criteria

The route should:

- accept a path parameter
- return the supplied book ID
- support requests such as:

```http
GET /books/123
```

### Hints

??? tip "Small Hint"

    The function needs a parameter.

??? tip "Stronger Hint"

    The function parameter should match the route parameter name.

??? tip "Almost There"

    FastAPI matches:

    ```python
    {book_id}
    ```

    to:

    ```python
    book_id
    ```

### Solution

??? success "Solution"

    ```python
    from fastapi import FastAPI

    app = FastAPI()


    @app.get("/books/{book_id}")
    def get_book(book_id: int):
        return {
            "book_id": book_id
        }
    ```

### Why This Exercise Exists

Path parameters are one of the most common routing patterns used in APIs.

---

## Compose

### Scenario

You are creating a books API.

Clients should be able to:

- list books
- filter by category
- retrieve a specific book

### Objective

Create routes that support these capabilities.

### Success Criteria

The learner should combine:

- route decorators
- path parameters
- query parameters

### Hints

??? tip "Small Hint"

    You'll probably need more than one route.

??? tip "Stronger Hint"

    Consider:

    ```http
    /books
    ```

    and

    ```http
    /books/{id}
    ```

??? tip "Almost There"

    Categories are usually a good fit for query parameters.

### Solution

??? success "Solution"

    ```python
    from fastapi import FastAPI

    app = FastAPI()


    @app.get("/books")
    def get_books(category: str | None = None):
        return {
            "category": category
        }


    @app.get("/books/{book_id}")
    def get_book(book_id: int):
        return {
            "book_id": book_id
        }
    ```

### Why This Exercise Exists

Most APIs combine multiple routing techniques to support common business requirements.

---

## Automate

### Scenario

Your organization maintains:

- Users API
- Books API
- Orders API
- Billing API

All routes are currently stored in a single file.

The file contains hundreds of routes and is becoming difficult to maintain.

### Objective

Create a reusable routing structure.

### Success Criteria

The learner should be able to:

- identify route organization problems
- use APIRouter
- create reusable route modules

### Hints

??? tip "Small Hint"

    Think about grouping related routes.

??? tip "Stronger Hint"

    FastAPI provides a tool specifically for organizing routes.

??? tip "Almost There"

    Investigate:

    ```python
    APIRouter
    ```

### Solution

??? success "Solution"

    Example:

    ```python
    from fastapi import APIRouter

    router = APIRouter(
        prefix="/books",
        tags=["Books"]
    )


    @router.get("/")
    def get_books():
        return []
    ```

    Main application:

    ```python
    from fastapi import FastAPI

    from books import router as books_router

    app = FastAPI()

    app.include_router(books_router)
    ```

### Why This Exercise Exists

As APIs grow, route organization becomes increasingly important.

APIRouter is one of the key tools FastAPI provides for maintaining large APIs.

---

## Common Pitfalls

### Mistake

Using action names in routes.

Example:

```http
/getBooks
```

### Why It Happens

Developers often think in terms of actions rather than resources.

### Better Approach

Prefer resource-oriented routes:

```http
GET /books
```

---

### Mistake

Creating routes that do too many things.

### Why It Happens

Developers attempt to solve multiple use cases with one endpoint.

### Better Approach

Create clear, focused routes with predictable behavior.

---

### Mistake

Placing all routes in one file.

### Why It Happens

The API initially starts small.

### Better Approach

Use:

```python
APIRouter
```

to organize functionality into separate modules.

---

## Why This Matters

Routing is one of the first capabilities every FastAPI developer learns.

FastAPI provides an elegant implementation through:

- Route decorators
- Path parameters
- Query parameters
- APIRouter

Together these tools help developers create APIs that are:

- Predictable
- Maintainable
- Discoverable
- Easy to extend

The goal is not simply to memorize:

```python
@app.get()
```

or:

```python
@app.post()
```

The goal is to understand how FastAPI implements one of the most fundamental API Engineering capabilities:

```text
Routing
```

---

# Related Capability

✅ ../../capabilities/routing/

Return to the capability page to review the API Engineering concepts and design principles behind routing.
