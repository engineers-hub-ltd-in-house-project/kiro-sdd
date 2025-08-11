---
description: Refactor code while maintaining GREEN state
allowed-tools: Bash, Read, Write, Edit, MultiEdit, Update
---

# Refactoring Phase

Refactor implementation while keeping all tests GREEN for feature: **$ARGUMENTS**

## Context Validation

### Previous Phases Check

- Implementation: @.kiro/specs/$ARGUMENTS/implementation.md
- Test results: All tests must be GREEN
- Technical debt list: From implementation phase
- Spec metadata: @.kiro/specs/$ARGUMENTS/spec.json

**CRITICAL**: Refactoring can only begin when all tests are passing (GREEN state).

## Task: Refactor While Maintaining GREEN

Generate refactor.md documenting code improvements:

### 1. Refactoring Document Structure

```markdown
# Refactoring Report: [Feature Name]

## Overview
Systematic improvement of code quality while maintaining all tests in GREEN state.
Every refactoring must be validated by running tests.

## Refactoring Strategy
1. Run all tests (confirm GREEN)
2. Make one refactoring change
3. Run all tests (must stay GREEN)
4. Commit the refactoring
5. Repeat

## Code Smells Identified

### Duplication
- Location 1: [file:line] - Duplicate logic for X
- Location 2: [file:line] - Duplicate logic for Y
- Solution: Extract to shared function/component

### Long Methods
- Method: [name] - 50+ lines
- Solution: Break into smaller methods

### Large Classes
- Class: [name] - Too many responsibilities
- Solution: Split into multiple classes (SRP)

### Poor Naming
- Variable: [old_name] → [new_name]
- Method: [old_name] → [new_name]
- Class: [old_name] → [new_name]

### Magic Numbers
- Value: [number] appears in X places
- Solution: Extract to named constant

### Complex Conditionals
- Location: [file:line]
- Solution: Extract to well-named method

## Refactoring Catalog

### Refactoring 1: Extract Method
**Before:**
[Code showing long method]

**After:**
[Code showing extracted methods]

**Tests Status:** ✅ All GREEN

### Refactoring 2: Remove Duplication
**Before:**
[Duplicated code in multiple places]

**After:**
[Shared utility/service]

**Tests Status:** ✅ All GREEN

### Refactoring 3: Introduce Parameter Object
**Before:**
[Method with many parameters]

**After:**
[Method with parameter object]

**Tests Status:** ✅ All GREEN

### Refactoring 4: Replace Magic Numbers
**Before:**
[Hard-coded values]

**After:**
[Named constants]

**Tests Status:** ✅ All GREEN

### Refactoring 5: Simplify Conditionals
**Before:**
[Complex nested if/else]

**After:**
[Guard clauses or strategy pattern]

**Tests Status:** ✅ All GREEN

## Design Pattern Applications

### Pattern: Repository
- Applied to: Data access layer
- Benefits: Separation of concerns, testability

### Pattern: Factory
- Applied to: Object creation
- Benefits: Flexibility, reduced coupling

### Pattern: Strategy
- Applied to: Algorithm variations
- Benefits: Open/closed principle

### Pattern: Observer
- Applied to: Event handling
- Benefits: Loose coupling

## Performance Optimizations

### Database Queries
- Before: N+1 queries
- After: Eager loading
- Improvement: 90% reduction in queries

### Caching
- Added: Redis caching layer
- Cache keys: [list of keys]
- TTL strategy: [expiration rules]

### Algorithm Improvements
- Before: O(n²) nested loops
- After: O(n log n) optimized algorithm
- Improvement: 10x faster for large datasets

## Code Organization

### Module Structure
Before:
```
- big_file.ext (1000 lines)
```

After:
```
- module/
  - core.ext (200 lines)
  - helpers.ext (150 lines)
  - types.ext (100 lines)
  - validators.ext (150 lines)
```

### Dependency Management
- Extracted interfaces
- Dependency injection
- Reduced coupling

## Error Handling Improvements

### Consistent Error Types
- Created error hierarchy
- Standardized error messages
- Added error codes

### Validation Enhancement
- Centralized validation rules
- Better error messages
- Input sanitization

## Documentation Added

### Code Comments
- Added JSDoc/docstrings
- Explained complex algorithms
- Documented business rules

### API Documentation
- OpenAPI annotations
- Example requests/responses
- Error scenarios

## Testing Improvements

### Test Organization
- Grouped related tests
- Better test names
- Shared test utilities

### Test Performance
- Parallelized test execution
- Optimized test data setup
- Reduced test runtime by X%

## Metrics Comparison

### Before Refactoring
- Code coverage: X%
- Cyclomatic complexity: Y
- Duplicate code: Z%
- Technical debt: N hours

### After Refactoring
- Code coverage: X+10%
- Cyclomatic complexity: Y-30%
- Duplicate code: <5%
- Technical debt: N-50% hours

## Remaining Technical Debt

Items not addressed:
1. [Issue] - Reason: [why deferred]
2. [Issue] - Reason: [why deferred]

## Next Steps

Recommendations for future improvements:
1. Performance monitoring
2. Additional caching
3. Database indexing
4. API versioning
```

### 2. Refactoring Checklist

For each refactoring:
- [ ] Tests are GREEN before change
- [ ] Make single refactoring
- [ ] Tests remain GREEN after change
- [ ] Code is cleaner/simpler
- [ ] No functionality changed
- [ ] Commit with clear message

### 3. Common Refactoring Patterns

#### Extract Method
When method is too long or does multiple things

#### Extract Variable
When expression is complex or repeated

#### Inline Variable
When variable doesn't add clarity

#### Extract Class
When class has too many responsibilities

#### Move Method
When method uses another class more

#### Replace Conditional with Polymorphism
When switch/if-else is based on type

### 4. Quality Metrics to Track

Monitor improvements in:
- Lines of code (reduction)
- Cyclomatic complexity (reduction)
- Code duplication (reduction)
- Test coverage (increase)
- Performance metrics (improvement)

### 5. Refactoring Safety Rules

Never:
- Change functionality
- Break any test
- Refactor without tests
- Make multiple changes at once
- Skip test runs

Always:
- Keep tests GREEN
- Make small changes
- Commit frequently
- Document why
- Measure improvement

### 6. Update Metadata

Update spec.json:
```json
{
  "phase": "refactored",
  "quality_metrics": {
    "coverage": "[percentage]",
    "complexity": "[average]",
    "duplication": "[percentage]"
  },
  "refactorings_applied": [count],
  "tests_status": "all_green",
  "approvals": {
    "refactoring": {
      "completed": true,
      "approved": false
    }
  },
  "updated_at": "current_timestamp"
}
```

## Instructions

1. **Confirm GREEN state** - All tests passing
2. **Identify code smells** - What needs improving
3. **Prioritize refactorings** - Biggest impact first
4. **Apply one at a time** - Small, safe changes
5. **Run tests after each** - Maintain GREEN
6. **Extract common code** - Remove duplication
7. **Improve naming** - Clear, descriptive names
8. **Simplify logic** - Reduce complexity
9. **Document improvements** - What and why
10. **Update metadata** - Record metrics

Generate refactoring documentation that **improves code quality** while maintaining all functionality.

## Output

Write `.kiro/specs/$ARGUMENTS/refactor.md` documenting all refactoring activities and improvements.