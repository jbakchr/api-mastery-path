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

### Scenario

A colleague has created the following API model:

```python
from pydantic import BaseModel

class ProductCreate(BaseModel):
    name: str
    price: float
```

You need to understand what FastAPI will validate automatically.

### Objective

Identify the automatic validation behaviour.

### Success Criteria

You should be able to explain:

- which fields are required
- which types are expected
- what happens if validation fails

Hints

??? tip "Small Hint"

    Look at each field definition.

??? tip "Stronger Hint"

    Every field has a Python type.

??? tip "Almost There"

    FastAPI uses these type hints to validate incoming data.

### Solution

??? success "Solution"

    FastAPI validates:

    - name is required
    - price is required
    - name must be a string
    - price must be a float

### Why This Exercise Exists

Before creating validation rules yourself, you should understand the validation FastAPI already provides automatically.

---

## Apply

### Scenario

You are building a product API.

A product name should:

- contain at least 3 characters
- contain no more than 100 characters

### Objective

Add validation rules to the model.

Starting point:

```python
from pydantic import BaseModel

class ProductCreate(BaseModel):
    name: str
```

### Success Criteria

The API should reject:

- names shorter than 3 characters
- names longer than 100 characters

### Hints

??? tip "Small Hint"

    Look at Pydantic's Field() helper.

??? tip "Stronger Hint"

    Field can define minimum and maximum lengths.

??? tip "Almost There"

    Investigate:

    ```python
    Field(min_length=?, max_length=?)
    ```

### Solution

??? success "Solution"

    ```python
    from pydantic import BaseModel, Field


    class ProductCreate(BaseModel):
        name: str = Field(
            min_length=3,
            max_length=100
        )
    ```

### Why This Exercise Exists

Most validation work involves extending the basic validation FastAPI already provides.

---

## Compose

### Scenario

You are creating a blog API.

Each blog post contains:

- title
- content
- author_email

### Objective

Create a model that validates:

- title length
- content length
- email format

### Success Criteria

Your solution should use:

- Field()
- EmailStr
- sensible validation limits

### Hints

??? tip "Small Hint"

    You'll need more than one validation technique.

??? tip "Stronger Hint"

    Think about both field constraints and specialized types.

??? tip "Almost There"

    EmailStr handles email validation automatically.

### Solution

??? success "Solution"

    ```python
    from pydantic import (
        BaseModel,
        EmailStr,
        Field
    )


    class BlogPostCreate(BaseModel):
        title: str = Field(
            min_length=5,
            max_length=200
        )

        content: str = Field(
            min_length=50
        )

        author_email: EmailStr
    ```

### Why This Exercise Exists

Real APIs rarely validate only one field. Most models combine multiple validation techniques.

---

## Automate

### Scenario

Your organization maintains:

- User APIs
- Product APIs
- Billing APIs
- Reporting APIs

Many models use:

- email addresses
- timestamps
- pagination parameters

You notice the same validation rules appearing repeatedly.

### Objective

Identify reusable validation patterns.

### Success Criteria

Design:

- at least one reusable model
- at least one reusable validation approach

### Hints

??? tip "Small Hint"

    Think about models shared across multiple endpoints.

??? tip "Stronger Hint"

    Pagination often uses the same fields repeatedly.

??? tip "Almost There"

    Common concepts such as email addresses and pagination can often be standardized.

### Solution

??? success "Solution"

    Example:

    ```python
    from pydantic import BaseModel


    class PaginationParams(BaseModel):
        page: int = 1
        page_size: int = 25
    ```

    This model can be reused across multiple endpoints.

### Why This Exercise Exists

As APIs grow, reusable validation patterns improve consistency and reduce duplication.

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