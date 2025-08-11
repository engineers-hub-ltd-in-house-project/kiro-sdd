# Kiro-SDD: Specification-Driven Development Framework

A comprehensive specification-driven development framework that prevents integration failures between frontend and backend in AI-assisted development.

## Problem Statement

In AI-assisted development with frontend-backend separation architecture, critical integration failures occur:
- Frontend sends parameters not defined in API specifications
- Backend expects data structures that frontend never implements
- Database schemas don't match API contracts
- Type definitions diverge between layers
- AI generates incompatible code for each layer independently

These issues stem from AI making isolated decisions without enforcing contracts between system layers.

## Solution

Kiro-SDD enforces strict contract-driven development through sequential specification phases:

1. **Schema First**: Define database structure
2. **API Specification**: Define exact contracts based on schema (REST/gRPC/GraphQL)
3. **Interface Definitions**: Create shared type definitions
4. **Test Specifications**: Define validation before implementation
5. **Implementation with References**: Every task must reference specifications

This prevents AI from creating incompatible code by requiring specification references for every implementation decision.

## Quick Start

### Installation

1. Copy the `.claude/commands/kiro/` directory to your project's `.claude/commands/` directory
2. Copy `.kiro/steering/` templates to your project (optional but recommended)
3. Initialize steering documents:
```bash
/kiro:steering
```
4. Initialize your first specification:
```bash
/kiro:spec-init "Your feature description here"
```

### Complete Workflow

```mermaid
graph LR
    S[Steering] -->|Optional| R[Requirements]
    R -->|Approve| U[Use Cases]
    U -->|Approve| Seq[Sequence]
    Seq -->|Approve| Sch[Schema]
    Sch -->|Approve| A[API]
    A -->|Approve| I[Interfaces]
    I -->|Approve| T[Tests]
    T -->|Approve| D[Design]
    D -->|Approve| Tasks[Tasks]
    Tasks -->|Ready| Impl[Implementation]
```

## Project Structure

```
your-project/
├── .claude/
│   └── commands/
│       └── kiro/           # Kiro commands
├── .kiro/
│   ├── steering/           # Project context
│   │   ├── product.md     # Product overview
│   │   ├── tech.md        # Technical stack
│   │   └── structure.md   # Code organization
│   └── specs/
│       └── your-feature/
│           ├── README.md           # Feature overview
│           ├── spec.json          # Metadata & approvals
│           ├── requirements.md    # Business requirements
│           ├── usecase.md        # Use cases & data
│           ├── sequence.md       # Sequence diagrams
│           ├── schema.md         # Database design
│           ├── api.md           # API specification (REST/gRPC/GraphQL)
│           ├── interfaces.md    # Type definitions
│           ├── tests-red.md     # Test specifications
│           ├── design.md        # Technical design
│           └── tasks.md         # Implementation tasks
└── CLAUDE.md              # Project instructions
```

## Available Commands

### Steering Commands (Optional but Recommended)
- `/kiro:steering` - Create/update core steering documents
- `/kiro:steering-custom` - Create custom steering for specialized contexts

### Core Specification Commands
- `/kiro:spec-init [description]` - Initialize new specification
- `/kiro:spec-requirements [feature]` - Generate requirements
- `/kiro:spec-usecase [feature]` - Define use cases
- `/kiro:spec-sequence [feature]` - Create sequence diagrams
- `/kiro:spec-schema [feature]` - Design database schema
- `/kiro:spec-api [feature] [--type rest|grpc|graphql]` - Define API contracts (REST/gRPC/GraphQL)
- `/kiro:spec-interfaces [feature]` - Create type definitions
- `/kiro:spec-tests-red [feature]` - Write test specifications
- `/kiro:spec-design [feature]` - Create technical design
- `/kiro:spec-tasks [feature]` - Generate implementation tasks

### Utility Commands
- `/kiro:spec-status [feature]` - Check specification progress
- `/kiro:spec-validate [feature]` - Validate task references

### TDD Commands
- `/kiro:impl-green [feature]` - Implement to pass tests
- `/kiro:refactor [feature]` - Refactor while keeping tests green

## Key Features

### 1. Contract Enforcement
Every layer must respect defined contracts:
- Database schema defines data structure
- API specification defines exact parameters and responses
- Interface definitions ensure type safety across layers
- No implementation without specification reference

### 2. Phased Development
Each phase must be approved before proceeding:
- Requirements → Use Cases → Sequences → Schema → API → Interfaces → Tests → Design → Tasks

### 3. Interactive Approvals
```bash
# When running /kiro:spec-design
> "Have you reviewed requirements.md? [y/N]"
# Answering 'y' automatically updates approval status
```

### 4. Specification References
Every implementation task MUST reference relevant specifications:
```markdown
- [ ] Implement user endpoint
  - Reference: api.md#post-users
  - Reference: interfaces.md#UserRequest
  - Reference: schema.md#users-table
```

### 5. Automatic Validation
The `/kiro:spec-validate` command ensures:
- All tasks have specification references
- No orphaned specifications
- Complete coverage of requirements
- Contract consistency between layers

## Why This Matters

Without Kiro-SDD, AI independently generates:
- Frontend code that sends `userId` 
- Backend expecting `user_id`
- Database with column `user_identifier`

With Kiro-SDD:
- Schema defines: `user_id UUID`
- API specifies: `user_id: string (UUID format)`
- Interface defines: `userId: string // maps to user_id`
- Implementation references these specifications

## Documentation

- [Getting Started Guide](docs/getting-started.md)
- [Methodology Overview](docs/methodology.md)
- [Example: Health Check](.kiro/specs/health-check/)

## Philosophy

Kiro-SDD follows these principles:

1. **Contract First**: Define contracts before code
2. **Approval Gates**: Human review at each phase
3. **Reference Mandatory**: Every task links to specs
4. **Type Safety**: Enforce type consistency across layers
5. **Incremental Progress**: Small, validated steps

## Contributing

This framework is based on the Kiro methodology for specification-driven development. Contributions that align with the core philosophy are welcome.

## License

MIT License - See [LICENSE](LICENSE) file for details.

## Acknowledgments

- Based on [claude-code-spec](https://github.com/gotalab/claude-code-spec) - the original specification-driven development framework
- Inspired by Kiro's specification-driven development methodology
- Built for Claude Code and AI-assisted development workflows
- Designed to solve real-world AI development integration failures
- Special thanks to the claude-code-spec team for providing the foundation for this enhanced framework

---

**Remember**: The goal is to prevent integration failures by ensuring every layer respects the contracts defined in specifications.