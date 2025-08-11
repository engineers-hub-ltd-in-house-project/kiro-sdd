---
description: Define type definitions and contracts for frontend/backend integration
allowed-tools: Bash, Read, Write, Edit, MultiEdit, Update
---

# Interface Definitions

Define type definitions and contracts for feature: **$ARGUMENTS**

## Language Setting
**IMPORTANT**: Generate all content in English, not Japanese.

## Context Validation

### Previous Phases Check

- Use cases: @.kiro/specs/$ARGUMENTS/usecase.md
- Sequence diagrams: @.kiro/specs/$ARGUMENTS/sequence.md
- Database schema: @.kiro/specs/$ARGUMENTS/schema.md
- API specification: @.kiro/specs/$ARGUMENTS/api.md
- Spec metadata: @.kiro/specs/$ARGUMENTS/spec.json

**CRITICAL**: Interface definitions require approved API specification.

## Interactive Approval

Before generating interfaces, ask the user:
```
Ready to generate interface definitions for $ARGUMENTS?
API specification should be reviewed first.
Have you reviewed api.md? [y/N]: 
```

If 'N': Stop and request review of API spec first.
If 'y': Update spec.json to mark API as approved and proceed:
```json
{
  "approvals": {
    "api": {
      "generated": true,
      "approved": true
    }
  }
}
```

## Task: Generate Interface Definitions

Generate interfaces.md with shared type definitions and contracts:

### 1. Interface Document Structure

Create a document that defines:

#### Shared Type System
- Common type definitions used across all layers
- Naming conventions for consistency
- Null/undefined handling strategy
- Serialization/deserialization rules

#### Data Transfer Objects (DTOs)
- Request objects for each API endpoint
- Response objects with complete structure
- Error response formats
- Pagination and metadata structures

#### Validation Contracts
- Field-level validation rules
- Business logic constraints
- Format specifications (dates, emails, etc.)
- Required vs optional fields

#### Type Mapping
- Database types → API types → Frontend types
- Ensure no data loss in conversions
- Handle precision and range differences
- Document any transformations needed

### 2. Critical Elements to Define

#### Naming Conventions
- Establish consistent naming across all layers
- Define case conventions (camelCase, snake_case, etc.)
- Document any naming transformations between layers

#### Type Precision
- Number types: integer ranges, decimal precision
- String types: max lengths, character sets
- Date/time: timezone handling, format specification
- Binary data: encoding methods

#### Null Safety
- Define when null is allowed vs required
- Distinguish between null, undefined, and empty
- Document default values
- Handle missing fields in responses

#### Collections
- Array vs Set semantics
- Ordering guarantees
- Maximum sizes
- Empty collection handling

### 3. Contract Documentation Format

```markdown
## Interface: [Name]

### Purpose
[What this interface represents]

### Used By
- Frontend: [Where/how it's used]
- Backend: [Where/how it's used]
- Database: [Related tables/fields]

### Field Definitions
| Field Name | Type | Required | Validation | Description |
|------------|------|----------|------------|-------------|
| field_1 | type | Yes/No | rules | purpose |
| field_2 | type | Yes/No | rules | purpose |

### Transformation Rules
- From DB: [Any transformations when reading from database]
- To API: [Any transformations for API layer]
- To Frontend: [Any transformations for frontend]

### Examples
- Valid example 1
- Valid example 2
- Invalid example with explanation
```

### 4. Type Safety Checklist

For each interface, verify:
- [ ] All fields from database schema are mapped
- [ ] All API parameters have corresponding types
- [ ] Frontend can handle all response variations
- [ ] Error cases have defined types
- [ ] Pagination is consistently typed
- [ ] Metadata fields are included
- [ ] Validation rules match across layers
- [ ] Optional fields are clearly marked
- [ ] Enums have all possible values
- [ ] Dates have clear format specification

### 5. Common Patterns to Include

#### Standard Response Wrapper
Define a consistent structure for all API responses

#### Error Response Format
Standardize how errors are communicated

#### Pagination Structure
Consistent pagination across all list endpoints

#### Filtering and Sorting
Standard parameter formats for queries

#### Batch Operations
How to handle multiple items in requests/responses

### 6. Validation Rules Documentation

For each field type, document:
- Allowed values or ranges
- Format requirements
- Length constraints
- Character restrictions
- Business rule validations
- Cross-field validations

### 7. Serialization Specifications

Define clearly:
- JSON field naming (camelCase vs snake_case)
- Date/time format (ISO 8601, Unix timestamp, etc.)
- Number precision handling
- Boolean representations
- Null vs missing field behavior
- Array/object nesting limits

### 8. Version Compatibility

Document:
- How to handle field additions
- Deprecation strategy
- Backward compatibility rules
- Optional field introduction
- Type migration plans

### 9. Testing Contracts

Include:
- Sample valid payloads
- Sample invalid payloads
- Edge cases to test
- Type boundary tests
- Null/empty handling tests

### 10. Update Metadata

Update spec.json:
```json
{
  "phase": "interfaces-defined",
  "interfaces_count": [number],
  "validation_rules_defined": true,
  "approvals": {
    "usecase": { "approved": true },
    "sequence": { "approved": true },
    "schema": { "approved": true },
    "api": { "approved": true },
    "interfaces": {
      "generated": true,
      "approved": false
    }
  },
  "updated_at": "current_timestamp"
}
```

## Instructions

1. **Review API specification** - Extract all data types
2. **Review database schema** - Map to API types
3. **Define request types** - For each endpoint
4. **Define response types** - Including errors
5. **Document transformations** - Between layers
6. **Specify validation rules** - For each field
7. **Add serialization rules** - JSON formatting
8. **Include examples** - Valid and invalid
9. **Create type mapping table** - DB → API → Frontend
10. **Update tracking metadata** - Record completion

Generate comprehensive interface definitions that serve as the **single source of truth** for data contracts across all system layers.

## Output

Write `.kiro/specs/$ARGUMENTS/interfaces.md` with complete type definitions and contracts.