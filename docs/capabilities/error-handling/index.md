# Error Handling

Error handling is the capability of communicating failures in a predictable, useful, and consistent way.

Every API will encounter situations where it cannot successfully complete a request:

- A resource does not exist
- Input is invalid
- Authentication fails
- Authorization is denied
- A dependency is unavailable
- An unexpected system error occurs

Error handling exists to help API consumers understand what went wrong and what they should do next.

As APIs grow larger and serve more consumers, error handling becomes increasingly important because developers often spend more time diagnosing failures than reading successful responses.

Good error handling improves:

- Developer experience
- Reliability
- Maintainability
- API usability
- System observability

Professional APIs treat errors as first-class API responses rather than afterthoughts.

---

## The Problem

Imagine an API that returns user information.

A client requests:

```http
GET /users/42
```

but user 42 does not exist.

Which response is better?

Option A:

```json
{}
```

Option B:

```json
{
  "message": "Error"
}
```

Option C:

```json
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User 42 does not exist"
  }
}
```

Option C provides useful information.

The client immediately understands:

- What happened
- Why it happened
- What failed

Without clear error handling, API consumers are forced to guess what went wrong.

As systems become larger, inconsistent or vague errors make APIs significantly harder to use and maintain.

Error handling exists because failures are inevitable and clients need useful feedback.

---

## Why

### Reliability

Errors allow systems to fail predictably instead of behaving unpredictably.

Clients should always know when an operation has failed.

### Maintainability

Consistent error structures make APIs easier to evolve and support over time.

Developers learn one pattern and can apply it everywhere.

### Consistency

Consumers should not need to learn a different error format for every endpoint.

A predictable API is easier to understand.

### Security

Good error handling communicates failures without exposing sensitive internal details.

For example:

✅ "Authentication failed"

instead of:

❌ "JWT signature validation failed in auth_service.py"

### User Experience

Human developers consume APIs.

Clear and actionable errors reduce frustration and improve productivity.

---

## Key Concepts

### Errors Are Normal

Failures are not exceptional in real systems.

Missing resources, invalid input, expired tokens, and service outages occur regularly.

Good APIs are designed with failure in mind.

---

### Client Errors

Client errors occur when the request cannot be processed because of something the client did.

Examples:

- Invalid input
- Missing required fields
- Invalid credentials
- Access denied
- Resource not found

The client can potentially fix these issues.

---

### Server Errors

Server errors occur when something unexpected happens within the system.

Examples:

- Database failures
- Service outages
- Unexpected exceptions
- Infrastructure issues

Clients usually cannot resolve these problems themselves.

---

### Error Consistency

A consistent error structure makes APIs easier to consume.

For example:

```json
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User 42 does not exist"
  }
}
```

and

```json
{
  "error": {
    "code": "INVALID_EMAIL",
    "message": "Email format is invalid"
  }
}
```

Both follow the same structure.

Consistency reduces cognitive load for API consumers.

---

### Actionable Errors

A good error helps the consumer understand what to do next.

Good:

```json
{
  "error": {
    "code": "INVALID_EMAIL",
    "message": "Email must contain @"
  }
}
```

Less useful:

```json
{
  "message": "Request failed"
}
```

Useful errors guide consumers toward successful requests.

---

### Error Design

When designing error responses, consider:

- Is the error clear?
- Is the error consistent?
- Is the error actionable?
- Is the error safe to expose?
- Would another developer understand it quickly?

Error handling is ultimately a communication problem.

---

# Discover

## Scenario

A client requests:

```http
GET /products/9999
```

but product 9999 does not exist.

The API team is debating how the API should respond.

Possible responses include:

```json
{}
```

```json
{
  "message": "Error"
}
```

```json
{
  "error": {
    "code": "PRODUCT_NOT_FOUND",
    "message": "Product 9999 does not exist"
  }
}
```

## Objective

Understand why clear error responses are important.

## Success Criteria

The learner should be able to:

- Explain why error handling exists
- Identify weaknesses in vague error responses
- Recognize characteristics of useful error responses

## Hints

??? tip "Small Hint"

    Imagine you are consuming the API rather than building it.

??? tip "Stronger Hint"

    Which response helps you understand exactly what failed?

??? tip "Almost There"

    The best response clearly explains the problem without requiring guesswork.

## Solution

??? success "Solution"

    The third response is the most useful.

    It clearly communicates:

    - What failed
    - Why it failed
    - Which resource caused the problem

    Clear communication is one of the primary goals of error handling.

## Why This Exercise Exists

Many developers initially think about successful API responses.

Professional API engineers must also think about failure scenarios.

Understanding this mindset shift is the foundation of effective error handling.

---

# Apply

## Scenario

A new User API supports:

- Create user
- Get user
- Delete user

The team has created three completely different error formats for different endpoints.

Consumers complain that error handling is inconsistent and difficult to understand.

## Objective

Design a consistent error response approach.

## Success Criteria

The learner should be able to:

- Identify inconsistency problems
- Create a shared error structure
- Explain why consistency improves usability

## Hints

??? tip "Small Hint"

    Think about how consumers learn APIs.

??? tip "Stronger Hint"

    Consistency often matters more than perfection.

??? tip "Almost There"

    All endpoints should communicate errors using a predictable structure.

## Solution

??? success "Solution"

    A shared error structure could be used across all endpoints.

    Example:

    ```json
    {
      "error": {
        "code": "ERROR_CODE",
        "message": "Human readable explanation"
      }
    }
    ```

    Consistency allows consumers to handle errors more easily and reduces confusion.

## Why This Exercise Exists

Many real-world API problems come from inconsistency rather than technical complexity.

Professional APIs prioritize predictable behaviour.

---

# Compose

## Scenario

A Book Store API supports:

- User accounts
- Authentication
- Book searches
- Purchasing

You must design error responses for:

- Book not found
- Invalid email
- Authentication failure
- Purchase denied

The responses should feel like part of a single system.

## Objective

Create a coherent error handling strategy.

## Success Criteria

The learner should be able to:

- Design multiple related error responses
- Maintain consistency
- Differentiate between failure types
- Create meaningful error messages

## Hints

??? tip "Small Hint"

    Start by defining a shared structure.

??? tip "Stronger Hint"

    Error codes can help classify failures.

??? tip "Almost There"

    Every error should look like it belongs to the same API.

## Solution

??? success "Solution"

    A possible approach:

    ```json
    {
      "error": {
        "code": "BOOK_NOT_FOUND",
        "message": "The requested book does not exist"
      }
    }
    ```

    ```json
    {
      "error": {
        "code": "INVALID_EMAIL",
        "message": "Email format is invalid"
      }
    }
    ```

    ```json
    {
      "error": {
        "code": "AUTHENTICATION_FAILED",
        "message": "Credentials are invalid"
      }
    }
    ```

    ```json
    {
      "error": {
        "code": "PURCHASE_DENIED",
        "message": "Purchase cannot be completed"
      }
    }
    ```

    Each error follows a consistent structure while communicating a different problem.

## Why This Exercise Exists

Real APIs rarely contain a single endpoint.

Engineers must design solutions that remain coherent across an entire system.

---

# Automate

## Scenario

Your organization operates twenty APIs maintained by multiple teams.

Each team currently creates its own error format.

Developers struggle because every API communicates failures differently.

The architecture team wants to establish a shared error standard.

## Objective

Identify reusable error handling patterns.

## Success Criteria

The learner should be able to:

- Think beyond a single endpoint
- Design organization-wide standards
- Identify reusable patterns
- Understand the value of standardization

## Hints

??? tip "Small Hint"

    Think about what every API has in common.

??? tip "Stronger Hint"

    Error structures can become organizational standards.

??? tip "Almost There"

    Reusable patterns reduce duplication and improve consistency.

## Solution

??? success "Solution"

    A shared organization-wide format could define:

    - Error code
    - Error message
    - Optional documentation link
    - Optional details field

    Example:

    ```json
    {
      "error": {
        "code": "RESOURCE_NOT_FOUND",
        "message": "Requested resource does not exist"
      }
    }
    ```

    Every API would use the same structure regardless of implementation technology.

## Why This Exercise Exists

API engineers eventually work beyond individual endpoints and services.

They help create reusable standards that improve consistency across many teams and systems.

---

# Why This Matters

Error handling solves the problem of communicating failures.

Without it:

- APIs become difficult to understand
- Debugging becomes frustrating
- Consumers must guess what went wrong
- Systems become less maintainable

Professional API engineers recognize that failures are an expected part of every system.

Good error handling helps APIs become:

- More reliable
- More understandable
- More maintainable
- More consistent
- More enjoyable to use

A well-designed API does not simply return successful responses.

It communicates failures clearly and predictably.

---

# Framework Implementations

Implement This Capability Using:

✅ FastAPI  
⬜ Express  
⬜ ASP.NET  
⬜ Spring Boot

Available Implementations:

- FastAPI Error Handling
