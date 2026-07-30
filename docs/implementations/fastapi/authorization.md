# FastAPI Authorization

This page demonstrates how FastAPI implements the concepts introduced in the Authorization capability page.

Before reading this page, it is recommended that you first understand:

- The problem authorization solves
- Why authorization matters
- General API engineering concepts behind authorization

Authorization is a capability.

FastAPI is one way of implementing that capability.

---

## Why

FastAPI provides several tools that make authorization easier to implement and maintain.

Once a user has been authenticated, APIs often need to enforce rules such as:

- Administrators can delete resources
- Managers can view reports
- Users can only access their own data

FastAPI's dependency system makes it easy to centralize authorization decisions and reuse them across many endpoints.

Benefits include:

✅ Reusable authorization logic

✅ Cleaner endpoint code

✅ Better maintainability

✅ Consistent access control

✅ Easier testing

✅ Reduced duplication

✅ Integration with authentication dependencies

Authorization rules can be implemented once and reused throughout the application.

---

## Core Concepts

### Depends

Most authorization logic is implemented using dependencies.

Example:

```python
from fastapi import Depends

@app.get("/reports")
def get_reports(
    user=Depends(require_manager)
):
    ...
```

The dependency determines whether access should be granted.

---

### Current User

Authorization decisions usually depend on the authenticated user.

Example:

```python
def get_current_user():
    ...
```

Authorization typically begins by identifying who is making the request.

---

### Role Checks

A common authorization strategy is role-based access control.

Example:

```python
if user["role"] != "admin":
    ...
```

Roles often represent broad groups of permissions.

Examples:

```text
User
Manager
Administrator
```

---

### Permission Checks

Some systems make decisions based on permissions rather than roles.

Example:

```python
if "delete_user" not in user["permissions"]:
    ...
```

Permissions often represent specific actions.

---

### HTTPException

Unauthorized actions are commonly rejected using `HTTPException`.

Example:

```python
raise HTTPException(
    status_code=403,
    detail="Access denied"
)
```

Authorization failures should communicate that access is not permitted.

---

### Ownership Checks

Many APIs allow users to access their own resources while restricting access to resources owned by others.

Example:

```python
if task.owner_id != user.id:
    ...
```

Ownership is one of the most common authorization patterns.

---

# Discover

## Scenario

You encounter the following code:

```python
from fastapi import FastAPI, Depends

app = FastAPI()


def get_current_user():
    return {
        "username": "jonas",
        "role": "admin"
    }


@app.delete("/users/{user_id}")
def delete_user(
    user_id: int,
    user=Depends(get_current_user)
):
    return {
        "deleted": user_id
    }
```

A user is authenticated before reaching the endpoint.

No authorization checks exist.

## Objective

Understand why authentication alone is not enough.

## Success Criteria

The learner should be able to explain:

- What information is available about the current user
- Why authorization is missing
- What risks may exist

## Hints

??? tip "Small Hint"

    The API knows who the user is.

??? tip "Stronger Hint"

    Consider whether every authenticated user should delete users.

??? tip "Almost There"

    Authentication establishes identity, but access rules are still missing.

## Solution

??? success "Solution"

    The endpoint knows who the caller is because `get_current_user()` provides identity information.

    However, the endpoint never checks whether the user is allowed to delete users.

    Authorization is required to enforce access control decisions.

## Why This Exercise Exists

The goal is to understand the distinction between authentication and authorization.

FastAPI often uses dependencies for both, but they solve different problems.

---

# Apply

## Scenario

The following endpoint should only be accessible by administrators.

```python
@app.delete("/users/{user_id}")
def delete_user(
    user_id: int,
    user=Depends(get_current_user)
):
    return {
        "deleted": user_id
    }
```

## Objective

Add authorization logic.

## Success Criteria

The learner should be able to:

- Check user roles
- Deny access when appropriate
- Return a useful error response

## Hints

??? tip "Small Hint"

    The current user already contains role information.

??? tip "Stronger Hint"

    Use an authorization check before continuing.

??? tip "Almost There"

    Return an error if the role is not allowed.

## Solution

??? success "Solution"

```python
from fastapi import HTTPException


@app.delete("/users/{user_id}")
def delete_user(
    user_id: int,
    user=Depends(get_current_user)
):

    if user["role"] != "admin":
        raise HTTPException(
            status_code=403,
            detail="Access denied"
        )

    return {
        "deleted": user_id
    }
```

The endpoint now verifies authorization before performing the operation.

## Why This Exercise Exists

The goal is to reinforce basic authorization checks and access control patterns.

---

# Compose

## Scenario

A Task API contains:

- View task
- Update task
- Delete task

Users should only access their own tasks.

Administrators should access any task.

## Objective

Combine ownership and role-based authorization.

## Success Criteria

The learner should be able to combine:

- Current user dependencies
- Role checks
- Ownership checks

## Hints

??? tip "Small Hint"

    Not all authorization requires roles.

??? tip "Stronger Hint"

    Ownership can also determine access.

??? tip "Almost There"

    Allow administrators broader access than normal users.

## Solution

??? success "Solution"

```python
def can_access_task(
    task,
    user
):

    if user["role"] == "admin":
        return True

    return task.owner_id == user["id"]
```

The authorization logic now considers:

- Administrative access
- Resource ownership

This creates more realistic access control behavior.

## Why This Exercise Exists

Real-world APIs often combine multiple authorization strategies.

Authorization is rarely solved through a single rule.

---

# Automate

## Scenario

Your organization maintains many FastAPI services.

Different teams implement authorization differently.

Some endpoints check roles directly.

Some check permissions.

Some duplicate the same authorization code repeatedly.

The architecture team wants a reusable approach.

## Objective

Create reusable authorization patterns.

## Success Criteria

The learner should be able to:

- Identify duplication
- Create reusable authorization dependencies
- Standardize authorization behavior

## Hints

??? tip "Small Hint"

    Look for repeated authorization checks.

??? tip "Stronger Hint"

    Dependencies can centralize access control.

??? tip "Almost There"

    Shared authorization functions improve consistency.

## Solution

??? success "Solution"

One possible approach is:

```python
def require_admin(
    user=Depends(get_current_user)
):

    if user["role"] != "admin":
        raise HTTPException(
            status_code=403,
            detail="Access denied"
        )

    return user
```

Endpoints can then use:

```python
@app.delete("/users/{user_id}")
def delete_user(
    user=Depends(require_admin)
):
    ...
```

This creates a reusable authorization pattern that can be applied consistently across services.

## Why This Exercise Exists

Professional APIs typically rely on shared access control patterns rather than implementing authorization separately in every endpoint.

---

## Common Pitfalls

### Confusing Authentication and Authorization

#### Why It Happens

The two concepts are closely related and often appear together.

#### Better Approach

Remember:

```text
Authentication
    ↓
Who are you?

Authorization
    ↓
What are you allowed to do?
```

Treat them as separate concerns.

---

### Checking Roles Inside Every Endpoint

#### Why It Happens

Authorization logic initially seems simple.

#### Better Approach

Move authorization into reusable dependencies.

```python
user = Depends(require_admin)
```

This reduces duplication.

---

### Ignoring Resource Ownership

#### Why It Happens

Developers focus only on roles.

#### Better Approach

Consider ownership rules as well.

Example:

```text
User can edit their own task
```

Ownership is a common authorization strategy.

---

### Using Inconsistent Authorization Rules

#### Why It Happens

Different developers implement access rules differently.

#### Better Approach

Create shared patterns and conventions.

Consistency improves maintainability and predictability.

---

### Returning Incorrect Status Codes

#### Why It Happens

Authentication and authorization failures are easily confused.

#### Better Approach

Understand the distinction:

```text
401 Unauthorized
    ↓
Authentication problem

403 Forbidden
    ↓
Authorization problem
```

Use responses that accurately describe the issue.

---

## Why This Matters

FastAPI provides excellent support for authorization through:

- Depends
- Reusable dependencies
- Current user patterns
- HTTPException
- Authentication integration

These tools help developers implement access control without scattering authorization logic throughout their applications.

As systems grow, authorization becomes increasingly important because different users often require different access levels.

Understanding FastAPI's authorization patterns helps you build APIs that are:

- More secure
- More maintainable
- More predictable
- Easier to scale

Authentication establishes identity.

Authorization enforces access control.

Together, they form a foundation for secure API engineering.

---

## Related Capability

✅ Authorization

Return to the Authorization capability page to review:

- Why authorization exists
- Roles and permissions
- Resource ownership
- Access control
- Authentication vs authorization

The capability page teaches the problem.

This page teaches how FastAPI implements the solution.
