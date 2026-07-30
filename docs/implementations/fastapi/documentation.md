# FastAPI Documentation

This page demonstrates how FastAPI implements the concepts introduced in the Documentation capability page.

Before reading this page, it is recommended that you first understand:

- The problem documentation solves
- Why documentation matters
- General API engineering concepts behind documentation

Documentation is a capability.

FastAPI is one way of implementing that capability.

---

## Why

One of FastAPI's most powerful features is its built-in documentation support.

Unlike many frameworks where documentation is added later through external tools, FastAPI generates API documentation automatically from your code.

Benefits include:

✅ Less boilerplate

✅ Automatic OpenAPI generation

✅ Interactive API exploration

✅ Automatically documented request models

✅ Automatically documented response models

✅ Better API discoverability

✅ Improved maintainability

FastAPI encourages developers to treat documentation as part of API development rather than an afterthought.

---

## Core Concepts

### OpenAPI

OpenAPI is a specification that describes REST APIs in a machine-readable format.

FastAPI automatically generates an OpenAPI document based on your routes, models, parameters, and metadata.

This OpenAPI specification powers FastAPI's documentation tools.

---

### Swagger UI

Swagger UI provides an interactive web interface for exploring and testing APIs.

FastAPI automatically generates Swagger UI.

By default:

```text
/docs
```

displays interactive API documentation.

Developers can:

- Browse endpoints
- View schemas
- Submit requests
- Inspect responses

without writing any additional code.

---

### ReDoc

ReDoc provides an alternative documentation interface.

By default:

```text
/redoc
```

displays a more documentation-oriented view of the API.

Many teams prefer ReDoc for reading and exploration.

---

### Route Metadata

Route metadata improves generated documentation.

Examples:

```python
@app.get(
    "/users",
    summary="List users",
    description="Retrieve all users"
)
```

This information appears automatically in generated documentation.

Metadata helps explain endpoint behavior.

---

### Request Models

Pydantic models contribute directly to generated documentation.

Example:

```python
class UserCreate(BaseModel):
    name: str
    email: str
```

FastAPI automatically documents:

- Fields
- Types
- Required values

Consumers can understand request expectations without separate documentation.

---

### Response Models

Response models describe endpoint outputs.

Example:

```python
@app.get(
    "/users/{user_id}",
    response_model=User
)
```

FastAPI automatically includes the response structure in the documentation.

This makes API contracts visible and discoverable.

---

### Tags

Tags organize related endpoints.

Example:

```python
@app.get(
    "/users",
    tags=["Users"]
)
```

This groups endpoints within the documentation interface.

Tags improve navigation as APIs grow.

---

### Application Metadata

FastAPI allows API-wide metadata.

Example:

```python
app = FastAPI(
    title="User API",
    description="Manage users",
    version="1.0.0"
)
```

This information appears throughout generated documentation.

---

# Discover

## Scenario

You encounter the following FastAPI application.

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/health")
def health_check():
    return {"status": "ok"}
```

Without adding any documentation-specific code, the API already exposes:

```text
/docs
```

and

```text
/redoc
```

## Objective

Understand FastAPI's built-in documentation support.

## Success Criteria

The learner should be able to explain:

- What OpenAPI is
- What Swagger UI provides
- Why documentation appears automatically

## Hints

??? tip "Small Hint"

    FastAPI is generating something from the route definition.

??? tip "Stronger Hint"

    The route information is being transformed into documentation.

??? tip "Almost There"

    FastAPI automatically creates an OpenAPI specification that powers the documentation interfaces.

## Solution

??? success "Solution"

    FastAPI automatically generates an OpenAPI specification.

    That specification is used to create:

    - Swagger UI at `/docs`
    - ReDoc at `/redoc`

    As routes are added, the documentation updates automatically.

    This reduces the effort required to keep documentation accurate.

## Why This Exercise Exists

The goal is to understand FastAPI's documentation-first approach.

FastAPI treats documentation as a natural byproduct of well-structured API code.

---

# Apply

## Scenario

The following endpoint appears in documentation, but it is not very descriptive.

```python
@app.post("/users")
def create_user():
    pass
```

You want consumers to understand its purpose more clearly.

## Objective

Improve the generated documentation using metadata.

## Success Criteria

The learner should be able to:

- Add a summary
- Add a description
- Improve endpoint discoverability

## Hints

??? tip "Small Hint"

    Route decorators support additional parameters.

??? tip "Stronger Hint"

    Look for ways to describe the endpoint directly in the decorator.

??? tip "Almost There"

    Use `summary` and `description`.

## Solution

??? success "Solution"

```python
@app.post(
    "/users",
    summary="Create user",
    description="Create a new user account"
)
def create_user():
    pass
```

This information automatically appears in the generated documentation.

Consumers can quickly understand the endpoint's purpose.

## Why This Exercise Exists

Documentation becomes more valuable when endpoints communicate their intentions clearly.

FastAPI makes this easy through route metadata.

---

## Compose

### Scenario

You are building a User API.

Requirements:

- Endpoints should be grouped together
- Request schemas should be documented
- Response schemas should be documented
- Consumers should understand the API without reading source code

### Objective

Combine FastAPI documentation features into a complete solution.

### Success Criteria

The learner should be able to combine:

- Tags
- Request models
- Response models
- Metadata

### Hints

??? tip "Small Hint"

    Think beyond individual routes.

??? tip "Stronger Hint"

    FastAPI documentation is generated from many pieces working together.

??? tip "Almost There"

    Models, metadata, and tags all contribute to documentation quality.

### Solution

??? success "Solution"

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI(
    title="User API",
    version="1.0.0"
)


class UserCreate(BaseModel):
    name: str
    email: str


class User(BaseModel):
    id: int
    name: str
    email: str


@app.post(
    "/users",
    tags=["Users"],
    summary="Create user",
    response_model=User
)
def create_user(user: UserCreate):

    return {
        "id": 1,
        **user.model_dump()
    }
```

FastAPI automatically documents:

- Request fields
- Response fields
- Endpoint purpose
- Endpoint grouping

without requiring separate documentation files.

### Why This Exercise Exists

Real-world APIs require multiple documentation features working together.

Developers should understand how FastAPI builds rich documentation from code.

---

## Automate

### Scenario

Your organization maintains ten FastAPI services.

Teams currently document APIs inconsistently.

Some provide titles.

Some provide tags.

Some provide schema descriptions.

Consumers complain that each API feels different.

### Objective

Create reusable FastAPI documentation patterns.

### Success Criteria

The learner should be able to:

- Identify documentation inconsistencies
- Create reusable standards
- Improve API discoverability
- Establish documentation conventions

### Hints

??? tip "Small Hint"

    Look for common metadata across APIs.

??? tip "Stronger Hint"

    Standards are easier than reinventing documentation every time.

??? tip "Almost There"

    Consistent metadata and tagging greatly improve usability.

### Solution

??? success "Solution"

An organization might require:

```python
app = FastAPI(
    title="API Name",
    description="API Description",
    version="1.0.0"
)
```

and require every endpoint to provide:

```python
tags=[...]
summary="..."
```

along with request and response models.

These conventions help ensure all APIs provide a similar documentation experience.

### Why This Exercise Exists

Professional API engineers often define standards that improve consistency across services and teams.

Documentation should scale just like code.

---

## Common Pitfalls

### Relying Solely on Route Names

#### Why It Happens

Developers assume endpoint paths explain everything.

Example:

```python
@app.post("/users")
```

#### Better Approach

Add metadata.

```python
summary="Create user"
description="Create a new user account"
```

Consumers should not need to guess endpoint behavior.

---

### Omitting Request Models

#### Why It Happens

Developers use raw dictionaries instead of typed models.

#### Better Approach

Use Pydantic models.

```python
class UserCreate(BaseModel):
    name: str
    email: str
```

FastAPI can then document the request schema automatically.

---

### Omitting Response Models

#### Why It Happens

Developers focus only on implementation.

#### Better Approach

Provide response models.

```python
response_model=User
```

This documents response expectations clearly.

---

### Ignoring Tags

#### Why It Happens

Small APIs initially seem easy to navigate.

#### Better Approach

Use tags consistently.

```python
tags=["Users"]
```

Organization becomes increasingly important as APIs grow.

---

### Treating Documentation as an Afterthought

#### Why It Happens

Developers focus exclusively on endpoint functionality.

#### Better Approach

View documentation as part of API design.

A well-documented API is easier to adopt, maintain, and evolve.

---

## Why This Matters

FastAPI provides powerful tooling that makes API documentation a natural part of API development.

Using:

- OpenAPI
- Swagger UI
- ReDoc
- Route metadata
- Request models
- Response models
- Tags

developers can create rich and accurate documentation directly from their code.

Because documentation is generated from the API's implementation, it is easier to keep documentation aligned with reality and reduce the risk of outdated information.

FastAPI also encourages developers to think about API contracts while building endpoints rather than treating documentation as a separate task that happens later.

In professional environments, documentation is often the primary way consumers discover and learn an API.

Good documentation improves:

- Discoverability
- Developer experience
- Integration speed
- Collaboration
- Maintainability

Understanding FastAPI's documentation features helps you build APIs that are easier to learn, easier to use, and easier to maintain over time.

---

## Related Capability

✅ Documentation

Return to the Documentation capability page to review:

- Why documentation exists
- API discoverability
- API contracts
- Documentation strategy
- Developer experience

The capability page teaches the problem.

This page teaches how FastAPI implements the solution.
