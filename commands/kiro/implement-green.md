---
description: Implement minimum code to make tests pass (Green state)
allowed-tools: Bash, Read, Write, Edit, MultiEdit, Update
---

# Implementation (Green State)

Implement minimum viable code to make tests pass for feature: **$ARGUMENTS**

## Context Validation

### Previous Phases Check

- Test specifications: @.kiro/specs/$ARGUMENTS/tests-red.md
- Interface definitions: @.kiro/specs/$ARGUMENTS/interfaces.md
- API specification: @.kiro/specs/$ARGUMENTS/api.md
- Database schema: @.kiro/specs/$ARGUMENTS/schema.md
- Spec metadata: @.kiro/specs/$ARGUMENTS/spec.json

**CRITICAL**: Implementation requires approved test specifications in RED state.

## Task: Implement Minimum Code for GREEN

Generate implementation.md documenting the minimal code to pass tests:

### 1. Implementation Strategy

```markdown
# Implementation Plan: [Feature Name]

## Overview
Minimal implementation to achieve GREEN state for all RED tests.
Focus on making tests pass, not on optimization or elegance.

## Implementation Order
Follow TDD cycle strictly:
1. Run test (verify RED)
2. Write minimum code
3. Run test (achieve GREEN)
4. Commit
5. Move to next test

## Phase 1: Database Implementation

### Create Tables and Migrations
Based on schema.md, implement:
1. Migration files in correct order
2. Primary tables
3. Relationship tables
4. Indexes
5. Constraints

### Seed Data
Minimal data for tests:
- Test users
- Sample records
- Required lookups

## Phase 2: Backend Implementation

### Data Models
Implement models/entities:
- Map to database tables
- Include validations
- Add serialization

### Repository Layer
Minimal CRUD operations:
- Create
- Read by ID
- Update
- Delete (soft)
- List with pagination

### Service Layer
Business logic implementation:
- Calculation methods
- Validation logic
- Transaction handling
- Error handling

### API Controllers
REST endpoints:
- Route definitions
- Request parsing
- Response formatting
- Error responses
- Authentication middleware

## Phase 3: Frontend Implementation

### Data Types
Type definitions:
- Request interfaces
- Response interfaces
- State types
- Props types

### API Client
Service layer:
- HTTP client setup
- API method wrappers
- Error handling
- Response transformation

### State Management
Application state:
- Store setup
- Actions
- Reducers/mutations
- Selectors

### Components
UI implementation:
- Presentational components
- Container components
- Forms
- Lists
- Error boundaries

### Routing
Navigation setup:
- Route definitions
- Guards
- Parameters
- Lazy loading

## Test Execution Log

### Unit Tests - Backend
| Test | Status | Implementation |
|------|--------|---------------|
| Calculate metrics | RED → GREEN | Added calculation method |
| Handle empty data | RED → GREEN | Added null checks |
| Validate parameters | RED → GREEN | Added validation |

### Unit Tests - Frontend
| Test | Status | Implementation |
|------|--------|---------------|
| Component renders | RED → GREEN | Created component |
| Handle interactions | RED → GREEN | Added event handlers |
| Display errors | RED → GREEN | Added error state |

### Integration Tests
| Test | Status | Implementation |
|------|--------|---------------|
| API returns 200 | RED → GREEN | Implemented endpoint |
| Creates resource | RED → GREEN | Added POST handler |
| Authentication | RED → GREEN | Added auth middleware |

### E2E Tests
| Test | Status | Implementation |
|------|--------|---------------|
| Complete journey | RED → GREEN | Full implementation |
| Error recovery | RED → GREEN | Error handling |

## Code Snippets

### Minimal Backend Implementation
[Key code snippets that made tests pass]

### Minimal Frontend Implementation
[Key code snippets that made tests pass]

### Database Queries
[Essential queries implemented]

## Deferred Items (for Refactoring)

Things intentionally not done yet:
- Performance optimization
- Code organization
- Duplicate code removal
- Complex error handling
- Caching
- Logging
- Monitoring
- Documentation

## Test Coverage Report

After implementation:
- Unit tests: X% coverage
- Integration tests: All passing
- E2E tests: Critical paths passing
- Total tests passing: X/Y

## Known Technical Debt

List of shortcuts taken:
1. Hard-coded values that should be configurable
2. Duplicate code between modules
3. Inefficient algorithms
4. Missing edge case handling
5. Incomplete error messages
```

### 2. Implementation Checklist

For each test moving from RED to GREEN:

- [ ] Run test, confirm RED state
- [ ] Write minimal code to pass
- [ ] Run test, confirm GREEN state
- [ ] Commit with message: "GREEN: [test name]"
- [ ] Do NOT refactor yet
- [ ] Move to next RED test

### 3. Minimal Code Principles

Follow these rules:
- **Fake it**: Return hard-coded values if that passes the test
- **Obvious implementation**: Only when truly simple
- **Triangulation**: Generalize only when forced by tests
- **No gold plating**: Don't add anything tests don't require
- **Copy-paste OK**: Duplication is fine at this stage

### 4. Common Patterns for GREEN

#### Return Fixed Values
If test expects specific output, just return it

#### Simple Conditionals
If test has multiple cases, use if/else

#### Basic Loops
If test requires iteration, use simple for loop

#### Direct Database Queries
No need for complex ORMs yet

#### Inline Validation
Put validation right in the method

### 5. Anti-Patterns to Avoid

Do NOT:
- Add features tests don't require
- Optimize performance
- Create abstractions
- Add extensive error handling
- Implement caching
- Add logging
- Create utilities
- Refactor existing code

### 6. Implementation Tracking

Track progress:
```markdown
## Test Progress Tracker

### Current Status: X/Y Tests GREEN

#### In Progress
- [ ] Test: [name] - Working on: [what part]

#### Completed Today
- [x] Test: [name] - GREEN at [timestamp]
- [x] Test: [name] - GREEN at [timestamp]

#### Blocked
- [ ] Test: [name] - Blocked by: [reason]

#### Next Up
1. Test: [name]
2. Test: [name]
3. Test: [name]
```

### 7. Update Metadata

Update spec.json:
```json
{
  "phase": "implementation-green",
  "tests_passing": {
    "unit": [count],
    "integration": [count],
    "e2e": [count],
    "total": "[passing]/[total]"
  },
  "technical_debt_items": [count],
  "approvals": {
    "implementation": {
      "generated": true,
      "tests_green": true,
      "approved": false
    }
  },
  "updated_at": "current_timestamp"
}
```

## Instructions

1. **Start with failing tests** - Confirm RED state
2. **Implement minimal code** - Just enough to pass
3. **Follow test order** - Unit → Integration → E2E
4. **Resist refactoring** - Save for next phase
5. **Use simple solutions** - Complexity comes later
6. **Track each GREEN** - Document what made it pass
7. **Commit frequently** - After each GREEN
8. **Note technical debt** - What needs refactoring
9. **Measure coverage** - Ensure tests are passing
10. **Update metadata** - Record progress

Generate minimal implementation that **achieves GREEN state** without premature optimization.

## Output

Write `.kiro/specs/$ARGUMENTS/implementation.md` documenting the minimal code that makes all tests pass.