# Getting Started with Kiro-SDD

## Prerequisites

- Claude Code CLI installed
- A project repository
- Basic understanding of specification-driven development

## Installation

### Step 1: Copy Commands to Your Project

Copy the `.claude/commands/kiro/` directory to your project's `.claude/commands/` directory:

```bash
# From your project root
mkdir -p .claude/commands
cp -r path/to/kiro-sdd/.claude/commands/kiro .claude/commands/
```

### Step 2: Copy Steering Templates (Optional but Recommended)

```bash
# Copy steering templates
cp -r path/to/kiro-sdd/.kiro/steering .kiro/
```

### Step 3: Initialize Steering Documents

```bash
/kiro:steering
```

### Step 4: Verify Installation

Check that commands are available:
```bash
# List available Kiro commands
ls .claude/commands/kiro/
```

## Your First Specification

### Step 0: Steering (Optional)

If you haven't already, initialize steering documents to provide project context:

```bash
/kiro:steering
/kiro:steering-custom  # For specialized contexts
```

**Note**: This is optional for new features or small additions. You can proceed directly to spec-init.

### Step 1: Initialize a Specification

Start with a clear, detailed feature description:

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
# For REST API (default)
/kiro:spec-api user-authentication

# For gRPC
/kiro:spec-api user-authentication --type grpc

# For GraphQL
/kiro:spec-api user-authentication --type graphql
```

Generates:
- REST: OpenAPI specification with endpoints, request/response formats
- gRPC: Protocol Buffer definitions with services and messages
- GraphQL: Schema with types, queries, mutations, and subscriptions
- Error codes and rate limiting rules

### Step 7: Create Interface Definitions

```bash
/kiro:spec-interfaces user-authentication
```

Produces shared type definitions that enforce consistency:
- TypeScript interfaces
- Type mappings between layers (e.g., `userId` → `user_id`)
- Validation schemas
- Ensures type safety across frontend/backend

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

**Important**: You will be prompted "Have you reviewed all specifications? [y/N]"

Comprehensive technical design that:
- Integrates all previous specifications
- Provides architecture decisions
- Includes traceability matrix
- Documents decision records

### Step 10: Generate Tasks

```bash
/kiro:spec-tasks user-authentication
```

**Important**: You will be prompted "Have you reviewed all specifications including tests? [y/N]"

Creates implementation tasks with:
- Task breakdown
- **Mandatory specification references** for every task
- Dependencies
- Acceptance criteria
- No AI self-judgment - follows specs exactly

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

## Key Principles

### Contract Enforcement

Kiro-SDD prevents integration failures by enforcing contracts:
- **Schema First**: Database defines the source of truth
- **API Contracts**: Exact parameters and responses defined
- **Type Safety**: Consistent types across all layers
- **No AI Self-Judgment**: Implementation follows specs exactly

### Why This Matters

Without Kiro-SDD, AI independently generates:
- Frontend: sends `userId`
- Backend: expects `user_id`
- Database: has column `user_identifier`

With Kiro-SDD:
- Schema defines: `user_id UUID`
- API specifies: `user_id: string (UUID format)` in REST/gRPC/GraphQL
- Interface defines: `userId: string // maps to user_id`
- Implementation references these specifications

## Best Practices

### 1. Review Thoroughly

Each approval gate is critical:
- Don't rush through approvals
- Edit specifications if they don't match your vision
- Ensure consistency across documents
- Interactive approvals eliminate manual spec.json editing

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
- Review the [health-check example](../.kiro/specs/health-check/)
- Start your own specification-driven project
- Check CLAUDE.md for project-specific guidelines

Remember: **Every layer must respect defined contracts, no implementation without specification reference.**