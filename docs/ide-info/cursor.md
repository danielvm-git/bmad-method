# BMAD Method - Cursor Instructions

## Activating Agents

BMAD agents are installed in `.cursor/rules/bmad/` as MDC rules.

### How to Use

1. **Reference in Chat**: Use `@bmad/{module}/agents/{agent-name}`
2. **Include Entire Module**: Use `@bmad/{module}`
3. **Reference Index**: Use `@bmad/index` for all available agents

### Examples

```
@bmad/core/agents/dev - Activate dev agent
@bmad/bmm/agents/architect - Activate architect agent
@bmad/core - Include all core agents/tasks
```

### Notes

- Rules are Manual type - only loaded when explicitly referenced
- No automatic context pollution
- Can combine multiple agents: `@bmad/core/agents/dev @bmad/core/agents/test`

---

## Code Snippets

BMAD provides code snippets to speed up agent and workflow invocations.

### Available Snippets

Snippets are automatically installed to `.vscode/bmad.code-snippets` during BMAD installation.

**Agent Shortcuts** (type prefix + Tab):

- `bdev` → `@bmad/bmm/agents/dev`
- `bpm` → `@bmad/bmm/agents/pm`
- `barch` → `@bmad/bmm/agents/architect`
- `bsm` → `@bmad/bmm/agents/sm`
- `btea` → `@bmad/bmm/agents/tea`
- `bmaster` → `@bmad/core/agents/bmad-master`

**Workflow Shortcuts**:

- `bstory` → Load Dev + run `*dev-story`
- `bprd` → Load PM + run `*prd`
- `barchitecture` → Load Architect + run `*architecture`
- `bsprint` → Load SM + run `*sprint-planning`

**Module References**:

- `bcore` → `@bmad/core`
- `bbmm` → `@bmad/bmm`
- `bindex` → `@bmad/index`

### Usage

1. Open a Markdown file (`.md`) or plain text file
2. Type snippet prefix (e.g., `bdev`)
3. Press Tab or Ctrl+Space to expand

### Customization

Edit `.vscode/bmad.code-snippets` in your project to add custom snippets. Your changes will be preserved during BMAD updates.
