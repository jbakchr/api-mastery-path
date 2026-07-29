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

## Discover

Answer the following questions:

1. What problems can occur if an API performs no validation?

2. Why might validating data close to the API boundary be beneficial?

3. What kinds of fields commonly require format validation?

---

## Apply

Consider a registration endpoint:

```json
{
  "username": "js",
  "email": "invalid-email",
  "age": -3
}
```

Identify all validation issues.

What error messages would you return to the client?

---

## Compose

You are designing an API for creating blog posts.

A blog post contains:

- title
- content
- author
- tags

Think about:

- Which fields are required?
- Which length limits might make sense?
- Which business rules should exist?

Design a validation strategy.

---

## Automate

Imagine your organization has:

- User APIs
- Product APIs
- Order APIs
- Billing APIs

Many validation rules are repeated.

Think about:

- Which validations could be standardized?
- Which validation patterns could be reused?
- How might consistency benefit API consumers?

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
