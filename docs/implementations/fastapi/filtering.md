# FastAPI Filtering

This page demonstrates how FastAPI implements the concepts introduced in the Filtering capability page.

Before reading this page, it is recommended that you first understand:

- The problem filtering solves
- Why filtering matters
- General API engineering concepts behind filtering

Filtering is a capability.

FastAPI is one way of implementing that capability.

---

### Why

FastAPI makes filtering straightforward because query parameters are automatically mapped to Python function parameters.

Benefits include:

✅ Minimal boilerplate

✅ Automatic request parsing

✅ Automatic validation

✅ Automatic type conversion

✅ Automatic OpenAPI documentation

✅ Clear and readable route definitions

Instead of manually extracting and validating values from requests, FastAPI handles much of the work automatically.

This allows API engineers to focus on designing useful filtering behavior rather than request-parsing logic.

---

### Core Concepts

#### Query Parameters

Query parameters are the most common mechanism for implementing filtering.

Example:

```http
GET /products?category=books
```

FastAPI automatically maps query parameters to function arguments.

```python
@app.get("/products")
def get_products(category: str | None = None):
    ...
```

---

#### Optional Parameters

Most filters should be optional.

Example:

```python
@app.get("/products")
def get_products(category: str | None = None):
    ...
```

Requests can be:

```http
GET /products
```

or:

```http
GET /products?category=books
```

The same endpoint can support multiple filtering scenarios.

---

#### Multiple Filters

Filtering often involves multiple query parameters.

```python
@app.get("/products")
def get_products(
    category: str | None = None,
    status: str | None = None
):
    ...
```

Example request:

```http
GET /products?category=books&status=active
```

FastAPI handles parameter extraction automatically.

---

#### Type Conversion

FastAPI converts query parameters into Python types.

```python
@app.get("/products")
def get_products(min_price: float | None = None):
    ...
```

Example:

```http
GET /products?min_price=100
```

FastAPI converts:

```text
"100"
```

into:

```python
100.0
```

---

#### Validation

FastAPI validates query parameters using type annotations.

```python
@app.get("/products")
def get_products(min_price: int):
    ...
```

If a consumer provides:

```http
GET /products?min_price=abc
```

FastAPI automatically returns a validation error.

---

#### Query()

FastAPI provides `Query()` for additional validation rules.

Example:

```python
from fastapi import Query

@app.get("/products")
def get_products(
    limit: int = Query(10, ge=1, le=100)
):
    ...
```

This allows:

- default values
- minimum values
- maximum values
- descriptions

---

### Discover

#### Scenario

You are given the following endpoint:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/products")
def get_products(category: str | None = None):
    return {"category": category}
```

#### Objective

Understand how FastAPI implements basic filtering.

#### Success Criteria

The learner should be able to explain:

- where the filter value comes from
- why the filter is optional
- how FastAPI maps requests to function parameters

#### Hints

??? tip "Small Hint"

    Look at the endpoint parameter.

??? tip "Stronger Hint"

    FastAPI automatically connects query parameters to function arguments.

??? tip "Almost There"

    The value of `category` comes from the request URL.

#### Solution

??? success "Solution"

    FastAPI treats `category` as an optional query parameter.

    Request:

    ```http
    GET /products?category=books
    ```

    Produces:

    ```python
    category = "books"
    ```

    FastAPI handles extraction automatically.

#### Why This Exercise Exists

The goal is to understand the simplest and most common FastAPI filtering mechanism.

---

### Apply

#### Scenario

You have the following endpoint:

```python
@app.get("/products")
def get_products():
    return products
```

Consumers want to filter products by category.

#### Objective

Modify the endpoint to support category filtering.

#### Success Criteria

The learner should be able to:

- add a query parameter
- make the parameter optional
- return matching results

#### Hints

??? tip "Small Hint"

    Add a parameter to the function.

??? tip "Stronger Hint"

    Use `category: str | None = None`.

??? tip "Almost There"

    Return only products whose category matches the provided value.

#### Solution

??? success "Solution"

```python
@app.get("/products")
def get_products(category: str | None = None):

    if category:
        return [
            product
            for product in products
            if product["category"] == category
        ]

    return products
```

#### Why This Exercise Exists

The goal is to reinforce how query parameters become filtering inputs within FastAPI endpoints.

---

### Compose

#### Scenario

You are building an e-commerce API.

Consumers want to filter products by:

- category
- minimum price
- maximum price
- availability

#### Objective

Create a filtering endpoint that supports multiple optional filters.

#### Success Criteria

The learner should be able to combine:

- optional query parameters
- filtering logic
- validation

#### Hints

??? tip "Small Hint"

    Each filter can be represented as a separate query parameter.

??? tip "Stronger Hint"

    All filters should remain optional.

??? tip "Almost There"

    Apply filters only when values are supplied.

#### Solution

??? success "Solution"

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/products")
def get_products(
    category: str | None = None,
    min_price: float | None = None,
    max_price: float | None = None,
    available: bool | None = None,
):
    results = products

    if category:
        results = [
            p for p in results
            if p["category"] == category
        ]

    if min_price is not None:
        results = [
            p for p in results
            if p["price"] >= min_price
        ]

    if max_price is not None:
        results = [
            p for p in results
            if p["price"] <= max_price
        ]

    if available is not None:
        results = [
            p for p in results
            if p["available"] == available
        ]

    return results
```

#### Why This Exercise Exists

Real-world APIs rarely rely on a single filter.

API engineers frequently combine multiple filtering criteria to create more useful endpoints.

---

### Automate

#### Scenario

Your organization maintains many APIs.

Developers repeatedly implement the same filtering patterns:

- category filters
- date filters
- status filters
- paging-related filters

The implementations are inconsistent.

#### Objective

Create reusable filtering patterns.

#### Success Criteria

The learner should be able to:

- identify duplication
- create reusable solutions
- standardize implementation patterns

#### Hints

??? tip "Small Hint"

    Look for repeated query parameter definitions.

??? tip "Stronger Hint"

    Similar validation rules often appear across endpoints.

??? tip "Almost There"

    Reuse parameter definitions and helper functions where appropriate.

#### Solution

??? success "Solution"

One reusable approach is defining common filter dependencies:

```python
from fastapi import Depends, Query


def common_filters(
    limit: int = Query(10, ge=1, le=100),
    status: str | None = None,
):
    return {
        "limit": limit,
        "status": status,
    }


@app.get("/products")
def get_products(
    filters: dict = Depends(common_filters)
):
    return filters
```

This centralizes validation and creates more consistent APIs.

#### Why This Exercise Exists

Professional FastAPI projects often rely on reusable patterns rather than repeatedly redefining filtering behavior across every endpoint.

---

### Common Pitfalls

#### Filtering Required Parameters

##### Why It Happens

Developers forget to provide a default value.

```python
def get_products(category: str):
```

FastAPI now expects the filter to always be provided.

##### Better Approach

Use optional parameters when appropriate.

```python
def get_products(category: str | None = None):
```

---

#### Filtering Everything

##### Why It Happens

Developers expose every field for filtering without considering user needs.

##### Better Approach

Only expose filters that provide genuine value.

Good API design is intentional.

---

#### Repeating Filtering Logic

##### Why It Happens

The same filtering code is copied between endpoints.

##### Better Approach

Create reusable dependencies, helper functions, or filtering utilities when patterns emerge.

---

#### Weak Validation

##### Why It Happens

Developers trust incoming values.

##### Better Approach

Use type annotations and `Query()` constraints whenever possible.

```python
limit: int = Query(10, ge=1, le=100)
```

---

#### Mixing Search And Filtering

##### Why It Happens

All data-discovery concerns are merged into one large endpoint.

##### Better Approach

Treat filtering and search as separate concepts with different goals.

Filtering narrows known criteria.

Search discovers unknown information.

---

### Why This Matters

Filtering is one of the most common API capabilities.

FastAPI implements filtering primarily through query parameters, type annotations, and validation.

These features allow API engineers to create filtering behavior with very little code while still benefiting from:

- automatic validation
- automatic documentation
- automatic type conversion
- readable endpoint definitions

You will encounter filtering in almost every professional API, including:

- e-commerce systems
- task management systems
- reporting systems
- internal business APIs
- public APIs

Understanding how FastAPI implements filtering is an important step toward becoming comfortable building data-discovery experiences in real-world APIs.

## Related Capability

✅ Filtering

Return to the Filtering capability page to review:

- why filtering exists
- when filtering should be used
- filtering design decisions
- filtering tradeoffs

The capability page teaches API Engineering thinking.

This page teaches how FastAPI implements those ideas.
