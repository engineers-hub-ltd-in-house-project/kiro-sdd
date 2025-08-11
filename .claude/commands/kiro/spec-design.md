---
description: Create integrated technical design document from all detailed specifications
allowed-tools: Bash, Read, Write, Edit, MultiEdit, Update
---

# Integrated Technical Design

Create comprehensive technical design document for feature: **$ARGUMENTS**

## Context Validation

### All Detailed Specifications Check

**CRITICAL**: This phase integrates all previous detailed specifications into a cohesive design document.

- Requirements: @.kiro/specs/$ARGUMENTS/requirements.md
- Use cases: @.kiro/specs/$ARGUMENTS/usecase.md
- Sequence diagrams: @.kiro/specs/$ARGUMENTS/sequence.md
- Database schema: @.kiro/specs/$ARGUMENTS/schema.md
- API specification: @.kiro/specs/$ARGUMENTS/api.md
- Interface definitions: @.kiro/specs/$ARGUMENTS/interfaces.md
- Spec metadata: @.kiro/specs/$ARGUMENTS/spec.json

## Interactive Approval

Before generating design document, ask the user:
```
Ready to generate integrated design for $ARGUMENTS?
All individual specifications should be reviewed first.
Have you reviewed all specifications (requirements, usecase, sequence, schema, api, interfaces)? [y/N]: 
```

If 'N': Stop and request review of specifications first.
If 'y': Update spec.json to mark all specifications as approved and proceed:
```json
{
  "approvals": {
    "requirements": { "generated": true, "approved": true },
    "usecase": { "generated": true, "approved": true },
    "sequence": { "generated": true, "approved": true },
    "schema": { "generated": true, "approved": true },
    "api": { "generated": true, "approved": true },
    "interfaces": { "generated": true, "approved": true }
  }
}
```

## Task: Integrate All Specifications

Generate design.md as the master design document:

### 1. Design Document Structure

```markdown
# Technical Design: [Feature Name]

## Executive Summary
[Brief overview of the solution, key decisions, and architecture]

## Requirements Traceability

### Requirements Coverage
[Table showing how each requirement from requirements.md is addressed]

| Requirement ID | Requirement | Design Component | Specification Reference |
|---------------|-------------|------------------|------------------------|
| REQ-1 | [Requirement] | [Component] | usecase.md#UC-1 |
| REQ-2 | [Requirement] | [Component] | api.md#endpoint-1 |

## System Architecture

### High-Level Architecture
[Consolidated view from all specifications]
- Frontend components (from sequence.md)
- Backend services (from api.md)
- Data layer (from schema.md)
- External integrations (from sequence.md)

### Component Interactions
[Mermaid diagram showing how all components connect]

## Data Architecture

### Data Model
[Summary from schema.md with key entities and relationships]

### Data Flow
[How data moves through the system from sequence.md]

## API Design

### API Overview
[Summary of all endpoints from api.md]

### Key API Decisions
- Authentication strategy
- Versioning approach
- Error handling patterns
- Rate limiting

## Interface Contracts

### Type System
[Summary from interfaces.md]
- Shared types
- Validation rules
- Serialization formats

## Implementation Strategy

### Technology Stack
[Consolidated technology choices and rationale]

### Development Phases
1. Phase 1: [What gets built first]
2. Phase 2: [What gets built next]
3. Phase 3: [Final components]

### Risk Mitigation
[Identified risks and mitigation strategies]

## Testing Strategy

### Test Coverage Plan
[From tests-red.md specification]
- Unit test approach
- Integration test approach
- E2E test approach

## Performance & Scalability

### Performance Targets
[Consolidated from all specifications]

### Scalability Approach
[How system will scale]

## Security Considerations

### Security Requirements
[Consolidated security needs]

### Security Implementation
[How security is addressed]

## Deployment Architecture

### Deployment Strategy
[How the system will be deployed]

### Infrastructure Requirements
[What infrastructure is needed]

## Migration Plan

### Data Migration
[If applicable, how existing data will be migrated]

### Rollback Strategy
[How to rollback if needed]

## Documentation Plan

### Technical Documentation
- API documentation
- Database documentation
- Code documentation

### User Documentation
- User guides
- Admin guides

## Appendices

### A. Detailed Specifications
Links to all detailed specification documents:
- requirements.md
- usecase.md
- sequence.md
- schema.md
- api.md
- interfaces.md
- tests-red.md

### B. Decision Log
Key technical decisions made during design:
| Decision | Rationale | Alternative Considered |
|----------|-----------|----------------------|
| [Decision] | [Why] | [What else was considered] |

### C. Glossary
[Technical terms and their definitions]
```

### 2. Integration Checklist

Ensure the design document includes:
- [ ] All requirements are mapped to design components
- [ ] All use cases are covered
- [ ] All sequence diagrams are represented
- [ ] Database schema is summarized
- [ ] API endpoints are listed
- [ ] Interface contracts are documented
- [ ] Test strategy is defined
- [ ] Performance requirements are specified
- [ ] Security is addressed
- [ ] Deployment is planned

### 3. Cross-Reference Validation

Verify consistency across all specifications:
- Data types match between schema.md and interfaces.md
- API definitions match sequence diagrams
- Use cases align with API endpoints
- Test cases cover all requirements
- No conflicting design decisions

### 4. Design Decisions Documentation

Document key decisions:
```markdown
## Design Decision Record

### Decision 1: [Title]
**Context**: [Why this decision was needed]
**Decision**: [What was decided]
**Rationale**: [Why this approach]
**Consequences**: [Impact of this decision]
**Alternatives**: [What else was considered]

### Decision 2: [Title]
[Continue for all major decisions]
```

### 5. Risk Assessment

Identify and document risks:
```markdown
## Risk Assessment

| Risk | Probability | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk description] | High/Medium/Low | High/Medium/Low | [Mitigation strategy] |
```

### 6. Dependencies Mapping

Show all system dependencies:
```markdown
## System Dependencies

### Internal Dependencies
- Component A depends on Component B
- Service X requires Database Y

### External Dependencies
- Third-party service integrations
- External APIs
- Libraries and frameworks

### Data Dependencies
- Data sources
- Data transformations
- Data validations
```

### 7. Update Metadata

Update spec.json:
```json
{
  "phase": "design-integrated",
  "specifications_integrated": [
    "requirements",
    "usecase",
    "sequence",
    "schema",
    "api",
    "interfaces"
  ],
  "design_decisions_count": [number],
  "risks_identified": [number],
  "approvals": {
    "design": {
      "generated": true,
      "integrated": true,
      "approved": false
    }
  },
  "updated_at": "current_timestamp"
}
```

## Instructions

1. **Review all specifications** - Read every detailed spec
2. **Extract key elements** - Pull essential parts from each
3. **Create coherent narrative** - Integrate into single document
4. **Validate consistency** - Ensure no conflicts
5. **Map requirements** - Show traceability
6. **Document decisions** - Explain key choices
7. **Identify risks** - Document mitigation
8. **Summarize for stakeholders** - Executive summary
9. **Cross-reference everything** - Link to source specs
10. **Update metadata** - Record integration

Generate an integrated design document that serves as the **single source of truth** for the entire technical solution.

## Output

Write `.kiro/specs/$ARGUMENTS/design.md` with complete integrated technical design.