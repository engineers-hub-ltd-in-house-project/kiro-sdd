# Getting Started with Kiro-SDD

## Prerequisites

- Claude Code CLI installed
- A project repository
- Basic understanding of specification-driven development

## Installation

### Step 1: Copy Commands to Your Project

Copy the `commands/kiro/` directory to your project's `.claude/commands/` directory:

```bash
# From your project root
mkdir -p .claude/commands
cp -r path/to/kiro-sdd/commands/kiro .claude/commands/
```

### Step 2: Verify Installation

Check that commands are available:
```bash
# List available Kiro commands
ls .claude/commands/kiro/
```

## Your First Specification

### Step 1: Initialize a Specification

Start with a clear feature description:

```bash
/kiro:spec-init "User authentication system with email/password login, JWT tokens, and password reset functionality"
```

This creates:
- `.kiro/specs/user-authentication/` directory
- Initial templates for all specification documents
- `spec.json` for tracking approvals

### Step 2: Generate Requirements

```bash
/kiro:spec-requirements user-authentication
```

**Review the generated `requirements.md`** and edit if needed. This document defines:
- User stories
- Functional requirements
- Non-functional requirements
- Acceptance criteria

### Step 3: Define Use Cases

```bash
/kiro:spec-usecase user-authentication
```

When prompted: "Have you reviewed requirements.md? [y/N]"
- Answer `y` to approve and continue
- Answer `N` to stop and review first

This generates:
- Use case descriptions
- Data elements required
- Actor definitions
- Success/failure scenarios

### Step 4: Create Sequence Diagrams

```bash
/kiro:spec-sequence user-authentication
```

Approves use cases and generates:
- Mermaid sequence diagrams
- API flow documentation
- Error handling flows

### Step 5: Design Database Schema

```bash
/kiro:spec-schema user-authentication
```

Creates:
- Table definitions
- Relationships
- Indexes
- Migration notes

### Step 6: Define API Specification

```bash
/kiro:spec-api user-authentication
```

Generates:
- Endpoint definitions
- Request/response formats
- Error codes
- Rate limiting rules

### Step 7: Create Interface Definitions

```bash
/kiro:spec-interfaces user-authentication
```

Produces:
- TypeScript interfaces
- Rust structs
- Validation schemas
- Type mappings

### Step 8: Write Test Specifications

```bash
/kiro:spec-tests-red user-authentication
```

Creates failing test specifications:
- Unit test scenarios
- Integration test plans
- E2E test cases
- Performance benchmarks

### Step 9: Technical Design

```bash
/kiro:spec-design user-authentication
```

Comprehensive technical design:
- Architecture decisions
- Component design
- Security considerations
- Performance strategies

### Step 10: Generate Tasks

```bash
/kiro:spec-tasks user-authentication
```

Creates implementation tasks with:
- Task breakdown
- Specification references
- Dependencies
- Acceptance criteria

## Implementation Phase

### Check Status

Before starting implementation:

```bash
/kiro:spec-status user-authentication
```

Output:
```
✅ Requirements: Approved
✅ Use Cases: Approved
✅ Sequences: Approved
✅ Schema: Approved
✅ API: Approved
✅ Interfaces: Approved
✅ Tests: Approved
✅ Design: Approved
✅ Tasks: Approved

Ready for implementation: YES
```

### Validate References

Ensure all tasks reference specifications:

```bash
/kiro:spec-validate user-authentication
```

### Start Implementation

With all approvals complete, begin coding:

1. Pick a task from `tasks.md`
2. Follow the specification references
3. Implement according to the defined contracts
4. Mark task as complete
5. Run tests to verify

## Best Practices

### 1. Review Thoroughly

Each approval gate is critical:
- Don't rush through approvals
- Edit specifications if they don't match your vision
- Ensure consistency across documents

### 2. Keep Specifications Updated

As implementation reveals new requirements:
- Update the relevant specification
- Re-run dependent specifications if needed
- Keep `spec.json` status accurate

### 3. Use Specification References

In your code:
```typescript
// Implementation of: api.md#post-login
// Interface: interfaces.md#LoginRequest
async function login(request: LoginRequest): Promise<LoginResponse> {
  // ...
}
```

### 4. Leverage Validation

Regularly run:
```bash
/kiro:spec-validate feature-name
```

This ensures:
- No orphaned specifications
- Complete task coverage
- Proper reference linking

## Common Workflows

### Adding a New Endpoint

1. Update `api.md` with endpoint specification
2. Update `interfaces.md` with new types
3. Update `tests-red.md` with test cases
4. Add task in `tasks.md` with references
5. Run `/kiro:spec-validate` to verify

### Modifying Database Schema

1. Update `schema.md` with changes
2. Review impact on `api.md`
3. Update `interfaces.md` if needed
4. Add migration task in `tasks.md`
5. Update affected test specifications

### Handling Requirement Changes

1. Update `requirements.md`
2. Re-run affected specifications
3. Update approval status
4. Validate all references still valid
5. Update implementation tasks

## Troubleshooting

### "Specification not found"

- Check feature name spelling
- Verify `.kiro/specs/feature-name/` exists
- Ensure `spec.json` is present

### "Approval required"

- Review the prerequisite phase
- Run the prerequisite command with approval
- Or manually update `spec.json`

### "Invalid references"

- Run `/kiro:spec-validate`
- Fix broken references in `tasks.md`
- Ensure specification sections exist

## Next Steps

- Explore the [methodology documentation](methodology.md)
- Review the [health-check example](../examples/health-check/)
- Start your own specification-driven project

Remember: **No code without specification, no specification without approval.**