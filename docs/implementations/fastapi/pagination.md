# FastAPI Pagination

This page demonstrates how FastAPI implements the concepts introduced in the Pagination capability page.

Before reading this page, it is recommended that you first understand:

- The problem pagination solves
- Why pagination matters
- General API engineering concepts behind pagination

Pagination is a capability.

FastAPI is one way of implementing that capability.

---

### Why

FastAPI provides excellent support for pagination through query parameters, type validation, and automatic documentation.

Benefits include:

✅ Minimal boilerplate

✅ Automatic query parameter handling

✅ Automatic validation

✅ Automatic OpenAPI documentation

✅ Consistent request patterns

✅ Easy integration with filtering and sorting

Rather than manually parsing request values, FastAPI automatically converts and validates pagination inputs.

This allows API engineers to focus on API design rather than request-processing code.

---

### Core Concepts

#### Query Parameters

Pagination is most commonly implemented using query parameters.

Example:

```http
GET /products?page=1&page_size=20
```

FastAPI automatically maps these values to function parameters.

```python
@app.get("/products")
def get_products(
    page: int = 1,
    page_size: int = 20
):
    ...
```

---

#### Page Numbers

One common pagination strategy is page-based pagination.

Example:

```http
GET /products?page=3&page_size=20
```

Conceptually:

```text
Page 1 → Items 1-20

Page 2 → Items 21-40

Page 3 → Items 41-60
```

This approach is easy for consumers to understand.

---

#### Limit and Offset

Another common strategy is limit/offset pagination.

Example:

```http
GET /products?offset=40&limit=20
```

Meaning:

```text
Skip first 40 items

Return next 20 items
```

FastAPI easily supports this pattern through query parameters.

---

#### Query()

FastAPI provides `Query()` for pagination validation.

Example:

```python
from fastapi import Query

@app.get("/products")
def get_products(
    page_size: int = Query(20, ge=1, le=100)
):
    ...
```

This helps prevent:

- negative values
- page sizes that are too large
- invalid requests

---

#### Pagination Metadata

Pagination is usually more useful when additional information is returned.

Example:

```json
{
    "items": [...],
    "page": 2,
    "page_size": 20,
    "total_items": 250
}
```

FastAPI does not generate pagination metadata automatically.

API engineers typically create it themselves.

---

#### Combining Pagination and Filtering

Pagination frequently works together with filtering.

Example:

```http
GET /products?category=books&page=2
```

Consumers first narrow the data set.

Then they page through the results.

This combination is extremely common in professional APIs.

---

### Discover

#### Scenario

You are given the following endpoint:

```python
@app.get("/products")
def get_products(
    page: int = 1,
    page_size: int = 20
):
    return {
        "page": page,
        "page_size": page_size
    }
```

#### Objective

Understand how FastAPI receives pagination inputs.

#### Success Criteria

The learner should be able to explain:

- where pagination values come from
- how FastAPI converts request values
- why default values are useful

#### Hints

??? tip "Small Hint"

    Look at the function parameters.

??? tip "Stronger Hint"

    FastAPI automatically maps query parameters to Python arguments.

??? tip "Almost There"

    Requests can override the default values.

#### Solution

??? success "Solution"

    FastAPI treats `page` and `page_size` as query parameters.

    Example:

    ```http
    GET /products?page=3&page_size=50
    ```

    Produces:

    ```python
    page = 3
    page_size = 50
    ```

    FastAPI performs the conversion automatically.

#### Why This Exercise Exists

The goal is to understand how FastAPI receives pagination information from incoming requests.

---

### Apply

#### Scenario

You have the following endpoint:

```python
@app.get("/products")
def get_products():
    return products
```

Consumers only want 20 products at a time.

#### Objective

Add basic pagination.

#### Success Criteria

The learner should be able to:

- accept page numbers
- accept page sizes
- return only a subset of results

#### Hints

??? tip "Small Hint"

    Use query parameters.

??? tip "Stronger Hint"

    Calculate where the page starts.

??? tip "Almost There"

    Slice the collection using the calculated values.

#### Solution

??? success "Solution"

```python
@app.get("/products")
def get_products(
    page: int = 1,
    page_size: int = 20
):
    start = (page - 1) * page_size
    end = start + page_size

    return products[start:end]
```

#### Why This Exercise Exists

The goal is to reinforce the basic mechanics of page-based pagination.

---

### Compose

#### Scenario

You are building an e-commerce API.

Consumers want:

- category filtering
- pagination
- pagination metadata

#### Objective

Create a complete paginated endpoint.

#### Success Criteria

The learner should be able to combine:

- filtering
- pagination
- response metadata

#### Hints

??? tip "Small Hint"

    Apply filtering first.

??? tip "Stronger Hint"

    Pagination should operate on the filtered result set.

??? tip "Almost There"

    Include helpful pagination information in the response.

#### Solution

??? success "Solution"

```python
@app.get("/products")
def get_products(
    category: str | None = None,
    page: int = 1,
    page_size: int = 20
):
    results = products

    if category:
        results = [
            p for p in results
            if p["category"] == category
        ]

    total_items = len(results)

    start = (page - 1) * page_size
    end = start + page_size

    return {
        "items": results[start:end],
        "page": page,
        "page_size": page_size,
        "total_items": total_items,
    }
```

#### Why This Exercise Exists

Real-world APIs often combine multiple capabilities.

Pagination becomes much more useful when integrated with filtering.

---

### Automate

#### Scenario

Your organization maintains many APIs.

Every team implements pagination differently:

- different parameter names
- different page sizes
- different metadata structures

Consumers complain about inconsistency.

#### Objective

Create reusable pagination patterns.

#### Success Criteria

The learner should be able to:

- identify duplication
- create reusable solutions
- standardize implementation patterns

#### Hints

??? tip "Small Hint"

    Look for repeated pagination parameters.

??? tip "Stronger Hint"

    Similar validation rules occur across endpoints.

??? tip "Almost There"

    Shared dependencies can reduce duplication.

#### Solution

??? success "Solution"

```python
from fastapi import Depends, Query


def pagination_params(
    page: int = Query(1, ge=1),
    page_size: int = Query(20, ge=1, le=100),
):
    return {
        "page": page,
        "page_size": page_size,
    }


@app.get("/products")
def get_products(
    pagination: dict = Depends(
        pagination_params
    )
):
    return pagination
```

This creates consistent pagination behavior across many endpoints.

#### Why This Exercise Exists

Professional FastAPI projects often standardize common patterns to improve maintainability and consistency.

---

### Common Pitfalls

#### Returning Everything Anyway

##### Why It Happens

Pagination parameters are accepted but never applied.

##### Better Approach

Always use pagination values when selecting which items to return.

---

#### Missing Validation

##### Why It Happens

Developers trust incoming values.

```python
page_size: int
```

A consumer could request:

```text
1000000
```

items.

##### Better Approach

Use limits.

```python
page_size: int = Query(
    20,
    ge=1,
    le=100
)
```

---

#### Forgetting Metadata

##### Why It Happens

The focus remains solely on the data itself.

##### Better Approach

Return helpful information such as:

- current page
- page size
- total item count

This improves consumer experience.

---

#### Paginating Before Filtering

##### Why It Happens

Pagination logic is implemented before data-discovery logic.

##### Better Approach

Apply:

```text
Filtering
    ↓
Sorting
    ↓
Pagination
```

This typically produces more useful results.

---

#### Inconsistent Parameter Names

##### Why It Happens

Different developers make different choices.

Examples:

```text
page

pageNumber

current_page

p
```

##### Better Approach

Establish shared conventions across the API.

Consistency is valuable.

---

### Why This Matters

Pagination is one of the most common capabilities implemented in modern APIs.

FastAPI makes pagination straightforward through:

- query parameters
- automatic type conversion
- validation
- OpenAPI documentation

These features allow API engineers to implement pagination with relatively little code while maintaining good API design practices.

You will encounter pagination in:

- public APIs
- internal APIs
- e-commerce systems
- reporting systems
- data platforms
- SaaS products

Understanding how FastAPI implements pagination is an important step toward building APIs that remain usable and scalable as datasets grow.

## Related Capability

✅ Pagination

Return to the Pagination capability page to review:

- why pagination exists
- pagination strategies
- pagination tradeoffs
- interactions with filtering and sorting

The capability page teaches API Engineering thinking.

This page teaches how FastAPI implements those ideas.
