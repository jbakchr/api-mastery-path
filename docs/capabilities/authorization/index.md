# Authorization

Authorization is the capability of determining what an authenticated identity is allowed to do.

Once an API knows who is making a request, it must often answer a second question:

```text
What is this caller allowed to do?
```

Authorization exists to answer that question.

Most real-world systems contain functionality that should not be available to every user.

Examples include:

- Administrative actions
- Sensitive data
- Financial information
- Internal resources
- User-specific content

Professional API engineers need to understand authorization because verifying identity alone is not enough to protect systems.

Authentication establishes who a caller is.

Authorization determines what they can access.

---

## The Problem

Imagine an API used by a company.

Employees can access:

```http
GET /employees/profile
```

Managers can access:

```http
GET /employees/payroll
```

System administrators can access:

```http
DELETE /employees/42
```

The API already supports authentication.

Every caller must sign in before using the API.

However, a problem remains.

An authenticated employee could attempt to access payroll information or delete employee records.

The API knows who the user is.

But it does not yet know what they should be allowed to do.

Authorization exists because authenticated users often have different levels of access.

---

## Why

### Reliability

Authorization ensures requests are handled according to defined access rules.

Systems behave more predictably when permissions are enforced consistently.

---

### Maintainability

Well-designed authorization models make it easier to manage access as systems grow.

Permissions can evolve without redesigning entire APIs.

---

### Consistency

Users and applications should encounter the same authorization rules regardless of which endpoint they access.

Consistent enforcement improves trust and predictability.

---

### Security

Authorization protects:

- Sensitive data
- Administrative functions
- Internal operations
- User-specific resources

Without authorization, authenticated users may gain inappropriate access.

---

### User Experience

Good authorization helps users access the resources they need while preventing access to resources they should not use.

The goal is protection without unnecessary confusion.

---

## Key Concepts

### Authentication vs Authorization

These concepts are related but different.

Authentication answers:

```text
Who are you?
```

Authorization answers:

```text
What are you allowed to do?
```

Authorization typically depends on successful authentication.

---

### Permissions

Permissions define specific actions that are allowed.

Examples:

```text
Create User
Delete User
View Reports
Manage Billing
```

Permissions often represent business capabilities.

---

### Roles

Roles group permissions together.

Examples:

```text
Employee
Manager
Administrator
```

Roles simplify permission management.

Instead of assigning individual permissions to every user, permissions can be assigned to roles.

---

### Resource Ownership

Many APIs allow users to access resources they own.

Examples:

```text
My Tasks
My Orders
My Documents
```

Ownership is a common authorization strategy.

A user may be authenticated but still restricted from viewing another user's resources.

---

### Principle of Least Privilege

Users should have only the access necessary to perform their tasks.

Example:

```text
Read access
```

should not automatically imply:

```text
Delete access
```

Limiting permissions reduces risk.

---

### Access Control

Access control refers to the rules used to determine whether actions should be allowed.

Authorization is ultimately about enforcing access control decisions.

---

# Discover

## Scenario

A company provides an API for customer management.

The API requires authentication.

Every employee can log in successfully.

The API also allows customer deletion:

```http
DELETE /customers/42
```

No additional access checks exist.

## Objective

Understand why authorization exists.

## Success Criteria

The learner should be able to:

- Explain the difference between authentication and authorization
- Identify authorization risks
- Describe why identity alone is insufficient

## Hints

??? tip "Small Hint"

    The API knows who the user is.

??? tip "Stronger Hint"

    Ask whether every authenticated user should have identical access.

??? tip "Almost There"

    Authentication proves identity. Authorization determines permissions.

## Solution

??? success "Solution"

    Authentication confirms the user's identity.

    Authorization determines whether that identity should be allowed to perform a specific action.

    Without authorization, authenticated users may gain access to functionality that should remain restricted.

## Why This Exercise Exists

A common misconception is that authentication alone protects an API.

Real systems often require additional authorization decisions after identity has been established.

---

# Apply

## Scenario

A team is designing access rules for an internal API.

Users belong to one of three roles:

```text
Employee
Manager
Administrator
```

The API supports:

```text
View Personal Profile
View Payroll Data
Delete Employee Records
```

## Objective

Apply authorization concepts to API design.

## Success Criteria

The learner should be able to:

- Match permissions to roles
- Explain authorization decisions
- Recognize access control requirements

## Hints

??? tip "Small Hint"

    Some actions are more sensitive than others.

??? tip "Stronger Hint"

    Consider which role genuinely needs each capability.

??? tip "Almost There"

    Permissions should align with responsibilities.

## Solution

??? success "Solution"

    One possible approach:

    - Employee → View Personal Profile
    - Manager → View Personal Profile, View Payroll Data
    - Administrator → View Personal Profile, View Payroll Data, Delete Employee Records

    Authorization decisions should follow business requirements and security needs.

## Why This Exercise Exists

API engineers frequently design role-based access systems.

Understanding how permissions map to responsibilities is an important skill.

---

# Compose

## Scenario

You are designing a Task Management API.

Each user can:

- Create tasks
- View their own tasks
- Update their own tasks

Administrators can:

- View all tasks
- Delete any task

## Objective

Design a complete authorization strategy.

## Success Criteria

The learner should be able to:

- Define access rules
- Distinguish ownership from administrative access
- Combine multiple authorization concepts

## Hints

??? tip "Small Hint"

    Not every authorization rule requires roles.

??? tip "Stronger Hint"

    Ownership can be a form of authorization.

??? tip "Almost There"

    Consider how users and administrators should differ.

## Solution

??? success "Solution"

    The API could use a combination of ownership and role-based authorization.

    Users:

    - Create tasks
    - View their own tasks
    - Update their own tasks

    Administrators:

    - Access all tasks
    - Delete any task

    Combining authorization strategies often creates more flexible systems.

## Why This Exercise Exists

Real-world APIs frequently require multiple authorization approaches working together.

---

# Automate

## Scenario

Your organization operates dozens of APIs.

Each team creates authorization rules independently.

Some use roles.

Some use permissions.

Some use custom solutions.

Developers struggle because authorization behaves differently across systems.

## Objective

Identify reusable authorization patterns.

## Success Criteria

The learner should be able to:

- Recognize authorization inconsistencies
- Identify common patterns
- Think about authorization across many systems
- Explain the value of standardization

## Hints

??? tip "Small Hint"

    Think beyond a single API.

??? tip "Stronger Hint"

    Shared authorization models reduce complexity.

??? tip "Almost There"

    Consistent authorization approaches are easier to maintain and understand.

## Solution

??? success "Solution"

    An organization may standardize:

    - Role definitions
    - Permission naming
    - Access review processes
    - Authorization strategies

    Shared standards help create predictable and maintainable systems.

## Why This Exercise Exists

Professional API engineers often contribute to security and access standards that extend beyond individual APIs.

Authorization becomes more valuable when handled consistently.

---

# Why This Matters

Authorization solves the problem of controlling access after identity has been established.

Without authorization:

- Authenticated users may gain excessive access
- Sensitive data may be exposed
- Administrative functionality may be misused
- Security risks increase

Professional API engineers use authorization to enforce business rules and protect resources.

Authorization helps provide:

- Access control
- Data protection
- Security
- Consistency
- Trust

Authentication determines who a caller is.

Authorization determines what that caller is allowed to do.

Together, they form a foundational part of secure API design.

---

# Framework Implementations

Implement This Capability Using:

✅ FastAPI  
⬜ Express  
⬜ ASP.NET  
⬜ Spring Boot

Available Implementations:

- FastAPI Authorization
