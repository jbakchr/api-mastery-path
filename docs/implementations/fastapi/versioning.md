# FastAPI Versioning

This page demonstrates how FastAPI implements the concepts introduced in the Versioning capability page.

Before reading this page, it is recommended that you first understand:

- The problem versioning solves
- Why versioning matters
- General API engineering concepts behind versioning

Versioning is a capability.

FastAPI is one way of implementing that capability.

---

### Why

FastAPI provides several straightforward approaches for API versioning.

Benefits include:

✅ Clear route organization

✅ Easy support for multiple API versions

✅ Automatic OpenAPI documentation

✅ Gradual migration paths

✅ Reduced risk of breaking consumers

✅ Good maintainability as APIs evolve

FastAPI does not enforce a specific versioning strategy.

Instead, it provides flexible routing and dependency features that allow API engineers to choose an appropriate approach.

---

### Core Concepts

#### Path Versioning

The most common FastAPI versioning approach is path versioning.

Example:

```http
/api/v1/products

/api/v2/products
```

FastAPI routes can easily support multiple versions:

```python
@app.get("/api/v1/products")
def get_products_v1():
    ...


@app.get("/api/v2/products")
def get_products_v2():
    ...
```

This approach is explicit and easy for consumers to understand.

---

#### APIRouter

FastAPI's `APIRouter` helps organize versioned endpoints.

Example:

```python
from fastapi import APIRouter

v1_router = APIRouter(
    prefix="/api/v1"
)

v2_router = APIRouter(
    prefix="/api/v2"
)
```

This allows related endpoints to be grouped together.

---

#### Multiple Endpoint Versions

Different API versions often coexist.

Example:

```text
Version 1
    ↓
Legacy consumers

Version 2
    ↓
New consumers
```

FastAPI allows multiple route implementations to exist simultaneously.

---

#### Shared Business Logic

Versioned APIs often share underlying business logic.

Example:

```python
def get_products():
    ...
```

Version-specific routes can call shared functions.

This reduces duplication while preserving version-specific behavior.

---

#### Response Evolution

One of the most common versioning scenarios involves response changes.

Version 1:

```json
{
  "name": "Jane Doe"
}
```

Version 2:

```json
{
  "first_name": "Jane",
  "last_name": "Doe"
}
```

FastAPI allows separate endpoints and response models for each version.

---

#### Response Models

Versioned APIs frequently use different response models.

Example:

```python
class UserV1(BaseModel):
    name: str


class UserV2(BaseModel):
    first_name: str
    last_name: str
```

This helps clearly separate behavior between versions.

---

#### Documentation

FastAPI automatically generates OpenAPI documentation.

Versioned routes appear as separate endpoints.

This helps consumers understand available versions and migration paths.

---

### Discover

#### Scenario

You are given the following FastAPI application:

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/api/v1/products")
def get_products_v1():
    return {
        "version": "v1"
    }


@app.get("/api/v2/products")
def get_products_v2():
    return {
        "version": "v2"
    }
```

#### Objective

Understand how FastAPI supports API versioning.

#### Success Criteria

The learner should be able to explain:

- how FastAPI distinguishes versions
- why multiple routes can exist
- which version a request will receive

#### Hints

??? tip "Small Hint"

    Look carefully at the route paths.

??? tip "Stronger Hint"

    The version appears in the URL.

??? tip "Almost There"

    Requests to different URLs reach different route handlers.

#### Solution

??? success "Solution"

    FastAPI treats each version as a separate route.

    Request:

    ```http
    GET /api/v1/products
    ```

    Returns Version 1.

    Request:

    ```http
    GET /api/v2/products
    ```

    Returns Version 2.

    Each route can evolve independently.

#### Why This Exercise Exists

The goal is to understand the most common versioning approach used in FastAPI applications.

---

### Apply

#### Scenario

You currently have:

```python
@app.get("/products")
def get_products():
    ...
```

A breaking change is required.

The old behavior must continue working.

#### Objective

Create a versioned endpoint.

#### Success Criteria

The learner should be able to:

- introduce a new API version
- preserve the existing version
- organize routes clearly

#### Hints

??? tip "Small Hint"

    Existing consumers should not break.

??? tip "Stronger Hint"

    Create separate routes for each version.

??? tip "Almost There"

    Use version-specific URL paths.

#### Solution

??? success "Solution"

```python
@app.get("/api/v1/products")
def get_products_v1():
    return {
        "version": "v1"
    }


@app.get("/api/v2/products")
def get_products_v2():
    return {
        "version": "v2"
    }
```

#### Why This Exercise Exists

The goal is to reinforce the core implementation pattern of introducing a new version without disrupting existing consumers.

---

### Compose

#### Scenario

You are building a customer management API.

Version 1 returns:

```json
{
  "name": "Jane Doe"
}
```

Version 2 should return:

```json
{
  "first_name": "Jane",
  "last_name": "Doe"
}
```

Both versions must remain available.

#### Objective

Create a versioned API that supports different response formats.

#### Success Criteria

The learner should be able to combine:

- route versioning
- response models
- backwards compatibility

#### Hints

??? tip "Small Hint"

    Different versions may require different schemas.

??? tip "Stronger Hint"

    Create separate response models.

??? tip "Almost There"

    Each version should expose its own contract.

#### Solution

??? success "Solution"

```python
from pydantic import BaseModel


class UserV1(BaseModel):
    name: str


class UserV2(BaseModel):
    first_name: str
    last_name: str


@app.get(
    "/api/v1/users/{user_id}",
    response_model=UserV1,
)
def get_user_v1(user_id: int):
    return {
        "name": "Jane Doe"
    }


@app.get(
    "/api/v2/users/{user_id}",
    response_model=UserV2,
)
def get_user_v2(user_id: int):
    return {
        "first_name": "Jane",
        "last_name": "Doe"
    }
```

#### Why This Exercise Exists

Many breaking changes involve request and response formats.

Versioning often requires multiple endpoint contracts to coexist.

---

### Automate

#### Scenario

Your organization maintains many APIs.

Each team manually creates versioned routes.

Over time:

- route structures differ
- naming conventions vary
- project organization becomes inconsistent

#### Objective

Create reusable versioning patterns.

#### Success Criteria

The learner should be able to:

- identify duplication
- create reusable solutions
- standardize implementation patterns

#### Hints

??? tip "Small Hint"

    What parts of versioning repeat across services?

??? tip "Stronger Hint"

    Router organization can improve consistency.

??? tip "Almost There"

    Shared conventions are often more important than the implementation itself.

#### Solution

??? success "Solution"

```python
from fastapi import APIRouter

v1_router = APIRouter(
    prefix="/api/v1",
    tags=["v1"]
)

v2_router = APIRouter(
    prefix="/api/v2",
    tags=["v2"]
)
```

Endpoints can then be attached to the appropriate router.

This creates a consistent structure across multiple services.

#### Why This Exercise Exists

Professional FastAPI systems often succeed because teams standardize implementation patterns rather than inventing new approaches for every project.

---

### Common Pitfalls

#### Supporting Too Many Versions Forever

##### Why It Happens

Teams fear removing old versions.

##### Better Approach

Define a deprecation strategy and communicate retirement timelines clearly.

---

#### Breaking Existing Versions

##### Why It Happens

Developers modify Version 1 when creating Version 2.

##### Better Approach

Treat released versions as stable contracts whenever possible.

---

#### Duplicating Business Logic

##### Why It Happens

Entire route implementations are copied between versions.

##### Better Approach

Share common business logic while keeping version-specific behavior isolated.

---

#### Inconsistent Version Naming

##### Why It Happens

Teams use different conventions.

Examples:

```text
v1

version1

api1

release1
```

##### Better Approach

Establish a consistent naming convention across APIs.

---

#### Creating New Versions Too Aggressively

##### Why It Happens

Every change results in a new version.

##### Better Approach

Understand the difference between:

```text
Breaking changes

and

Non-breaking changes
```

Not every improvement requires a new API version.

---

### Why This Matters

Versioning is one of the most important capabilities for long-lived APIs.

FastAPI makes versioning relatively straightforward through:

- path-based routing
- APIRouter organization
- response models
- automatic documentation

These features make it easier to support API evolution while reducing the risk of breaking existing consumers.

You will encounter API versioning in:

- public APIs
- SaaS platforms
- enterprise systems
- internal service ecosystems
- third-party integration platforms

Understanding how FastAPI implements versioning is an important step toward building APIs that can evolve safely over time.

## Related Capability

✅ Versioning

Return to the Versioning capability page to review:

- why versioning exists
- breaking changes
- backward compatibility
- deprecation strategies
- API evolution principles

The capability page teaches API Engineering thinking.

This page teaches how FastAPI implements those ideas.
