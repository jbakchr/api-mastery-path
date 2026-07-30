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
``**
FastAPI automatically documents:**- Request fields
- Response field**- Endpoint purpose
- Endpoint gro**ing

without requiring**eparate documentation files**
### Why This Exercise Exists

Re**-world APIs require multiple docu**ntation features working together**
Developers**hould understand how**astAPI builds rich documentation **om code.

---

## Automate

### S**nario

Your organization maintain**ten FastAPI services.

Teams curr**tly document APIs inconsistently.**Some provide titles.

Some provid**tags.

Some provide schema descri**ions.

Consumers complain that ea** API feels different.

### Object**e

Create reusable FastAPI docume**ation patterns.

### Success Crit**ia

The learner should be able to**
- Identify documentation inconsi**encies
- Create reusable standard**- Improve API discoverability
- E**ablish documentation conventions
**## Hints

??? tip "Small Hint"

 ** Look for common metadata across **Is.

??? tip "Stronger Hint"

   **tandards are easier than reinvent**g documentation every time.

??? **p "Almost There"

    Consistent **tadata and tagging greatly improv**usability.

### Solution

??? suc**ss "Solution"

An organization mi**t require:

```python
app = FastA**(
    title="API Name",
    descr**tion="API Description",
    versi**="1.0.0"
)
```

and require every**ndpoint to provide:

```python
ta**=[...]
summary="..."
```

along w**h request and response models.

T**se conventions help ensure all AP** provide a similar documentation **perience.

### Why This Exercise **ists

Professional API engineers **ten define standards that improve**onsistency across services and te**s.

Documentation should scale ju** like code.

---

## Common Pitfa**s

### Relying Solely on Route Na**s

#### Why It Happens

Developer**assume endpoint paths explain eve**thing.

Example:

```python
@app.**st("/users")
```

#### Better App**ach

Add metadata.

```python
sum**ry="Create user"
description="Cre**e a new user account"
```

Consum**s should not need to guess endpoi** behavior.

---

### Omitting Req**st Models

#### Why It Happens

D**elopers use raw dictionaries inst**d of typed models.

#### Better A**roach

Use Pydantic models.

```p**hon
class UserCreate(BaseModel):
**  name: str
    email: str
```

F**tAPI can then document the reques**schema automatically.

---

### O**tting Response Models

#### Why I**Happens

Developers focus only on**mplementation.

#### Better Appro**h

Provide response models.

```p**hon
response_model=User
```

This**ocuments response expectations cl**rly.

---

### Ignoring Tags

###**Why It Happens

Small APIs initia**y seem easy to navigate.

#### Be**er Approach

Use tags consistentl**

```python
tags=["Users"]
```

O**anization becomes increasingly im**rtant as APIs grow.

---

### Tre**ing Documentation as an Afterthou**t

#### Why It Happens

Developer**focus exclusively on endpoint fun**ionality.

#### Better Approach

**ew documentation as part of API d**ign.

A well-documented API is ea**er to adopt, maintain, and evolve**
---

## Why This Matters

FastAP**provides powerful tooling that ma**s API documentation a natural par**of API development.

Using:

- Op**API
- Swagger UI
- ReDoc
- Route **tadata
- Request models
- Respons**models
- Tags

developers can cre**e rich and accurate documentation**irectly from their code.

Because**ocumentation is generated from th**API's implementation, it is easie**to keep documentation aligned wit**reality and reduce the risk of ou**ated information.

FastAPI also e**ourages developers to think about**PI contracts while building endpo**ts rather than treating documenta**on as a separate task that happen**later.

In professional environme**s, documentation is often the pri**ry way consumers discover and lea** an API.

Good documentation impr**es:

- Discoverability
- Develope**experience
- Integration speed
- **llaboration
- Maintainability

Un**rstanding FastAPI's documentation**eatures helps you build APIs that**re easier to learn, easier to use, and easier to maintain over time.

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