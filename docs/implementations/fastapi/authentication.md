# FastAPI Authentication

This page demonstrates how FastAPI implements the concepts introduced in the Authentication capability page.

Before reading this page, it is recommended that you first understand:

- The problem authentication solves
- Why authentication matters
- General API engineering concepts behind authentication

Authentication is a capability.

FastAPI is one way of implementing that capability.

---

## Why

FastAPI provides built-in tools that make it easier to implement authentication in a consistent and maintainable way.

Authentication can quickly become complex when developers must manually handle:

- Credentials
- Identity verification
- Protected endpoints
- Security workflows

FastAPI provides reusable building blocks that reduce boilerplate and encourage clean authentication patterns.

Benefits include:

✅ Built-in security utilities

✅ Reusable authentication dependencies

✅ OpenAPI integration

✅ Automatic documentation support

✅ Cleaner endpoint code

✅ Better maintainability

✅ Consistent authentication workflows

FastAPI encourages developers to separate authentication logic from business logic.

---

## Core Concepts

### Depends

`Depends()` is one of the most important concepts in FastAPI authentication.

Authentication checks are often implemented as reusable dependencies.

Example:

```python
from fastapi import Depends

@app.get("/profile")
def get_profile(user=Depends(get_current_user)):
    return user
```

This allows authentication logic to be reused across many endpoints.

---

### OAuth2PasswordBearer

`OAuth2PasswordBearer` extracts authentication tokens from incoming requests.

Example:

```python
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(
    tokenUrl="token"
)
```

FastAPI automatically looks for a bearer token in the request.

---

### Access Tokens

Authentication systems commonly issue tokens after successful authentication.

Example:

```text
Authorization: Bearer abc123
```

The token becomes evidence of the caller's identity.

---

### Current User Dependencies

Many APIs create reusable functions that retrieve the authenticated user.

Example:

```python
def get_current_user(
    token: str = Depends(oauth2_scheme)
):
    ...
```

This allows endpoints to work with authenticated identities rather than raw tokens.

---

### Security Utilities

FastAPI provides additional security helpers through:

```python
fastapi.security
```

These utilities simplify authentication implementation and integrate with OpenAPI documentation.

---

### Protected Endpoints

Protected endpoints require successful authentication before processing requests.

Example:

```python
@app.get("/me")
def get_me(
    user=Depends(get_current_user)
):
    return user
```

Only authenticated callers can access the endpoint.

---

# Discover

## Scenario

You encounter the following code:

```python
from fastapi import FastAPI, Depends
from fastapi.security import OAuth2PasswordBearer

app = FastAPI()

oauth2_scheme = OAuth2PasswordBearer(
    tokenUrl="token"
)


@app.get("/profile")
def get_profile(
    token: str = Depends(oauth2_scheme)
):
    return {
        "token": token
    }
```

## Objective

Understand how FastAPI extracts authentication information from requests.

## Success Criteria

The learner should be able to explain:

- What `OAuth2PasswordBearer` does
- What `Depends()` does
- How authentication information reaches the endpoint

## Hints

??? tip "Small Hint"

    Look at the relationship between `Depends()` and `oauth2_scheme`.

??? tip "Stronger Hint"

    FastAPI is automatically obtaining information from the request.

??? tip "Almost There"

    The token is being extracted before the endpoint executes.

## Solution

??? success "Solution"

    `OAuth2PasswordBearer` extracts a bearer token from the incoming request.

    `Depends()` injects the resulting value into the endpoint.

    This allows endpoints to receive authentication information without manually parsing request headers.

## Why This Exercise Exists

The goal is to understand the building blocks FastAPI provides for authentication.

---

# Apply

## Scenario

A profile endpoint currently allows public access.

```python
@app.get("/profile")
def get_profile():
    return {
        "name": "Jonas"
    }
```

You want to require authentication.

## Objective

Protect the endpoint using FastAPI authentication tools.

## Success Criteria

The learner should be able to:

- Add an authentication dependency
- Require a bearer token
- Restrict endpoint access

## Hints

??? tip "Small Hint"

    Add a dependency parameter.

??? tip "Stronger Hint"

    Use `OAuth2PasswordBearer`.

??? tip "Almost There"

    Inject a token before processing the request.

## Solution

??? success "Solution"

```python
from fastapi import Depends
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(
    tokenUrl="token"
)


@app.get("/profile")
def get_profile(
    token: str = Depends(oauth2_scheme)
):
    return {
        "name": "Jonas"
    }
```

The endpoint now requires authentication information before executing.

## Why This Exercise Exists

The goal is to reinforce the core FastAPI authentication workflow.

Many APIs use authentication dependencies to protect endpoints.

---

# Compose

## Scenario

You are building a Task API.

Requirements:

- Users must authenticate
- Endpoints should access the current user
- Authentication logic should be reusable
- Multiple endpoints should share the same authentication behavior

## Objective

Create a reusable authentication solution.

## Success Criteria

The learner should be able to combine:

- OAuth2PasswordBearer
- Depends
- Reusable authentication functions

## Hints

??? tip "Small Hint"

    Authentication logic should not be duplicated in every endpoint.

??? tip "Stronger Hint"

    Create a shared dependency.

??? tip "Almost There"

    Build a function that retrieves the current user.

## Solution

??? success "Solution"

```python
from fastapi import Depends
from fastapi.security import OAuth2PasswordBearer

oauth2_scheme = OAuth2PasswordBearer(
    tokenUrl="token"
)


def get_current_user(
    token: str = Depends(oauth2_scheme)
):
    return {
        "username": "jonas"
    }


@app.get("/tasks")
def list_tasks(
    user=Depends(get_current_user)
):
    return {
        "user": user
    }
```

Authentication logic now exists in a reusable dependency.

Endpoints can focus on business logic rather than authentication details.

## Why This Exercise Exists

Real APIs rarely protect a single endpoint.

Reusable authentication patterns improve maintainability.

---

# Automate

## Scenario

Your organization operates multiple FastAPI services.

Different teams implement authentication differently.

Some endpoints parse tokens manually.

Some use custom middleware.

Some duplicate authentication logic throughout their codebases.

The architecture team wants a standardized approach.

## Objective

Create reusable FastAPI authentication patterns.

## Success Criteria

The learner should be able to:

- Identify duplication
- Create reusable solutions
- Standardize implementation patterns

## Hints

??? tip "Small Hint"

    Look for repeated authentication logic.

??? tip "Stronger Hint"

    Shared authentication dependencies reduce duplication.

??? tip "Almost There"

    Authentication should be implemented once and reused everywhere.

## Solution

??? success "Solution"

One possible approach is to standardize:

```python
oauth2_scheme
```

and:

```python
get_current_user()
```

across all services.

Example:

```python
def get_current_user(
    token: str = Depends(oauth2_scheme)
):
    ...
```

Endpoints can then use:

```python
user = Depends(get_current_user)
```

This creates a consistent authentication approach across many APIs.

## Why This Exercise Exists

Professional APIs typically rely on reusable security patterns rather than isolated implementations.

Consistency improves maintainability and reduces errors.

---

## Common Pitfalls

### Mixing Authentication and Business Logic

#### Why It Happens

Developers place authentication checks directly inside endpoints.

Example:

```python
@app.get("/tasks")
def get_tasks():

    # authentication logic

    # business logic
```

#### Better Approach

Move authentication into reusable dependencies.

```python
user = Depends(get_current_user)
```

---

### Duplicating Authentication Logic

#### Why It Happens

Authentication code is copied between endpoints.

#### Better Approach

Create shared authentication functions.

```python
def get_current_user():
    ...
```

Reuse them everywhere.

---

### Confusing Authentication and Authorization

#### Why It Happens

The concepts are closely related.

#### Better Approach

Separate responsibilities.

Authentication:

```text
Who are you?
```

Authorization:

```text
What are you allowed to do?
```

Authenticate first.

Authorize second.

---

### Protecting Some Endpoints Inconsistently

#### Why It Happens

Developers forget to apply authentication dependencies.

#### Better Approach

Create clear patterns and consistently apply them across protected endpoints.

---

### Trusting Tokens Without Validation

#### Why It Happens

Developers focus only on retrieving tokens.

#### Better Approach

Always verify and validate tokens before trusting their contents.

Authentication is about establishing trust, not merely reading credentials.

---

## Why This Matters

FastAPI provides powerful tools for implementing authentication through:

- Depends
- OAuth2PasswordBearer
- Security utilities
- Reusable dependencies

These tools allow developers to separate authentication logic from business logic and create reusable security patterns.

As APIs grow, authentication becomes increasingly important because systems must reliably identify callers before granting access to protected functionality.

Understanding FastAPI's authentication tools helps you build APIs that are:

- More secure
- More maintainable
- More consistent
- Easier to extend

Authentication is one of the foundational capabilities of professional API engineering, and FastAPI provides a clean and structured way to implement it.

---

## Related Capability

✅ Authentication

Return to the Authentication capability page to review:

- Why authentication exists
- Identity and credentials
- Authentication vs authorization
- Trust and verification

The capability page teaches the problem.

This page teaches how FastAPI implements the solution.