# Versioning

Versioning is a capability that allows APIs to evolve without unexpectedly breaking existing consumers.

As APIs mature, requirements change:

- fields are added
- fields are removed
- endpoints change
- business rules evolve

Versioning exists because APIs are long-lived systems.

An API may have consumers that depend on its behavior for months or years.

API engineers should care about versioning because it helps balance innovation with stability.

---

### The Problem

Imagine a customer management API.

An endpoint currently returns:

```json
{
  "id": 123,
  "name": "Jane Doe"
}
```

Many applications consume this API.

Months later, the team decides to replace:

```json
"name"
```

with:

```json
"first_name",
"last_name"
```

The change seems reasonable.

However:

```text
Existing applications fail
        ↓
Unexpected errors occur
        ↓
Customers become frustrated
```

The API team improved the API.

But they also broke existing consumers.

Versioning exists because APIs often need to evolve while continuing to support systems that depend on previous behavior.

---

### Why

Versioning improves:

✅ Reliability

Consumers can continue using known behavior.

✅ Stability

Changes can be introduced more safely.

✅ Maintainability

API evolution becomes more structured.

✅ Consumer Trust

Consumers are less likely to experience unexpected breaking changes.

✅ Long-Term Sustainability

APIs can adapt to new requirements without constantly disrupting users.

---

### Key Concepts

#### API Evolution

APIs rarely remain unchanged.

Over time teams may:

- add features
- improve designs
- fix mistakes
- support new requirements

Versioning provides a strategy for managing these changes.

---

#### Breaking Changes

A breaking change is a change that causes existing consumers to stop working correctly.

Examples:

```text
Removing a field

Renaming a field

Removing an endpoint

Changing request formats

Changing response formats
```

Breaking changes are one of the primary reasons versioning exists.

---

#### Non-Breaking Changes

Some changes can be introduced without affecting consumers.

Examples:

```text
Adding a new optional field

Adding a new endpoint

Adding new filtering options
```

These changes often do not require a new API version.

API engineers should understand the difference.

---

#### Backward Compatibility

Backward compatibility means existing consumers continue to work after changes are introduced.

Example:

```text
Version 1 clients continue functioning

While

Version 2 introduces improvements
```

Maintaining backward compatibility can reduce disruption for consumers.

---

#### Multiple Versions

Organizations frequently support more than one version at a time.

Conceptually:

```text
Version 1
    ↓
Legacy consumers

Version 2
    ↓
New consumers
```

This allows migration to occur gradually rather than all at once.

---

#### Deprecation

Older versions rarely live forever.

API teams often:

```text
Announce deprecation
        ↓
Provide migration guidance
        ↓
Retire older versions
```

Versioning is not only about creating versions.

It is also about managing their lifecycle.

---

#### Versioning Strategies

There are multiple ways to expose API versions.

Common approaches include:

```text
Version in URL

Version in headers

Version in content negotiation
```

Each strategy has advantages and tradeoffs.

The specific implementation is less important than maintaining a clear and consistent approach.

---

#### Communication

Versioning is partly a technical problem.

It is also a communication problem.

Consumers need to know:

- what changed
- why it changed
- when older versions will be retired
- how to migrate

Good communication is often as important as the versioning mechanism itself.

---

### Discover

#### Scenario

Your organization provides an API used by dozens of customer applications.

The API team wants to rename several response fields to improve clarity.

#### Objective

Understand why versioning exists.

#### Success Criteria

The learner should be able to:

- explain why breaking changes are risky
- describe how versioning protects consumers
- recognize when API evolution creates compatibility concerns

#### Hints

??? tip "Small Hint"

    What happens if an application expects data that no longer exists?

??? tip "Stronger Hint"

    Not all consumers can update immediately.

??? tip "Almost There"

    Versioning allows APIs to evolve while minimizing disruptions.

#### Solution

??? success "Solution"

    Renaming response fields may break existing applications.

    Versioning allows newer behavior to be introduced without immediately removing older behavior.

    This provides consumers with time to adapt and migrate.

#### Why This Exercise Exists

APIs often serve many consumers with different update schedules.

Versioning exists largely to manage change responsibly.

---

### Apply

#### Scenario

You are designing a public API expected to be used by external partners.

The API will likely evolve over time.

#### Objective

Determine when versioning should be considered.

#### Success Criteria

The learner should be able to:

- identify changes that may impact consumers
- distinguish between breaking and non-breaking changes
- explain why versioning may be necessary

#### Hints

??? tip "Small Hint"

    Consider changes to request and response formats.

??? tip "Stronger Hint"

    Not every change requires a new version.

??? tip "Almost There"

    Versioning becomes most important when changes could cause existing integrations to fail.

#### Solution

??? success "Solution"

    Versioning should be considered whenever changes may break existing consumers.

    Examples include:

    - removing fields
    - changing field names
    - altering request structures
    - removing endpoints

    Non-breaking additions may not require a new version.

#### Why This Exercise Exists

API engineers must understand which changes affect compatibility and which changes can safely be introduced within an existing version.

---

### Compose

#### Scenario

You are designing an API platform with:

- authentication
- filtering
- pagination
- reporting endpoints

The platform serves both internal teams and external customers.

New requirements are expected every year.

#### Objective

Design an API evolution strategy.

#### Success Criteria

The learner should be able to:

- reason about long-term API evolution
- identify compatibility risks
- balance innovation with stability

#### Hints

??? tip "Small Hint"

    Consider how consumers will handle future changes.

??? tip "Stronger Hint"

    Different consumers may upgrade at different times.

??? tip "Almost There"

    A successful strategy allows improvements while preserving stability for existing users.

#### Solution

??? success "Solution"

    A reasonable strategy might:

    - define versioning rules
    - identify what constitutes a breaking change
    - support version transitions
    - establish deprecation procedures
    - communicate changes clearly

    This helps the platform evolve without creating unnecessary disruption.

#### Why This Exercise Exists

Real-world APIs must balance technical improvement with consumer stability.

Versioning is an important part of achieving that balance.

---

### Automate

#### Scenario

An organization operates dozens of APIs.

Each team handles versioning differently:

- different version naming
- different deprecation policies
- different migration processes

Consumers find the ecosystem confusing.

#### Objective

Identify reusable versioning patterns.

#### Success Criteria

The learner should be able to:

- identify inconsistency problems
- define common versioning conventions
- explain the value of standardized approaches

#### Hints

??? tip "Small Hint"

    Consider what happens when every team invents its own rules.

??? tip "Stronger Hint"

    Consistency reduces consumer confusion.

??? tip "Almost There"

    Shared versioning standards improve maintainability and predictability.

#### Solution

??? success "Solution"

    The organization should establish standards for:

    - version naming
    - version lifecycles
    - deprecation periods
    - migration guidance
    - communication processes

    Consistent versioning practices improve both developer experience and maintainability.

#### Why This Exercise Exists

As API ecosystems grow, consistency becomes increasingly important.

Reusable versioning patterns help both API consumers and API engineering teams.

---

### Why This Matters

Versioning solves the problem of API evolution.

Without versioning, changes can unexpectedly break existing consumers and reduce confidence in the platform.

Versioning helps API engineers:

- manage breaking changes
- support long-lived consumers
- evolve APIs responsibly
- communicate change effectively

Professional APIs rarely remain static.

They change, improve, and adapt over time.

Versioning provides a structured way to manage that evolution while balancing innovation with stability.

## Framework Implementations

Implement This Capability Using:

✅ FastAPI

⬜ Express

⬜ ASP.NET

⬜ Spring Boot

Next Implementation:

👉 FastAPI Versioning
