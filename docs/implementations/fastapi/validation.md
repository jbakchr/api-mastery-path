# FastAPI Validation

This page demonstrates how FastAPI implements the validation concepts introduced in the Validation capability page.

Before reading this page, it is recommended that you first understand:

- Why validation exists
- Common validation rules
- Request validation
- Response validation

Validation is a capability.

FastAPI is one way of implementing that capability.

---

## Why FastAPI Validation Is Powerful

FastAPI provides built-in validation through Pydantic.

Instead of manually checking values:

```python
if not email:
    ...
```

or:

```python
if age < 0:
    ...
```

you define the expected structure of your data and FastAPI automatically validates incoming requests.

Benefits:

✅ Less boilerplate

✅ Consistent validation

✅ Automatic documentation

✅ Useful error messages

✅ Easier maintenance

---

## Request Validation

A common use case is validating incoming request data.

Imagine a user registration endpoint.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()


class UserCreate(BaseModel):
    username: str
    email: str
    age: int


@app.post("/users")
def create_user(user: UserCreate):
    return user
```

FastAPI automatically validates:

- Required fields
- Data types
- Request structure

If validation fails, FastAPI returns an error response.

---

## Missing Required Fields

Suppose a client sends:

```json
{
  "email": "jonas@example.com"
}
```

The request is missing:

```text
username
```

FastAPI automatically returns an error.

You do not need to write custom validation code for required fields.

---

## Type Validation

Suppose a client sends:

```json
{
  "username": "jonas",
  "email": "jonas@example.com",
  "age": "twenty-five"
}
```

FastAPI expects:

```python
age: int
```

Validation fails before the endpoint logic runs.

This prevents invalid data from entering the application.

---

## Adding Validation Rules

Many APIs require stronger validation than simple type checking.

Pydantic's `Field()` can be used to specify validation rules.

```python
from pydantic import BaseModel, Field


class UserCreate(BaseModel):
    username: str = Field(
        min_length=3,
        max_length=50
    )

    age: int = Field(
        gt=0,
        lt=120
    )
```

Examples:

```text
min_length
max_length
gt (greater than)
lt (less than)
ge (greater than or equal)
le (less than or equal)
```

These rules are automatically enforced.

---

## Email Validation

Many APIs accept email addresses.

Pydantic provides specialized types.

```python
from pydantic import BaseModel, EmailStr


class UserCreate(BaseModel):
    email: EmailStr
```

Now FastAPI validates that incoming values look like valid email addresses.

Example:

✅ `jonas@example.com`

❌ `not-an-email`

---

## Response Validation

FastAPI can also validate data leaving the API.

```python
from pydantic import BaseModel


class UserResponse(BaseModel):
    id: int
    username: str
    email: str
```

```python
@app.get("/users/{user_id}",
         response_model=UserResponse)
def get_user(user_id: int):
    return {
        "id": user_id,
        "username": "jonas",
        "email": "jonas@example.com"
    }
```

Benefits:

- Consistent responses
- Better API contracts
- Safer refactoring
- Improved documentation

---

## Custom Validation

Sometimes business rules require custom validation.

Example:

```text
Usernames cannot contain spaces.
```

Pydantic allows custom validators.

```python
from pydantic import BaseModel, field_validator


class UserCreate(BaseModel):
    username: str

    @field_validator("username")
    @classmethod
    def validate_username(cls, value):
        if " " in value:
            raise ValueError(
                "Username cannot contain spaces."
            )

        return value
```

This automatically runs whenever a request is validated.

---

## Validation Errors

When validation fails, FastAPI automatically returns structured error responses.

Example:

```json
{
  "detail": [
    {
      "type": "string_too_short",
      "loc": ["body", "username"],
      "msg": "String should have at least 3 characters"
    }
  ]
}
```

This makes it easier for API consumers to understand what went wrong.

---

## Discover

Consider the following model:

```python
class ProductCreate(BaseModel):
    name: str
    price: float
```

Questions:

1. Which fields are required?
2. What data types are expected?
3. What validation occurs automatically?

---

## Apply

Extend the model:

```python
class ProductCreate(BaseModel):
    name: str
```

Requirements:

- Name must contain at least 3 characters
- Name must contain no more than 100 characters

Implement the validation rules.

---

## Compose

Design validation for a blog post API.

A blog post contains:

- title
- content
- author_email
- tags

Think about:

- Required fields
- Length limits
- Email validation
- Response models

Create appropriate Pydantic models.

---

## Automate

Imagine your organization maintains:

- User APIs
- Product APIs
- Billing APIs
- Reporting APIs

Many APIs use:

- IDs
- Pagination parameters
- Email addresses
- Timestamps

Think about:

- Which models might be reusable?
- Which validation rules are repeated?
- How could shared models improve consistency?

---

## Why This Matters

FastAPI's validation features allow developers to focus on defining API contracts rather than writing repetitive validation logic.

As systems grow, strong validation becomes increasingly important for:

- Reliability
- Security
- Maintainability
- API usability

The goal is not simply to learn Pydantic or FastAPI syntax.

The goal is to understand how FastAPI helps implement one of the most important API Engineering capabilities:

```text
Validation
```

---

# Related Capability

✅ ../../capabilities/validation/

Return to the capability page to review the underlying concepts and engineering principles behind validation.