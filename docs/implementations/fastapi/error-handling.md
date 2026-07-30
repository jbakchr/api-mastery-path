# FastAPI Error Handling

This page demonstrates how FastAPI implements the concepts introduced in the Error Handling capability page.

Before reading this page, it is recommended that you first understand:

- The problem error handling solves
- Why error handling matters
- General API engineering concepts behind error handling

Error Handling is a capability.

FastAPI is one way of implementing that capability.

---

## Why

FastAPI provides built-in tools for handling API failures in a consistent and predictable manner.

These tools make it easier to communicate errors clearly while minimizing boilerplate code.

Benefits include:

✅ Less boilerplate

✅ Consistent error responses

✅ Built-in HTTP status code support

✅ Automatic OpenAPI documentation

✅ Customizable exception handling

✅ Cleaner endpoint code

Instead of manually building error responses everywhere, FastAPI provides reusable mechanisms that help keep APIs maintainable.

---

## Core Concepts

### HTTPException

`HTTPException` is FastAPI's primary mechanism for returning API errors.

Example:

```python
from fastapi import HTTPException

raise HTTPException(
    status_code=404,
    detail="User not found"
)
```

This automatically creates an appropriate error response.

---

### Status Codes

Status codes communicate the general category of a response.

Common examples:

```text
200 OK
201 Created
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
500 Internal Server Error
```

Choosing appropriate status codes helps API consumers understand what happened.

---

### detail

The `detail` parameter contains information about the error.

Example:

```python
raise HTTPException(
    status_code=404,
    detail="Book not found"
)
```

Response:

```json
{
  "detail": "Book not found"
}
```

The detail message should be clear and helpful.

---

### Custom Exceptions

As APIs grow, repeating the same `HTTPException` logic becomes tedious.

Custom exceptions allow reusable error definitions.

Example:

```python
class UserNotFoundError(Exception):
    pass
```

These exceptions can later be handled centrally.

---

### Exception Handlers

Exception handlers define application-wide error behavior.

Example:

```python
@app.exception_handler(UserNotFoundError)
async def user_not_found_handler(request, exc):
    return JSONResponse(
        status_code=404,
        content={
            "error": {
                "code": "USER_NOT_FOUND",
                "message": "User does not exist"
            }
        }
    )
```

Exception handlers help enforce consistency across an entire API.

---

### Validation Errors

FastAPI automatically validates request data.

If validation fails:

```python
class User(BaseModel):
    email: EmailStr
```

and the request contains invalid data, FastAPI automatically returns a validation error response.

This reduces the amount of error handling developers must write manually.

---

# Discover

## Scenario

You encounter the following endpoint:

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

@app.get("/users/{user_id}")
def get_user(user_id: int):

    if user_id != 1:
        raise HTTPException(
            status_code=404,
            detail="User not found"
        )

    return {"id": 1, "name": "Jonas"}
```

## Objective

Understand how FastAPI implements error handling.

## Success Criteria

The learner should be able to explain:

- What `HTTPException` does
- Why status code 404 is used
- What the `detail` field represents

## Hints

??? tip "Small Hint"

    Look at what happens when the user ID is not 1.

??? tip "Stronger Hint"

    FastAPI stops processing when the exception is raised.

??? tip "Almost There"

    The exception becomes an HTTP error response automatically.

## Solution

??? success "Solution"

    The endpoint raises an `HTTPException`.

    FastAPI converts this exception into a response:

    ```json
    {
      "detail": "User not found"
    }
    ```

    with status code:

    ```text
    404 Not Found
    ```

    This allows developers to communicate failure clearly without manually building the response.

## Why This Exercise Exists

The goal is to learn the most common FastAPI mechanism for communicating API failures.

---

# Apply

## Scenario

The following endpoint always returns a successful response.

```python
@app.get("/books/{book_id}")
def get_book(book_id: int):
    return {"id": book_id}
```

You want the API to return an error when the book does not exist.

Assume only book ID 1 exists.

## Objective

Modify the code using `HTTPException`.

## Success Criteria

The learner should be able to:

- Raise an error conditionally
- Use an appropriate status code
- Provide a useful error message

## Hints

??? tip "Small Hint"

    Compare the incoming ID against the valid ID.

??? tip "Stronger Hint"

    Use `HTTPException`.

??? tip "Almost There"

    Return a 404 when the book is not found.

## Solution

??? success "Solution"

```python
from fastapi import HTTPException

@app.get("/books/{book_id}")
def get_book(book_id: int):

    if book_id != 1:
        raise HTTPException(
            status_code=404,
            detail="Book not found"
        )

    return {
        "id": 1,
        "title": "API Engineering"
    }
```

## Why This Exercise Exists

The goal is to reinforce the most common FastAPI error handling workflow.

Professional APIs frequently return errors when resources cannot be found.

---

# Compose

## Scenario

A User API must support:

- User lookup
- Authentication
- Validation

Failures should be communicated consistently.

You want all "user not found" situations to return the same response structure.

## Objective

Create a custom exception and exception handler.

## Success Criteria

The learner should be able to combine:

- Custom exceptions
- Exception handlers
- Structured error responses

## Hints

??? tip "Small Hint"

    Create a custom exception class.

??? tip "Stronger Hint"

    Register an exception handler.

??? tip "Almost There"

    Return a consistent JSON structure from the handler.

## Solution

??? success "Solution"

```python
from fastapi import FastAPI, Request
from fastapi.responses import JSONResponse

app = FastAPI()


class UserNotFoundError(Exception):
    pass


@app.exception_handler(UserNotFoundError)
async def user_not_found_handler(
    request: Request,
    exc: UserNotFoundError
):
    return JSONResponse(
        status_code=404,
        content={
            "error": {
                "code": "USER_NOT_FOUND",
                "message": "User does not exist"
            }
        }
    )


@app.get("/users/{user_id}")
def get_user(user_id: int):

    if user_id != 1:
        raise UserNotFoundError()

    return {
        "id": 1,
        "name": "Jonas"
    }
```

## Why This Exercise Exists

Real APIs often require consistency across many endpoints.

Exception handlers help centralize error behavior.

---

# Automate

## Scenario

Your organization operates multiple FastAPI services.

Different teams currently create error responses in different formats:

```json
{
  "message": "Error"
}
```

```json
{
  "detail": "Something failed"
}
```

```json
{
  "error": "User missing"
}
```

The architecture team wants a shared approach.

## Objective

Create reusable error handling patterns.

## Success Criteria

The learner should be able to:

- Identify duplication
- Create reusable solutions
- Standardize implementation patterns

## Hints

??? tip "Small Hint"

    Look for repeated error structures.

??? tip "Stronger Hint"

    Centralize error response creation.

??? tip "Almost There"

    Exception handlers can enforce organization-wide standards.

## Solution

??? success "Solution"

One possible approach is to create a shared error format:

```python
{
    "error": {
        "code": "RESOURCE_NOT_FOUND",
        "message": "Resource not found"
    }
}
```

and implement custom exceptions:

```python
class ResourceNotFoundError(Exception):
    pass
```

with shared exception handlers:

```python
@app.exception_handler(ResourceNotFoundError)
async def resource_not_found_handler(
    request,
    exc
):
    return JSONResponse(
        status_code=404,
        content={
            "error": {
                "code": "RESOURCE_NOT_FOUND",
                "message": "Resource not found"
            }
        }
    )
```

This ensures every service communicates failures consistently.

## Why This Exercise Exists

Professional APIs often depend on reusable patterns rather than isolated implementations.

Consistency becomes increasingly important as systems and teams grow.

---

# Common Pitfalls

## Returning Errors Instead of Raising Exceptions

### Why It Happens

Developers treat error responses like normal return values.

Example:

```python
return {
    "error": "User not found"
}
```

### Better Approach

Use `HTTPException`.

```python
raise HTTPException(
    status_code=404,
    detail="User not found"
)
```

This communicates failure correctly through HTTP.

---

## Using Generic Error Messages

### Why It Happens

Developers focus on implementation rather than API consumers.

Example:

```python
detail="Error"
```

### Better Approach

Provide meaningful messages.

```python
detail="User not found"
```

Useful errors are easier to debug.

---

## Repeating Error Logic Everywhere

### Why It Happens

Each endpoint creates its own error response.

Example:

```python
raise HTTPException(...)
```

copied throughout the codebase.

### Better Approach

Use custom exceptions and exception handlers for reusable patterns.

---

## Exposing Internal Details

### Why It Happens

Developers expose exception messages directly.

Example:

```python
detail=str(exception)
```

This may leak implementation details.

### Better Approach

Return safe, client-friendly messages.

```python
detail="Unexpected server error"
```

while logging internal details separately.

---

## Ignoring Validation Errors

### Why It Happens

Developers forget that FastAPI performs validation automatically.

### Better Approach

Understand the validation responses produced by FastAPI and design your API to work consistently with them.

---

# Why This Matters

FastAPI provides powerful built-in support for error handling.

Using:

- `HTTPException`
- Status codes
- Validation errors
- Custom exceptions
- Exception handlers

developers can create APIs that communicate failures clearly and consistently.

These tools help reduce boilerplate while improving maintainability and usability.

In professional environments, error handling is essential because API consumers spend significant time diagnosing problems and recovering from failures.

Understanding FastAPI's error handling tools allows you to build APIs that are easier to maintain, easier to debug, and more enjoyable to use.

---

## Related Capability

✅ Error Handling

Return to the Error Handling capability page to review:

- Why error handling exists
- Error design principles
- Consistency strategies
- API engineering thinking

The capability page teaches the problem.

This page teaches how FastAPI implements the solution.
