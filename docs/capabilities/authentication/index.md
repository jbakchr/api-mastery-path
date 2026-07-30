# Authentication

Authentication is the capability of verifying the identity of a caller.

When an API receives a request, it must often answer a simple but important question:

```text
Who is making this request?
```

Authentication exists to provide a reliable way to answer that question.

Without authentication, systems cannot determine whether requests originate from trusted users, applications, or services.

Authentication is one of the foundational security capabilities of modern APIs.

Professional API engineers need to understand authentication because most real-world APIs protect data, functionality, or resources that should not be accessible to everyone.

---

## The Problem

Imagine a Payroll API that supports the following endpoint:

```http
GET /employees/payroll
```

The endpoint returns salary information for employees.

The API works.

The endpoint returns correct data.

However, there is a problem.

Anyone who discovers the endpoint can access payroll information.

The API has no way of determining:

- Who is making the request
- Whether the caller is a legitimate employee
- Whether the caller should be trusted

The issue is not routing.

The issue is not validation.

The issue is identity.

Authentication exists because APIs often need to verify who is making a request before providing access to protected functionality.

---

## Why

### Reliability

Authentication allows systems to make decisions based on verified identities.

Without authentication, APIs cannot distinguish trusted callers from unknown callers.

---

### Maintainability

Consistent authentication approaches simplify API development and maintenance.

Teams can apply the same authentication strategy across many endpoints and services.

---

### Consistency

Consumers should interact with authentication mechanisms in predictable ways.

Consistent authentication flows improve usability and reduce confusion.

---

### Security

Authentication is one of the primary security controls used by APIs.

It helps protect:

- Sensitive data
- Private resources
- Administrative functionality
- Organizational systems

Without authentication, APIs often become vulnerable to unauthorized access.

---

### User Experience

Well-designed authentication helps legitimate users access APIs successfully while preventing unauthorized access.

The goal is to provide security without unnecessary friction.

---

## Key Concepts

### Identity

Identity answers the question:

```text
Who are you?
```

Authentication exists to establish identity.

An identity may represent:

- A person
- An application
- A service
- A device

---

### Credentials

Credentials are evidence used to prove identity.

Examples include:

- Passwords
- API keys
- Tokens
- Certificates

The specific credential type matters less than the underlying goal:

```text
Prove identity.
```

---

### Authentication vs Authorization

These concepts are closely related but distinct.

Authentication answers:

```text
Who are you?
```

Authorization answers:

```text
What are you allowed to do?
```

Authentication typically happens first.

Authorization decisions often depend on authenticated identity.

---

### Sessions

A session allows a system to remember an authenticated user between requests.

After successful authentication, the system can recognize future requests without requiring the user to repeatedly prove their identity.

---

### Tokens

A token is a credential that can be presented with requests.

Tokens are commonly used in modern APIs because they allow identity information to travel with requests.

---

### Trust

Authentication is fundamentally about trust.

A system receives a request and must decide:

```text
Can I trust the identity being presented?
```

Authentication provides mechanisms for establishing that trust.

---

# Discover

## Scenario

A company creates an API that allows users to delete customer records.

The endpoint is publicly accessible:

```http
DELETE /customers/42
```

No authentication is required.

Anyone who knows the endpoint can submit requests.

## Objective

Understand why authentication exists.

## Success Criteria

The learner should be able to:

- Explain the problem authentication solves
- Identify risks associated with unauthenticated APIs
- Describe why identity matters

## Hints

??? tip "Small Hint"

    Think about who might discover the endpoint.

??? tip "Stronger Hint"

    Consider what prevents unauthorized users from submitting requests.

??? tip "Almost There"

    The API currently has no way to verify who is making the request.

## Solution

??? success "Solution"

    The API cannot determine who is making requests.

    Because identity cannot be verified, unauthorized users may gain access to protected functionality.

    Authentication exists to establish trust and verify identity before protected operations are performed.

## Why This Exercise Exists

Authentication becomes easier to understand when viewed as a solution to a real problem.

The problem is not technology.

The problem is determining who should be trusted.

---

# Apply

## Scenario

A team operates three APIs:

### Public Weather API

Provides public weather forecasts.

### Employee Payroll API

Provides payroll information.

### Administrative Dashboard API

Allows system configuration.

The team must decide where authentication is required.

## Objective

Apply authentication concepts to API design decisions.

## Success Criteria

The learner should be able to:

- Identify when authentication is appropriate
- Evaluate security requirements
- Explain authentication decisions

## Hints

??? tip "Small Hint"

    Not every API requires the same level of protection.

??? tip "Stronger Hint"

    Consider the sensitivity of the information being exposed.

??? tip "Almost There"

    The more sensitive the data or functionality, the more likely authentication is needed.

## Solution

??? success "Solution"

    A possible approach:

    - Public Weather API: Authentication may not be required.
    - Employee Payroll API: Authentication should be required.
    - Administrative Dashboard API: Authentication should be required.

    Authentication decisions are often driven by risk and sensitivity.

## Why This Exercise Exists

API engineers must make practical decisions about when authentication is necessary and why.

---

# Compose

## Scenario

You are designing a Task Management API.

Features include:

- Creating tasks
- Updating tasks
- Deleting tasks
- Viewing personal tasks

Multiple users will use the API.

Each user should access only their own information.

## Objective

Design an authentication strategy.

## Success Criteria

The learner should be able to:

- Identify authentication requirements
- Explain how identities affect API behavior
- Design a coherent authentication approach

## Hints

??? tip "Small Hint"

    Think about how the API identifies users.

??? tip "Stronger Hint"

    The API must know which user is making each request.

??? tip "Almost There"

    User-specific functionality typically requires authenticated identities.

## Solution

??? success "Solution"

    The API should require users to authenticate before accessing personal task data.

    Once authenticated, the API can associate requests with individual identities.

    This enables the API to provide personalized and secure behavior.

## Why This Exercise Exists

Real-world APIs often combine authentication with data ownership and access control concerns.

Authentication is a foundational building block for these systems.

---

# Automate

## Scenario

Your organization operates dozens of APIs.

Each team implements authentication differently.

Some use API keys.

Some use tokens.

Some use custom solutions.

Developers struggle because every API behaves differently.

## Objective

Identify reusable authentication patterns.

## Success Criteria

The learner should be able to:

- Recognize authentication inconsistencies
- Identify opportunities for standardization
- Think about authentication at organizational scale
- Explain the benefits of common approaches

## Hints

??? tip "Small Hint"

    Consistency is valuable for both developers and consumers.

??? tip "Stronger Hint"

    Reusable standards reduce complexity.

??? tip "Almost There"

    Authentication strategies become easier to maintain when teams follow shared patterns.

## Solution

??? success "Solution"

    An organization may establish authentication standards covering:

    - Credential types
    - Identity verification processes
    - Token handling
    - Security expectations

    Shared standards improve consistency and reduce duplication across teams.

## Why This Exercise Exists

Professional API engineers often help establish security patterns that scale across many systems.

Authentication is most effective when approached consistently.

---

# Why This Matters

Authentication solves the problem of verifying identity.

Without authentication:

- APIs cannot reliably determine who is making requests
- Sensitive functionality becomes difficult to protect
- Trust becomes difficult to establish
- Security risks increase

Professional API engineers rely on authentication to build secure and trustworthy systems.

Authentication provides a foundation for:

- Security
- Trust
- Access control
- User-specific experiences
- Protected resources

Before an API can decide what a caller is allowed to do, it must first determine who the caller is.

Authentication is how that identity is established.

---

# Framework Implementations

Implement This Capability Using:

✅ FastAPI  
⬜ Express  
⬜ ASP.NET  
⬜ Spring Boot

Available Implementations:

- FastAPI Authentication
