# Testing

Testing is a capability that allows API engineers to verify that an API behaves as expected.

As APIs grow, changes are constantly introduced:

- new endpoints
- new validations
- new authentication rules
- new business logic

Testing exists because every change introduces risk.

Without testing, API engineers must rely on manual verification and assumptions.

API engineers should care about testing because it provides confidence that APIs continue working correctly as systems evolve.

---

### The Problem

Imagine a task management API.

A developer adds a new validation rule:

```text
Task titles must contain at least 3 characters.
```

The change appears harmless.

The application deploys successfully.

Hours later, users report that creating tasks no longer works.

The application starts returning unexpected errors.

The developer accidentally broke existing functionality.

Nothing in the deployment process detected the issue before release.

Testing exists because APIs change frequently, and every change introduces the possibility of unintended consequences.

---

### Why

Testing improves:

✅ Reliability

Changes can be verified before deployment.

✅ Maintainability

Developers can refactor with greater confidence.

✅ Consistency

Expected behavior becomes documented and repeatable.

✅ Quality

Bugs are discovered earlier.

✅ Developer Confidence

Teams spend less time wondering whether changes broke existing functionality.

---

### Key Concepts

#### Verification

Testing is fundamentally about verification.

A test answers a question such as:

```text
Does this behavior work as expected?
```

Example:

```text
Can users create tasks?

Can invalid data be rejected?

Can unauthorized requests be blocked?
```

---

#### Expected Behavior

A test defines what should happen.

Example:

```text
Given valid input

Expect success
```

or

```text
Given invalid input

Expect a validation error
```

This creates a clear definition of correct behavior.

---

#### Automated Testing

Tests can be executed repeatedly.

Instead of manually checking behavior after every change:

```text
Developer changes code
        ↓
Tests run
        ↓
Problems are detected
```

This greatly reduces risk.

---

#### Regression

A regression occurs when previously working functionality stops working.

Example:

```text
Feature works today

Code changes tomorrow

Feature now fails
```

Testing helps detect regressions before users encounter them.

---

#### Test Coverage

Coverage describes how much of a system is exercised by tests.

Examples:

```text
Routes

Validation

Authentication

Authorization

Business logic
```

High coverage does not guarantee quality.

However, important behavior should generally be tested.

---

#### Happy Paths

A happy path tests expected and successful behavior.

Example:

```text
Valid request

Expected success response
```

These tests verify that the API works when used correctly.

---

#### Edge Cases

Real-world systems encounter unusual situations.

Examples:

```text
Missing fields

Invalid values

Unauthenticated requests

Unexpected input
```

Testing edge cases often reveals issues before users find them.

---

#### Confidence

Perhaps the most important outcome of testing is confidence.

Testing does not prove that software is perfect.

Instead it provides evidence that important behavior continues to function as expected.

---

### Discover

#### Scenario

An API team frequently makes changes to an application.

Before every release they manually test several endpoints.

Sometimes they forget to test important scenarios.

Production issues occur regularly.

#### Objective

Understand why testing exists.

#### Success Criteria

The learner should be able to:

- explain the risks of manual verification alone
- describe how testing reduces change risk
- identify situations where testing provides value

#### Hints

??? tip "Small Hint"

    What happens when developers forget to manually check something?

??? tip "Stronger Hint"

    Systems often become more complex over time.

??? tip "Almost There"

    Testing provides a repeatable way to verify behavior.

#### Solution

??? success "Solution"

    Manual verification depends on people remembering every scenario.

    Testing provides repeatable validation that can be executed whenever changes occur.

    This reduces the likelihood of bugs reaching production.

#### Why This Exercise Exists

Many teams only appreciate testing after experiencing costly regressions.

Understanding why testing exists is more important than learning testing tools.

---

### Apply

#### Scenario

You are designing an API that allows users to create products.

The API requires:

- a product name
- a positive price

#### Objective

Identify what should be tested.

#### Success Criteria

The learner should be able to:

- identify successful scenarios
- identify failure scenarios
- reason about expected behavior

#### Hints

??? tip "Small Hint"

    Think about both valid and invalid requests.

??? tip "Stronger Hint"

    What happens if the price is negative?

??? tip "Almost There"

    Tests should verify both success and failure behavior.

#### Solution

??? success "Solution"

    Useful tests might include:

    - valid product creation
    - missing product name
    - negative price
    - invalid data types

    These tests verify both expected behavior and error handling.

#### Why This Exercise Exists

Professional testing involves thinking carefully about expected and unexpected inputs.

---

### Compose

#### Scenario

You are designing an API with:

- authentication
- authorization
- validation
- filtering
- pagination

#### Objective

Develop a testing strategy.

#### Success Criteria

The learner should be able to:

- identify important areas to test
- combine multiple API capabilities
- reason about risk and confidence

#### Hints

??? tip "Small Hint"

    APIs are usually more than individual endpoints.

??? tip "Stronger Hint"

    Consider what happens when capabilities interact.

??? tip "Almost There"

    Important system behaviors often involve multiple capabilities working together.

#### Solution

??? success "Solution"

    A complete testing strategy might verify:

    - authenticated access
    - authorization rules
    - validation behavior
    - filtering behavior
    - pagination behavior
    - expected error responses

    This provides confidence that the API behaves correctly across multiple capabilities.

#### Why This Exercise Exists

Real-world APIs depend on many capabilities working together.

Testing should reflect how the system is actually used.

---

### Automate

#### Scenario

An organization operates dozens of APIs.

Every team tests differently.

Some teams:

- test manually
- test inconsistently
- skip testing under deadlines

Software quality varies significantly.

#### Objective

Identify reusable testing practices.

#### Success Criteria

The learner should be able to:

- identify testing consistency problems
- recognize reusable testing patterns
- explain the value of standardization

#### Hints

??? tip "Small Hint"

    What happens when every team invents its own process?

??? tip "Stronger Hint"

    Consistent testing practices reduce risk.

??? tip "Almost There"

    Organizations benefit when testing becomes repeatable and predictable.

#### Solution

??? success "Solution"

    The organization should establish shared practices for:

    - automated testing
    - critical API behaviors
    - failure scenarios
    - test execution during development
    - release validation

    Consistent testing improves reliability across the entire API ecosystem.

#### Why This Exercise Exists

Testing scales best when it becomes a repeatable organizational capability rather than an individual developer habit.

---

### Why This Matters

Testing solves the problem of uncertainty.

Without testing, API changes introduce risk that may not be discovered until users encounter failures.

Testing helps API engineers:

- verify behavior
- prevent regressions
- improve reliability
- increase confidence
- evolve systems safely

Professional APIs are rarely built once and left unchanged.

They are continuously improved and modified.

Testing provides the safety net that allows those improvements to happen responsibly.

## Framework Implementations

Implement This Capability Using:

✅ FastAPI

⬜ Express

⬜ ASP.NET

⬜ Spring Boot

Next Implementation:

👉 FastAPI Testing
