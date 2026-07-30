# Validation

Validation is one of the most important capabilities in API Engineering.

Every API accepts data from external clients:

- Users
- Frontend applications
- Mobile apps
- Other APIs
- Automated systems

Without validation, invalid or unexpected data can enter the system and cause errors, security issues, and unreliable behavior.

The goal of validation is to ensure that incoming and outgoing data matches the expectations of the API.

---

## The Problem

Imagine a user registration API.

A client sends:

```json
{
  "email": "not-an-email",
  "age": -5
}
```

Should this data be accepted?

Most likely not.

Without validation:

- Bad data enters the database
- Bugs become harder to diagnose
- Unexpected behavior occurs
- Security risks may increase

Validation helps detect problems as early as possible.

---

## Why Validation Matters

Validation helps ensure that:

✅ Required fields are present

✅ Data has the correct type

✅ Values are within acceptable ranges

✅ Business rules are enforced

✅ Consumers receive useful error messages

A well-designed API makes invalid states difficult to create.

---

## Common Validation Rules

Many APIs use validation rules such as:

### Required Fields

A field must be provided.

Example:

```json
{
  "username": ""
}
```

The API may require:

```text
username is required
```

---

### Data Types

A value must be a specific type.

Examples:

```text
name → string

age → integer

active → boolean
```

---

### Length Constraints

A value must be a certain length.

Examples:

```text
Password must be at least 12 characters.
```

```text
Username must be between 3 and 50 characters.
```

---

### Range Constraints

A value must be within an acceptable range.

Examples:

```text
Age must be greater than 0.
```

```text
Percentage must be between 0 and 100.
```

---

### Format Validation

A value must follow a specific format.

Examples:

```text
Email addresses

Phone numbers

URLs

Dates
```

---

## Request Validation

Request validation occurs when data enters the API.

Example:

```http
POST /users
```

Request body:

```json
{
  "name": "Jonas",
  "email": "jonas@example.com"
}
```

The API verifies that:

- Required fields exist
- Types are correct
- Formats are valid
- Business rules are satisfied

Only then is processing allowed to continue.

Request validation is often the most visible form of validation within APIs.

---

## Response Validation

Validation can also occur when data leaves the API.

Example:

```json
{
  "id": 123,
  "email": "jonas@example.com"
}
```

The API may validate the response before sending it to clients.

Benefits include:

- Consistent responses
- Fewer contract violations
- More reliable API consumers

Response validation is especially useful for large systems where multiple components generate data.

---

## Discover Exercise

### Scenario

Your company has a user registration API.

Users occasionally submit invalid data:

```json
{
  "email": "not-an-email",
  "age": -5
}
```

The API currently accepts the request.

### Objective

Identify which fields should be validated.

### Success Criteria

You should be able to:

- identify invalid fields
- explain why they are invalid
- suggest appropriate validation rules

### Hints

??? tip "Small Hint"

    Think about which values might cause problems if stored in a database.

??? tip "Stronger Hint"

    Consider email formats and numeric ranges.

??? tip "Almost There"

    A valid email should look like an email address.
    Age typically should not be negative.

### Solution

??? success "Solution"

    Validation rules could include:

    - Email must follow a valid email format
    - Age must be greater than or equal to 0

### Why This Exercise Exists

Validation begins by recognizing bad data before thinking about implementation.

---

## Apply Exercise

### Scenario

You are designing an API for registering new users.

Each user contains:

- username
- email
- age

### Objective

Create a validation plan.

### Success Criteria

Define:

- required fields
- length rules
- format rules
- range rules

??? tip "Small Hint"

    Start by deciding which fields are mandatory.

??? tip "Stronger Hint"

    Consider username length and email format requirements.

??? tip "Almost There"

    Think about realistic limits such as:

    - username length
    - minimum age
    - email format

### Solution

??? success "Solution"

    Example rules:

    - username required
    - username 3-50 characters
    - email required
    - email must be valid
    - age must be >= 0

### Why This Exercise Exists

API engineers often design validation before writing code.

---

## Compose Exercise

### Scenario

You are designing a blog platform API.

A blog post contains:

- title
- content
- author
- tags

### Objective

Design a complete validation strategy.

### Success Criteria

Define:

- required fields
- length constraints
- business rules
- response validation needs

### Hints

??? tip "Small Hint"

    Think about which fields should never be empty.

??? tip "Stronger Hint"

    Consider title length and minimum content length.

??? tip "Almost There"

    Blog posts with empty titles or content probably should not be accepted.

### Solution

??? success "Solution"

    Example:

    - title required
    - title max 200 characters
    - content required
    - content min 50 characters
    - tags optional
    - maximum 10 tags

### Why This Exercise Exists

Real APIs typically validate multiple related fields simultaneously.

---

## Automate

### Scenario

Your organization has:

- User APIs
- Product APIs
- Billing APIs
- Order APIs

All teams repeatedly create similar validation rules.

### Objective

Identify validation patterns that could be standardized.

### Success Criteria

Suggest:

- reusable rules
- shared validation approaches
- consistency improvements

### Hints

??? tip "Small Hint"

    Think about data types commonly used across services.

??? tip "Stronger Hint"

    Consider IDs, emails and timestamps.

??? tip "Almost There"

    Many APIs validate the same concepts repeatedly.

### Solution

??? success "Solution"

    Examples:

    - shared email validation
    - shared UUID validation
    - shared pagination rules
    - shared timestamp formats

### Why This Exercise Exists

Strong API ecosystems rely on consistent validation patterns.

---

## Why This Matters

Validation is often one of the first lines of defense in an API.

Strong validation helps create APIs that are:

- Predictable
- Reliable
- Easier to use
- Easier to maintain

Regardless of framework, every API engineer eventually faces the same challenge:

```text
How do I ensure that incoming and outgoing
data is valid?
```

Learning validation is therefore not about learning a framework feature.

It is about learning a fundamental API Engineering capability.

---

# Framework Implementations

Implement This Capability Using

✅ ../../implementations/fastapi/validation/

⬜ Express

⬜ ASP.NET

⬜ Spring Boot
