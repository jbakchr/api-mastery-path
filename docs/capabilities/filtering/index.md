# Filtering

Filtering allows API consumers to request only the data they care about.

Without filtering, APIs often return large collections of data, forcing consumers to retrieve information they do not need and discard it themselves.

Filtering exists because most API consumers are not asking:

> "Give me everything."

They are asking:

> "Give me the specific information that matches my needs."

API engineers should care about filtering because it improves usability, reduces unnecessary data transfer, and helps consumers find relevant information efficiently.

---

### The Problem

Imagine an online bookstore API.

```http
GET /books
```

The API contains:

- 250,000 books
- multiple genres
- multiple authors
- books from many years

A consumer wants:

- science fiction books
- books published after 2020
- books by a specific author

Without filtering:

```text
Retrieve all books
        ↓
Process results locally
        ↓
Discard most of the data
```

This wastes:

- bandwidth
- processing time
- API resources
- developer effort

As APIs grow, returning everything becomes increasingly impractical.

Filtering solves this problem by allowing consumers to request relevant subsets of data.

---

### Why

Filtering improves:

✅ User Experience

Consumers can quickly find relevant information.

✅ Performance

Less data is transferred over the network.

✅ Scalability

Large datasets become more manageable.

✅ Maintainability

Clear filtering rules create predictable behavior.

✅ API Adoption

APIs are easier and more enjoyable to use when consumers can efficiently locate the data they need.

---

### Key Concepts

#### Filterable Fields

Not every field should be filterable.

Examples:

```text
author
category
status
created_date
price
```

A good API intentionally chooses which fields can be filtered.

---

#### Exact Match Filtering

Used when values must match exactly.

Example:

```text
status = active
```

Useful for:

- states
- categories
- identifiers

---

#### Range Filtering

Used when values exist within a range.

Examples:

```text
price >= 100

created_after = 2024-01-01
```

Useful for:

- dates
- numbers
- measurements

---

#### Multi-Filter Queries

Consumers often need more than one filter.

Example:

```text
Active users
created after January 1st
from Denmark
```

Combining filters can significantly improve data discovery.

---

#### Optional Filtering

Most filters should be optional.

Example:

```text
No filters
    ↓
Return all available items

Some filters
    ↓
Return matching items
```

This creates flexible APIs.

---

#### Invalid Filters

API engineers must decide:

```text
Ignore invalid filters?

Or

Return an error?
```

Consistency is usually more important than the specific decision.

Consumers should understand how the API behaves.

---

#### Filtering vs Search

Filtering and search often look similar but solve different problems.

Filtering:

```text
Find records matching known criteria
```

Examples:

```text
status=active

category=books
```

Search:

```text
Find records containing unknown information
```

Examples:

```text
search=python

search=fastapi
```

Understanding this distinction helps produce better API designs.

---

### Discover

#### Scenario

You are building a task management API.

The endpoint:

```http
GET /tasks
```

returns every task in the system.

Users complain that finding open tasks assigned to them requires downloading hundreds of tasks and manually filtering the results.

#### Objective

Understand why filtering exists.

#### Success Criteria

The learner should be able to:

- explain why returning all data is problematic
- describe how filtering improves API usability
- identify situations where filtering would be valuable

#### Hints

??? tip "Small Hint"

    Do users usually want every record?

??? tip "Stronger Hint"

    Think about what happens as the number of tasks grows.

??? tip "Almost There"

    Filtering allows consumers to request only relevant data.

#### Solution

??? success "Solution"

    Returning all tasks becomes increasingly inefficient as datasets grow.

    Filtering allows users to retrieve only:

    - open tasks
    - their own tasks
    - tasks with a specific priority

    This improves usability, performance, and efficiency.

#### Why This Exercise Exists

Many API capabilities exist because growth creates problems.

Filtering is one of the first capabilities that becomes necessary when datasets become larger than a consumer can comfortably process.

---

### Apply

#### Scenario

You are designing a products API.

Consumers want to retrieve products by:

- category
- price range
- availability

#### Objective

Design a filtering strategy.

#### Success Criteria

The learner should be able to:

- identify useful filterable fields
- distinguish between exact-match and range filters
- justify filtering decisions

#### Hints

??? tip "Small Hint"

    Which fields are consumers most likely to use when browsing products?

??? tip "Stronger Hint"

    Some fields are categories while others represent numerical ranges.

??? tip "Almost There"

    Category and availability work well as exact filters. Price often benefits from range filtering.

#### Solution

??? success "Solution"

    A reasonable design might allow:

    - category filtering
    - availability filtering
    - minimum price filtering
    - maximum price filtering

    This provides flexibility without exposing every field for filtering.

#### Why This Exercise Exists

API engineers must intentionally choose which filtering capabilities provide value.

Effective filtering is a design decision, not simply a technical feature.

---

### Compose

#### Scenario

You are building an employee directory API.

Consumers want to:

- filter by department
- filter by office location
- filter by employment status
- sort results
- paginate large result sets

#### Objective

Design a complete data discovery experience.

#### Success Criteria

The learner should be able to:

- identify how filtering interacts with other capabilities
- design a consistent request experience
- reason about tradeoffs

#### Hints

??? tip "Small Hint"

    Filtering rarely exists by itself.

??? tip "Stronger Hint"

    Large datasets usually require pagination.

??? tip "Almost There"

    Think about how filtering, sorting, and pagination work together.

#### Solution

??? success "Solution"

    A complete solution combines:

    - filtering to narrow results
    - sorting to organize results
    - pagination to limit response size

    These capabilities work together to help users discover data efficiently.

#### Why This Exercise Exists

Real-world APIs rarely use capabilities in isolation.

Professional API design often involves combining multiple capabilities into a coherent experience.

---

### Automate

#### Scenario

Your organization operates dozens of APIs.

Many teams have implemented filtering differently:

- inconsistent parameter names
- inconsistent date formats
- inconsistent error behavior

Consumers find the APIs difficult to learn.

#### Objective

Identify reusable filtering patterns.

#### Success Criteria

The learner should be able to:

- identify consistency problems
- define reusable filtering conventions
- explain why standardization matters

#### Hints

??? tip "Small Hint"

    Do all APIs behave the same way?

??? tip "Stronger Hint"

    Consumers benefit when patterns are predictable.

??? tip "Almost There"

    Consistent filtering conventions reduce learning effort across APIs.

#### Solution

??? success "Solution"

    The organization should establish common standards for:

    - parameter naming
    - date formats
    - range filters
    - error responses
    - filtering behavior

    Consistency improves maintainability and consumer experience across the API ecosystem.

#### Why This Exercise Exists

As organizations scale, consistency becomes increasingly important.

Reusable filtering patterns help teams create predictable APIs and reduce friction for API consumers.

---

### Why This Matters

Filtering solves the problem of data overload.

Without filtering, consumers often receive more information than they need.

Filtering helps API engineers create APIs that are:

- easier to use
- more efficient
- more scalable
- more maintainable

A professional API does not simply expose data.

It provides consumers with practical ways to discover the data they actually need.

## Framework Implementations

Implement This Capability Using:

✅ FastAPI

⬜ Express

⬜ ASP.NET

⬜ Spring Boot

Next Implementation:

👉 FastAPI Filtering
