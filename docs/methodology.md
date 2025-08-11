# Kiro-SDD Methodology

## Overview

Kiro-SDD (Specification-Driven Development) is a systematic approach to software development that addresses the fundamental disconnect between AI-generated code and actual business requirements.

## The Problem

### The 70% Rewrite Phenomenon

In AI-assisted development, we consistently observe that approximately 70% of generated database queries need to be rewritten during frontend-backend integration. This occurs due to:

1. **Premature Convergence**: AI attempts to solve database, API, and UI concerns simultaneously
2. **Specification Drift**: Lack of clear boundaries between development phases
3. **Assumption Cascade**: Each assumption compounds, leading to systemic misalignment
4. **Missing Feedback Loops**: No validation between phases

### Real-World Impact

- **Time Loss**: 3-4x longer development cycles
- **Quality Issues**: Inconsistent implementations
- **Technical Debt**: Accumulated from hasty fixes
- **Team Frustration**: Constant rework demoralizes developers

## The Solution

### Core Principles

#### 1. Separation of Concerns

Each phase focuses on a single aspect:
- **Requirements**: WHAT needs to be built
- **Use Cases**: WHO uses it and HOW
- **Sequences**: WHEN things happen
- **Schema**: WHERE data lives
- **API**: HOW systems communicate
- **Interfaces**: WHAT data looks like
- **Tests**: HOW we verify
- **Design**: HOW it's architected
- **Tasks**: WHAT gets implemented

#### 2. Approval Gates

Human review between each phase ensures:
- Alignment with business goals
- Technical feasibility
- Consistency across specifications

#### 3. Specification References

Every implementation task must reference:
- Source specification section
- Related specifications
- Test specifications

#### 4. Incremental Validation

Continuous validation through:
- Status checks
- Reference validation
- Coverage analysis

## Specification Phases

### Phase 1: Requirements Definition

**Focus**: Business needs and user stories

**Outputs**:
- User stories in EARS format
- Functional requirements
- Non-functional requirements
- Acceptance criteria

**Key Questions**:
- What problem are we solving?
- Who are the users?
- What are the success metrics?

### Phase 2: Use Case Analysis

**Focus**: User interactions and data flow

**Outputs**:
- Actor definitions
- Use case descriptions
- Data elements
- Success/failure scenarios

**Key Questions**:
- How do users interact with the system?
- What data is required?
- What are the edge cases?

### Phase 3: Sequence Design

**Focus**: System interactions and timing

**Outputs**:
- Sequence diagrams
- State transitions
- Error flows
- Timing constraints

**Key Questions**:
- In what order do things happen?
- How do components interact?
- What happens when things fail?

### Phase 4: Schema Design

**Focus**: Data structure and relationships

**Outputs**:
- Table definitions
- Relationships
- Indexes
- Constraints

**Key Questions**:
- How is data organized?
- What are the relationships?
- How do we ensure integrity?

### Phase 5: API Specification

**Focus**: Service contracts and interfaces

**Outputs**:
- Endpoint definitions
- Request/response formats
- Error codes
- Rate limits

**Key Questions**:
- How do services communicate?
- What are the contracts?
- How do we handle errors?

### Phase 6: Interface Definition

**Focus**: Type safety and contracts

**Outputs**:
- Type definitions
- Validation schemas
- Data transformations
- Contract tests

**Key Questions**:
- What are the data shapes?
- How do we ensure type safety?
- What validations are needed?

### Phase 7: Test Specification

**Focus**: Verification and validation

**Outputs**:
- Unit test scenarios
- Integration test plans
- E2E test cases
- Performance criteria

**Key Questions**:
- How do we verify correctness?
- What are the test boundaries?
- How do we measure success?

### Phase 8: Technical Design

**Focus**: Architecture and implementation

**Outputs**:
- Component architecture
- Design patterns
- Security measures
- Performance strategies

**Key Questions**:
- How is it built?
- What patterns apply?
- How do we ensure quality?

### Phase 9: Task Breakdown

**Focus**: Implementation planning

**Outputs**:
- Task list with references
- Dependencies
- Acceptance criteria
- Time estimates

**Key Questions**:
- What needs to be done?
- In what order?
- How do we verify completion?

## Implementation Strategy

### The Reference System

Every task must include:

```markdown
- [ ] Implement user login endpoint
  - Reference: api.md#post-auth-login
  - Reference: interfaces.md#LoginRequest
  - Reference: tests-red.md#login-tests
  - Acceptance: All tests pass, response < 200ms
```

### Benefits of References

1. **Traceability**: Every line of code traces to requirements
2. **Consistency**: Implementation matches specification
3. **Validation**: Easy to verify completeness
4. **Maintenance**: Changes propagate clearly

## Approval Workflow

### Interactive Approvals

Commands prompt for approval:
```
Have you reviewed requirements.md? [y/N]
```

Answering 'y':
- Updates approval status
- Allows progression to next phase
- Logs approval timestamp

### Manual Approvals

Edit `spec.json` directly:
```json
"approvals": {
  "requirements": {
    "generated": true,
    "approved": true,
    "approved_at": "2025-01-15T10:00:00Z",
    "approved_by": "manual"
  }
}
```

## Validation and Verification

### Status Checking

```bash
/kiro:spec-status feature-name
```

Provides:
- Phase completion status
- Approval status
- Next steps
- Blockers

### Reference Validation

```bash
/kiro:spec-validate feature-name
```

Ensures:
- All tasks have references
- References point to valid sections
- No orphaned specifications
- Complete requirement coverage

## Common Patterns

### Pattern 1: API-First Development

1. Start with API specification
2. Derive interfaces from API
3. Design schema to support API
4. Create tests for contracts
5. Implement with confidence

### Pattern 2: Data-First Development

1. Begin with schema design
2. Build API around data model
3. Create interfaces from schema
4. Test data integrity
5. Implement with type safety

### Pattern 3: Test-First Development

1. Write test specifications first
2. Derive interfaces from tests
3. Design API to pass tests
4. Schema supports test cases
5. Implementation guided by tests

## Anti-Patterns to Avoid

### Anti-Pattern 1: Skipping Phases

**Problem**: Jumping from requirements to implementation
**Result**: Misaligned implementation
**Solution**: Follow all phases, even if briefly

### Anti-Pattern 2: Retroactive Specifications

**Problem**: Writing specs after implementation
**Result**: Specifications don't match reality
**Solution**: Always specify before implementing

### Anti-Pattern 3: Ignored Approvals

**Problem**: Auto-approving without review
**Result**: Cascading errors
**Solution**: Genuine review at each gate

### Anti-Pattern 4: Missing References

**Problem**: Tasks without specification links
**Result**: Implementation drift
**Solution**: Mandatory reference validation

## Metrics and Success

### Key Metrics

1. **Specification Coverage**: % of code with spec references
2. **First-Time Success**: % of implementations passing tests initially
3. **Rework Rate**: % of code requiring changes post-implementation
4. **Approval Time**: Average time per phase approval
5. **Defect Origin**: % of bugs traceable to specification gaps

### Success Indicators

- **< 10% Rework Rate**: Specifications align with implementation
- **> 90% Coverage**: Comprehensive specification references
- **< 1 Day Approval**: Efficient review process
- **> 80% First-Time Success**: Clear specifications

## Integration with AI Tools

### AI as Specification Generator

AI excels at:
- Generating initial specifications
- Identifying edge cases
- Creating comprehensive tests
- Suggesting implementation patterns

### Human as Specification Validator

Humans excel at:
- Business alignment
- Feasibility assessment
- Priority decisions
- Quality gates

### Optimal Collaboration

1. AI generates specification draft
2. Human reviews and adjusts
3. AI implements from specification
4. Human validates against requirements
5. AI refines based on feedback

## Case Studies

### Case Study 1: E-Commerce Platform

**Without Kiro-SDD**:
- 3 months development
- 2 months fixing integration issues
- 40% of code rewritten

**With Kiro-SDD**:
- 1 month specifications
- 2 months development
- < 5% rework
- 40% faster overall

### Case Study 2: Healthcare API

**Without Kiro-SDD**:
- Security issues discovered late
- Major schema redesign required
- 6-month delay

**With Kiro-SDD**:
- Security considered in requirements
- Schema validated early
- On-time delivery

## Conclusion

Kiro-SDD transforms chaotic AI-assisted development into a predictable, efficient process. By enforcing specification-first development with approval gates and reference requirements, we eliminate the 70% rewrite problem and deliver higher quality software faster.

The methodology's strength lies not in its complexity, but in its systematic approach to managing complexity. Each phase builds on the previous, creating a solid foundation for implementation that aligns with business needs and technical constraints.

Remember: **Specification is not documentation—it's the blueprint for success.**