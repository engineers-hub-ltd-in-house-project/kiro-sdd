# Kiro-SDD: Specification-Driven Development Framework

A comprehensive specification-driven development framework that prevents integration failures between frontend and backend in AI-assisted development.
Based on [claude-code-spec](https://github.com/gotalab/claude-code-spec) with enhanced contract enforcement.

## Problem Statement

In AI-assisted development with frontend-backend separation, critical integration failures occur:
- Frontend sends parameters not defined in API specifications
- Backend expects data structures that frontend never implements
- Database schemas don't match API contracts
- Type definitions diverge between layers
- AI generates incompatible code for each layer independently

## Solution

Kiro-SDD enforces strict contract-driven development through sequential specification phases:
1. **Schema First**: Define database structure
2. **API Specification**: Define exact contracts based on schema (REST/gRPC/GraphQL)
3. **Interface Definitions**: Create shared type definitions
4. **Test Specifications**: Define validation before implementation
5. **Implementation with References**: Every task must reference specifications

<!-- =============================================== -->
<!-- PROJECT-SPECIFIC CONFIGURATION (Placeholder)    -->
<!-- =============================================== -->
<!-- 
## Project-Specific Settings

Add your project-specific configurations here when not using Kiro-SDD:
- Custom development guidelines
- Project conventions
- Team-specific rules
- Technology constraints
-->

<!-- =============================================== -->
<!-- KIRO-SDD FRAMEWORK SECTION                     -->
<!-- =============================================== -->

## Kiro-SDD Context

### Paths
- Steering: `.kiro/steering/`
- Specs: `.kiro/specs/`
- Commands: `.claude/commands/`

### Steering vs Specification

**Steering** (`.kiro/steering/`) - Guide AI with project-wide rules and context  
**Specs** (`.kiro/specs/`) - Formalize development process for individual features

### Active Specifications
- Check `.kiro/specs/` for active specifications
- Use `/kiro:spec-status [feature-name]` to check progress

## Development Guidelines
- Think in English, but generate responses in Japanese (思考は英語、回答の生成は日本語で行うように)

## Kiro-SDD Workflow

### Phase 0: Steering (Optional)
`/kiro:steering` - Create/update steering documents
`/kiro:steering-custom` - Create custom steering for specialized contexts

**Note**: Optional for new features or small additions. Can proceed directly to spec-init.

### Phase 1: Specification Initialization
`/kiro:spec-init [detailed description]` - Initialize spec structure with:
- Feature directory creation
- Template files (README, requirements, design, tasks)
- Metadata tracking (spec.json)
- Project description storage

### Phase 2: Contract-First Specification Generation
The workflow enforces contracts between all system layers:

1. `/kiro:spec-requirements [feature]` - Business requirements (EARS format)
2. `/kiro:spec-usecase [feature]` - Use cases and data elements
3. `/kiro:spec-sequence [feature]` - Sequence diagrams showing component interactions
4. `/kiro:spec-schema [feature]` - **Database contracts** (SQL DDL, migrations)
5. `/kiro:spec-api [feature] [--type rest|grpc|graphql]` - **API contracts** (OpenAPI/Protocol Buffers/GraphQL)
6. `/kiro:spec-interfaces [feature]` - **Type contracts** (shared definitions)
7. `/kiro:spec-tests-red [feature]` - **Test contracts** (validation specs)

**Critical**: Each specification builds on previous ones, ensuring consistency

### Phase 3: Design Integration
`/kiro:spec-design [feature]` - Create integrated technical design
- Prompts: "Have you reviewed all specifications? [y/N]"
- Integrates all detailed specs into master design document
- Provides traceability matrix and decision records

### Phase 4: Task Generation
`/kiro:spec-tasks [feature]` - Generate implementation tasks
- Prompts: "Have you reviewed all specifications including tests? [y/N]"
- Every task includes mandatory specification references
- No AI self-judgment - follows specs exactly

### Phase 5: Implementation & Tracking
- Begin implementation following generated tasks
- Use `/kiro:spec-status [feature]` to track progress
- Update task completion status as work progresses

## Development Rules
1. **Contract Enforcement**: Every layer must respect defined contracts
2. **Schema First**: Database defines the source of truth for data structures
3. **No AI Self-Judgment**: Implementation must reference specifications exactly
4. **Approval Gates**: Each phase requires human review before proceeding
5. **Reference Mandatory**: Every task must link to relevant specifications
6. **Type Safety**: Enforce consistent types across all layers
7. **Validation First**: Tests defined before implementation
8. **Progress Tracking**: Use `/kiro:spec-status` to monitor compliance

## Why This Prevents Integration Failures

Without Kiro-SDD, AI independently generates:
- Frontend: sends `userId`
- Backend: expects `user_id`
- Database: has column `user_identifier`

With Kiro-SDD:
- Schema defines: `user_id UUID`
- API specifies: `user_id: string (UUID format)` in REST/gRPC/GraphQL
- Interface defines: `userId: string // maps to user_id`
- Implementation references these specifications

## Approval Workflow
The modern workflow uses interactive approval prompts:
- Eliminates manual spec.json editing
- Enforces review checkpoints
- Allows immediate progression after confirmation
- Maintains safety with option to stop for review

## Getting Started with Kiro-SDD
1. (Optional) Initialize steering: `/kiro:steering`
2. Create specification: `/kiro:spec-init [detailed-feature-description]`
3. Generate requirements: `/kiro:spec-requirements [feature-name]`
4. Continue through all specification phases
5. Review integrated design
6. Generate and follow implementation tasks