# FEEDBACK

This initial FEEDBACK.md is structured with thoughts on how things might be improved during the development of this API Mastery Path.

And so, to keep feedback easy to understanding and managable the feedback is/should be structured based on the sort of progression of this project which is this:

1. Fundamentals
2. Capabilities
3. Implementations
4. Projects
5. Production APIs

Hence, every feedback is/should be written according to section some feedback belongs.

## Fundamentals

...

## Capabilities

### Capability Template

Focus:

- Thinking
- Understanding
- Design
- Engineering

Template:

```
The Problem

Concept Explanation

Discover Exercise
Apply Exercise
Compose Exercise
Automate Exercise

Why This Matters

Framework Implementations
```

### Validation

Copilot itself had a suggestion of adding exercises to validation - although it did itself include some which was sort of more "verbal" and not techonology specific.

Having exercises however - or at least in general - seems like a very good idea so that a reader/user of API Mastery Path can practise mastering api development in which way that would be most fitting.

Nevertheless, this idea is one to ponder - should it make sense to create (more) exercises for one to practise validation.

### Example Validation Exercise - Capability Pages

#### Discover Exercise

##### Scenario

You maintain a user registration API.

Users occasionally submit invalid email addresses and negative ages.

The API currently accepts everything.

##### Objective

Identify which fields should be validated.

##### Success Criteria

You should be able to explain:

- what should be validated
- why each rule exists
- what error should be returned

##### Hints

??? tip "Small Hint"
    
    Think about data that could cause problems later.


??? tip "Stronger Hint"

    Consider required fields, email format and numeric ranges.

??? tip "Almost There"

    Email should follow an email format.
    Age probably should not be negative.

##### Solution

??? success "Solution"

    Possible validation rules:

    - Email must be valid
    - Age must be >= 0
    - Name is required

##### Why This Exercise Exists

Understanding what should be validated is more important than learning framework syntax.

## Implementation

### Example Validation Exercise - Implementation Pages

#### Discover

##### Scenario

You are creating a user registration endpoint.

You want FastAPI to automatically reject requests that are missing a username.

##### Objective

Create a Pydantic model that requires:

- username
- email

##### Success Criteria

Requests missing either field should fail validation.

##### Hints

??? tip "Small Hint"

    Use BaseModel.

??? tip "Stronger Hint"

    Create a class inheriting from BaseModel.

??? tip "Almost There"

    Define:

    ```python
    class UserCreate(BaseModel):
        ...
    ```

##### Solution

??? success "Solution"

    ```python
    from pydantic import BaseModel


    class UserCreate(BaseModel):
        username: str
        email: str
    ```

##### Why This Exercise Exists

This introduces FastAPI request validation using required fields.

## Projects

...

## Production APIs

...