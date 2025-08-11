---
description: Create test specifications in Red state following TDD principles
allowed-tools: Bash, Read, Write, Edit, MultiEdit, Update
---

# Test Specification (Red State)

Create comprehensive test specifications for feature: **$ARGUMENTS**

## Language Setting
**IMPORTANT**: Generate all content in English, not Japanese.

## Context Validation

### Previous Phases Check

- Use cases: @.kiro/specs/$ARGUMENTS/usecase.md
- Sequence diagrams: @.kiro/specs/$ARGUMENTS/sequence.md
- Database schema: @.kiro/specs/$ARGUMENTS/schema.md
- API specification: @.kiro/specs/$ARGUMENTS/api.md
- Interface definitions: @.kiro/specs/$ARGUMENTS/interfaces.md
- Spec metadata: @.kiro/specs/$ARGUMENTS/spec.json

**CRITICAL**: Test specifications require all previous phases to be approved.

## Interactive Approval

Before generating test specs, ask the user:
```
Ready to generate test specifications for $ARGUMENTS?
Interface definitions should be reviewed first.
Have you reviewed interfaces.md? [y/N]: 
```

If 'N': Stop and request review of interfaces first.
If 'y': Update spec.json to mark interfaces as approved and proceed:
```json
{
  "approvals": {
    "interfaces": {
      "generated": true,
      "approved": true
    }
  }
}
```

## Task: Generate Test Specifications

Generate tests-red.md with failing test cases following TDD principles:

### 1. Test Document Structure

```markdown
# Test Specifications: [Feature Name]

## Overview
Comprehensive test suite following Test-Driven Development (TDD) methodology.
All tests should be in RED state (failing) before implementation.

## Test Strategy
- Test-first approach: Write tests before code
- Coverage targets: Unit (80%), Integration (100%), E2E (critical paths)
- Test pyramid: Many unit tests, some integration, few E2E
- Mock strategy: Mock external dependencies, not internal modules

## Unit Tests

### Backend Unit Tests

#### Test Suite: [Service/Module Name]
Purpose: Test business logic in isolation

Test Cases:
1. **Test: Should calculate metrics correctly**
   - Arrange: Set up test data
   - Act: Call calculation method
   - Assert: Verify correct output
   - Expected: RED (method doesn't exist yet)

2. **Test: Should handle empty data gracefully**
   - Arrange: Empty dataset
   - Act: Call method
   - Assert: Returns default values
   - Expected: RED

3. **Test: Should validate input parameters**
   - Arrange: Invalid parameters
   - Act: Call with bad data
   - Assert: Throws validation error
   - Expected: RED

#### Test Suite: [Another Module]
[Additional test cases...]

### Frontend Unit Tests

#### Component Tests
1. **Test: Should render with default props**
   - Arrange: Default prop values
   - Act: Render component
   - Assert: Elements present
   - Expected: RED (component not created)

2. **Test: Should handle user interactions**
   - Arrange: Rendered component
   - Act: Simulate click/input
   - Assert: Correct callback fired
   - Expected: RED

3. **Test: Should display error states**
   - Arrange: Error prop
   - Act: Render
   - Assert: Error message visible
   - Expected: RED

#### Service Tests
1. **Test: Should transform API response**
   - Arrange: Raw API response
   - Act: Transform method
   - Assert: Correct format
   - Expected: RED

## Integration Tests

### API Integration Tests

#### Test Suite: API Endpoints
1. **Test: GET /api/resource should return 200**
   - Arrange: Test database setup
   - Act: HTTP GET request
   - Assert: Status 200, correct schema
   - Expected: RED (endpoint not implemented)

2. **Test: POST /api/resource should create resource**
   - Arrange: Valid request body
   - Act: HTTP POST request
   - Assert: Status 201, resource created
   - Expected: RED

3. **Test: Should handle authentication**
   - Arrange: Invalid/missing token
   - Act: API request
   - Assert: Status 401
   - Expected: RED

4. **Test: Should validate request body**
   - Arrange: Invalid request
   - Act: POST with bad data
   - Assert: Status 400, error details
   - Expected: RED

### Database Integration Tests

1. **Test: Should perform transactions correctly**
   - Arrange: Database state
   - Act: Multi-step operation
   - Assert: All or nothing
   - Expected: RED

2. **Test: Should handle concurrent updates**
   - Arrange: Parallel operations
   - Act: Simultaneous updates
   - Assert: Data consistency
   - Expected: RED

## End-to-End Tests

### Critical User Journeys

#### Journey 1: [Primary Use Case]
1. **Test: Complete happy path**
   - Steps:
     1. User logs in
     2. Navigates to feature
     3. Performs main action
     4. Sees success result
   - Assertions at each step
   - Expected: RED (feature not built)

2. **Test: Error recovery**
   - Steps:
     1. User action fails
     2. Error displayed
     3. User corrects
     4. Success on retry
   - Expected: RED

#### Journey 2: [Secondary Use Case]
[Additional E2E tests...]

## Performance Tests

1. **Test: API response time < 200ms**
   - Arrange: Normal load
   - Act: Measure response time
   - Assert: P95 < 200ms
   - Expected: RED

2. **Test: Handle 100 concurrent users**
   - Arrange: Load test setup
   - Act: Simulate users
   - Assert: No errors, acceptable response time
   - Expected: RED

## Security Tests

1. **Test: SQL injection prevention**
   - Arrange: Malicious input
   - Act: Submit to API
   - Assert: Input sanitized
   - Expected: RED

2. **Test: XSS prevention**
   - Arrange: Script in input
   - Act: Display in UI
   - Assert: Script not executed
   - Expected: RED

3. **Test: Authorization checks**
   - Arrange: User without permission
   - Act: Attempt restricted action
   - Assert: Access denied
   - Expected: RED

## Test Data Management

### Fixtures
- User fixtures: Standard test users
- Data fixtures: Sample datasets
- State fixtures: Predefined app states

### Mocks
- External service mocks
- API response mocks
- Database query mocks

### Test Database
- Separate test database
- Seed data scripts
- Reset between tests

## Test Execution Plan

### Phase 1: Unit Tests
1. Write all unit tests (RED)
2. Implement minimum code (GREEN)
3. No refactoring yet

### Phase 2: Integration Tests
1. Write integration tests (RED)
2. Connect components (GREEN)
3. Basic refactoring

### Phase 3: E2E Tests
1. Write E2E tests (RED)
2. Complete implementation (GREEN)
3. Full refactoring

## Acceptance Criteria Mapping

Map each test to requirements:
- UC-1.1 → Test: [test name]
- UC-1.2 → Test: [test name]
- API-1 → Test: [test name]
[Complete mapping...]

## Test Coverage Targets

| Type | Target | Rationale |
|------|--------|-----------|
| Unit | 80% | Core logic coverage |
| Integration | 100% | All APIs tested |
| E2E | Critical paths | User journeys |
| Performance | Key metrics | SLA compliance |
| Security | OWASP Top 10 | Security baseline |
```

### 2. Test Case Template

For each test, document:
- **Test name**: Descriptive name
- **Test type**: Unit/Integration/E2E
- **Dependencies**: What needs to exist
- **Initial state**: RED (failing)
- **Success criteria**: What makes it GREEN
- **Related requirement**: Links to specs

### 3. Mock Specifications

Define all mocks needed:
- External API responses
- Database queries
- User interactions
- Time/date functions
- Random generators

### 4. Test Data Requirements

Specify:
- Minimum dataset sizes
- Edge cases to cover
- Invalid data examples
- Boundary conditions
- Performance test data volumes

### 5. Testing Tools Selection

Recommend appropriate tools:
- Unit test framework
- Integration test framework
- E2E test framework
- Mocking libraries
- Assertion libraries
- Coverage tools

### 6. Update Metadata

Update spec.json:
```json
{
  "phase": "tests-red-defined",
  "test_cases_count": [number],
  "test_types": {
    "unit": [count],
    "integration": [count],
    "e2e": [count],
    "performance": [count],
    "security": [count]
  },
  "approvals": {
    "tests-red": {
      "generated": true,
      "approved": false
    }
  },
  "updated_at": "current_timestamp"
}
```

## Instructions

1. **Review all specifications** - Understand complete system
2. **Map requirements to tests** - Every requirement needs a test
3. **Start with unit tests** - Test smallest units first
4. **Add integration tests** - Test component interactions
5. **Include E2E tests** - Test complete workflows
6. **Add edge cases** - Invalid inputs, errors
7. **Include performance tests** - Response times, load
8. **Add security tests** - Common vulnerabilities
9. **Document test data** - Fixtures and mocks
10. **Update metadata** - Record test counts

Generate comprehensive test specifications that **ensure quality through TDD** by defining all tests before implementation.

## Output

Write `.kiro/specs/$ARGUMENTS/tests-red.md` with complete test specifications in RED state.