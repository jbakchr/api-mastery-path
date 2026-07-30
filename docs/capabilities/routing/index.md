# Routing

Routing is one of the foundational capabilities of API Engineering.

Every API receives requests from clients.

Examples:

- Web applications
- Mobile applications
- Other APIs
- Internal services
- Automated systems

When a request arrives, the API must decide:

```text
What functionality should handle this request?
```

Routing is the capability that answers this question.

Without routing, an API has no way of directing incoming requests to the correct functionality.

---

## The Problem

Imagine an online bookstore API.

Clients need to:

- View all books
- View a specific book
- Create a book
- Update a book
- Delete a book

A request arrives:

```http
GET /books/123
```

How does the API know that:

```text
Book #123 should be retrieved
```

rather than:

```text
Create a new book
```

or:

```text
List all books
```

Routing provides the structure that allows APIs to direct requests to the correct operation.

Without routing:

- Requests become ambiguous
- APIs become difficult to maintain
- Consumers become confused
- Functionality becomes hard to discover

---

## Why Routing Matters

Routing helps ensure:

✅ Requests reach the correct functionality

✅ APIs remain organized as they grow

✅ Consumers can discover resources easily

✅ Functionality remains predictable

✅ URLs communicate intent clearly

Well-designed routing is one of the foundations of a usable API.

---

## Key Concepts

### Endpoints

An endpoint is a specific location within an API.

Examples:

```http
GET /books

GET /books/123

POST /books
```

Each endpoint represents a specific capability exposed by the API.

---

### Resources

Most APIs expose resources.

Examples:

```text
Users

Books

Orders

Products

Invoices
```

Routes are often designed around resources rather than actions.

Instead of:

```http
GET /getBooks
```

many APIs prefer:

```http
GET /books
```

because the route represents a resource.

---

### HTTP Methods

The same route can perform different actions depending on the HTTP method.

Examples:

```http
GET /books
```

Retrieve books.

---

```http
POST /books
```

Create a book.

---

```http
PUT /books/123
```

Update book 123.

---

```http
DELETE /books/123
```

Delete book 123.

---

### Path Parameters

Some routes operate on specific resources.

Example:

```http
GET /users/42
```

The value:

```text
42
```

is a path parameter.

Path parameters identify a specific resource.

---

### Query Parameters

Query parameters modify how a route behaves.

Example:

```http
GET /books?category=python
```

The route remains:

```text
/books
```

but the query parameter changes what data is returned.

Common uses:

- Filtering
- Searching
- Sorting
- Pagination

---

### Route Design

Good routes are:

✅ Predictable

✅ Consistent

✅ Easy to understand

Examples:

```http
GET /users

GET /users/42

POST /users

DELETE /users/42
```

Poor routes often become difficult to learn and maintain.

---

## Discover

### Scenario

You join a team that maintains a books API.

You discover the following routes:

```http
GET /books

GET /books/123

POST /books

DELETE /books/123
```

### Objective

Understand what each route is responsible for.

### Success Criteria

The learner should be able to:

- identify the resource
- explain the purpose of each route
- identify which routes operate on a specific resource

### Hints

??? tip "Small Hint"

    Focus on the URL path first.

??? tip "Stronger Hint"

    Notice that all routes relate to the same resource.

??? tip "Almost There"

    Some routes operate on the collection while others operate on a specific item.

### Solution

??? success "Solution"

    Resource:

    ```text
    Books
    ```

    Routes:

    ```http
    GET /books
    ```

    Retrieves all books.

    ```http
    GET /books/123
    ```

    Retrieves a specific book.

    ```http
    POST /books
    ```

    Creates a new book.

    ```http
    DELETE /books/123
    ```

    Deletes a specific book.

### Why This Exercise Exists

Before designing routes, API engineers must understand how routes map requests to functionality.

---

## Apply

### Scenario

You are designing a task management API.

Users should be able to:

- View tasks
- View a specific task
- Create tasks
- Delete tasks

### Objective

Design routes that support these capabilities.

### Success Criteria

The learner should be able to:

- identify resources
- define appropriate routes
- choose suitable HTTP methods

### Hints

??? tip "Small Hint"

    Think about the resource first.

??? tip "Stronger Hint"

    Resources are usually nouns.

??? tip "Almost There"

    You probably need routes similar to:

    ```http
    /tasks
    ```

    and

    ```http
    /tasks/{id}
    ```

### Solution

??? success "Solution"

    Example:

    ```http
    GET /tasks

    GET /tasks/{id}

    POST /tasks

    DELETE /tasks/{id}
    ```

### Why This Exercise Exists

Designing routes is one of the first responsibilities when creating a new API.

---

## Compose

### Scenario

You are designing an e-commerce API.

Resources include:

- Customers
- Orders
- Products

Customers can create orders.

Orders contain products.

### Objective

Design an initial routing structure.

### Success Criteria

The learner should be able to:

- identify resources
- create routes for multiple resources
- maintain consistency across routes

### Hints

??? tip "Small Hint"

    Start by identifying the major resources.

??? tip "Stronger Hint"

    Think about how each resource would be listed and accessed individually.

??? tip "Almost There"

    Most resources need:

    ```http
    GET /resource

    GET /resource/{id}

    POST /resource
    ```

### Solution

??? success "Solution"

    Example:

    ```http
    GET /customers
    GET /customers/{id}

    GET /orders
    GET /orders/{id}

    GET /products
    GET /products/{id}

    POST /orders
    ```

### Why This Exercise Exists

Real APIs typically contain multiple resources that must remain consistent and understandable.

---

## Automate

### Scenario

Your organization develops:

- User APIs
- Product APIs
- Billing APIs
- Reporting APIs

Each team creates routes differently.

Examples:

```http
/getUsers

/users

/userList

/fetch-users
```

Developers struggle to understand unfamiliar APIs.

### Objective

Define reusable routing principles.

### Success Criteria

The learner should be able to:

- identify inconsistencies
- propose routing standards
- improve API consistency

### Hints

??? tip "Small Hint"

    Consider how consistency affects API consumers.

??? tip "Stronger Hint"

    Think about resource-oriented design.

??? tip "Almost There"

    Teams might benefit from agreeing on common resource naming conventions.

### Solution

??? success "Solution"

    Examples:

    - Use nouns for resources
    - Use plural resource names
    - Avoid action-based URLs
    - Use HTTP methods to express actions
    - Follow consistent naming conventions

### Why This Exercise Exists

Large organizations benefit greatly from standardized routing patterns.

---

## Why This Matters

Routing is one of the first capabilities every API engineer must understand.

It answers a fundamental question:

```text
How do requests reach the correct functionality?
```

Strong routing helps make APIs:

- Predictable
- Consistent
- Discoverable
- Maintainable

Almost every other API capability depends on routing.

For example:

```text
Routing
    ↓
Validation
    ↓
Error Handling
    ↓
Authentication
    ↓
Authorization
```

Learning routing is therefore not about learning framework syntax.

It is about understanding one of the foundational capabilities of API Engineering.

---

# Framework Implementations

Implement This Capability Using

✅ ../../implementations/fastapi/routing/

⬜ Express

⬜ ASP.NET

⬜ Spring Boot