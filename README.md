# Kiro-SDD: Specification-Driven Development Framework

> A comprehensive specification-driven development framework inspired by Kiro methodology, designed to prevent the common "70% rewrite" problem in AI-assisted development.

## 🎯 Problem Statement

In AI-assisted development, approximately 70% of database queries need to be rewritten during frontend-backend integration. This occurs because:
- Design phases attempt to handle DB schema, API, and UI simultaneously
- Lack of clear specification boundaries between phases
- AI makes self-judgments without proper specification references
- Missing approval gates between critical development phases

## 💡 Solution

Kiro-SDD enforces a strict specification-driven workflow with:
- **Discrete phases** with mandatory approvals
- **Specification references** in every implementation task
- **Interactive approval prompts** between phases
- **Clear separation** of concerns (data → API → UI)

## 🚀 Quick Start

### Installation

1. Copy the `commands/kiro/` directory to your project's `.claude/commands/` directory
2. Initialize your first specification:
```bash
/kiro:spec-init "Your feature description here"
```

### Basic Workflow

```mermaid
graph LR
    A[Requirements] -->|Approve| B[Use Cases]
    B -->|Approve| C[Sequence]
    C -->|Approve| D[Schema]
    D -->|Approve| E[API]
    E -->|Approve| F[Interfaces]
    F -->|Approve| G[Tests]
    G -->|Approve| H[Design]
    H -->|Approve| I[Tasks]
    I -->|Ready| J[Implementation]
```

## 📁 Project Structure

```
your-project/
├── .claude/
│   └── commands/
│       └── kiro/           # Copy commands here
└── .kiro/
    └── specs/
        └── your-feature/
            ├── README.md           # Feature overview
            ├── spec.json          # Metadata & approvals
            ├── requirements.md    # Business requirements
            ├── usecase.md        # Use cases & data
            ├── sequence.md       # Sequence diagrams
            ├── schema.md         # Database design
            ├── api.md           # API specification
            ├── interfaces.md    # Type definitions
            ├── tests-red.md     # Test specifications
            ├── design.md        # Technical design
            └── tasks.md         # Implementation tasks
```

## 🔧 Available Commands

### Core Specification Commands
- `/kiro:spec-init [description]` - Initialize new specification
- `/kiro:spec-requirements [feature]` - Generate requirements
- `/kiro:spec-usecase [feature]` - Define use cases
- `/kiro:spec-sequence [feature]` - Create sequence diagrams
- `/kiro:spec-schema [feature]` - Design database schema
- `/kiro:spec-api [feature]` - Define API contracts
- `/kiro:spec-interfaces [feature]` - Create type definitions
- `/kiro:spec-tests-red [feature]` - Write test specifications
- `/kiro:spec-design [feature]` - Create technical design
- `/kiro:spec-tasks [feature]` - Generate implementation tasks

### Utility Commands
- `/kiro:spec-status [feature]` - Check specification progress
- `/kiro:spec-validate [feature]` - Validate task references

## ✨ Key Features

### 1. Phased Development
Each phase must be approved before proceeding:
- Requirements → Use Cases → Sequences → Schema → API → Interfaces → Tests → Design → Tasks

### 2. Interactive Approvals
```bash
# When running /kiro:spec-design
> "Have you reviewed requirements.md? [y/N]"
# Answering 'y' automatically updates approval status
```

### 3. Specification References
Every implementation task MUST reference relevant specifications:
```markdown
- [ ] Implement health endpoint
  - Reference: api.md#get-health
  - Reference: interfaces.md#HealthResponse
```

### 4. Automatic Validation
The `/kiro:spec-validate` command ensures:
- All tasks have specification references
- No orphaned specifications
- Complete coverage of requirements

## 📚 Documentation

- [Getting Started Guide](docs/getting-started.md)
- [Methodology Overview](docs/methodology.md)
- [Example: Health Check](examples/health-check/)

## 🎓 Philosophy

Kiro-SDD follows these principles:

1. **Specification First**: No code without specification
2. **Approval Gates**: Human review at each phase
3. **Reference Mandatory**: Every task links to specs
4. **English Generation**: Consistent technical documentation
5. **Incremental Progress**: Small, validated steps

## 🤝 Contributing

This framework is based on the Kiro methodology for specification-driven development. Contributions that align with the core philosophy are welcome.

## 📄 License

MIT License - See [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Inspired by Kiro's specification-driven development methodology
- Built for Claude Code and AI-assisted development workflows
- Designed to solve real-world AI development challenges

---

**Remember**: The goal is to prevent the "70% rewrite" problem by ensuring every piece of code has a clear specification and approval before implementation begins.