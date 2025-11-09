# Epic Backlog Template

Use this template to track and manage epics across your project.

---

# Epic Backlog - {Project Name}

**Project**: {Project Name}  
**Last Updated**: YYYY-MM-DD  
**Product Manager**: {Name}  
**Total Epics**: {Count}

---

## Active Sprint

| ID       | Title                      | Status      | Assignee   | T-shirt | Function Points | Iteration | Start Date | Target Date | Estimated Effort |
| -------- | -------------------------- | ----------- | ---------- | ------- | --------------- | --------- | ---------- | ----------- | ---------------- |
| EPIC-001 | User Authentication System | In Progress | @dev-team  | L       | 13              | Sprint 1  | 2025-11-01 | 2025-11-15  | 6-8h             |
| EPIC-002 | Dashboard Analytics        | Backlog     | @data-team | M       | 8               | Sprint 2  | 2025-11-16 | 2025-11-30  | 3-4h             |

---

## Backlog

| ID       | Title               | Status  | Assignee   | T-shirt | Function Points | Iteration | Start Date | Target Date | Estimated Effort |
| -------- | ------------------- | ------- | ---------- | ------- | --------------- | --------- | ---------- | ----------- | ---------------- |
| EPIC-003 | Payment Processing  | Backlog | Unassigned | XL      | 21              | TBD       | TBD        | TBD         | 8-10h            |
| EPIC-004 | Notification System | Backlog | Unassigned | M       | 8               | TBD       | TBD        | TBD         | 3-4h             |
| EPIC-005 | Mobile App Support  | Backlog | Unassigned | L       | 13              | TBD       | TBD        | TBD         | 6-8h             |

---

## Completed

| ID       | Title         | Status | Assignee    | T-shirt | Function Points | Iteration | Start Date | Target Date | Estimated Effort | Actual Effort |
| -------- | ------------- | ------ | ----------- | ------- | --------------- | --------- | ---------- | ----------- | ---------------- | ------------- |
| EPIC-000 | Project Setup | Done   | @infra-team | S       | 3               | Sprint 0  | 2025-10-15 | 2025-10-31  | 1-2h             | 1.5h          |

---

## Blocked

| ID       | Title                   | Status  | Assignee          | T-shirt | Function Points | Blocker                | Expected Resolution |
| -------- | ----------------------- | ------- | ----------------- | ------- | --------------- | ---------------------- | ------------------- |
| EPIC-006 | Third-party Integration | Blocked | @integration-team | M       | 8               | Waiting for API access | 2025-12-01          |

---

## Field Definitions

### ID

Unique identifier for the epic following format: `EPIC-XXX`

- Sequential numbering starting from 001
- Zero-padded to 3 digits
- Example: EPIC-001, EPIC-023, EPIC-142

### Title

Concise, descriptive name for the epic (max 60 characters).

- Use title case
- Be specific and actionable
- Examples: "User Authentication System", "Payment Processing Integration"

### Status

Current state of the epic. Valid values:

- **Backlog**: Not yet started, awaiting prioritization
- **In Progress**: Currently being worked on
- **Done**: Completed and deployed
- **Blocked**: Cannot proceed due to external dependency

### Assignee

GitHub username or team responsible for the epic.

- Format: `@username` or `@team-name`
- Use "Unassigned" if not yet assigned
- Examples: `@alice`, `@backend-team`

### T-shirt Size

Relative size estimation using t-shirt sizes.

- **XS** (Extra Small): 1-2 hours
- **S** (Small): 2-4 hours
- **M** (Medium): 1-3 days
- **L** (Large): 1-2 weeks
- **XL** (Extra Large): 2-4 weeks
- **XXL** (Extra Extra Large): 1+ month

### Function Points

Numeric estimation of epic complexity using Function Point Analysis.

- **1-3 points**: Simple feature, minimal complexity
- **5-8 points**: Moderate complexity, standard feature
- **13-21 points**: Complex feature, multiple components
- **34+ points**: Very complex, consider splitting

Common values: 1, 2, 3, 5, 8, 13, 21, 34 (Fibonacci sequence)

### Iteration

Sprint or iteration where the epic is planned.

- Format: "Sprint N" or "TBD" if not yet scheduled
- Examples: "Sprint 1", "Sprint 3", "TBD"

### Start Date

Actual or planned start date for epic work.

- Format: YYYY-MM-DD (ISO 8601)
- Use "TBD" if not yet scheduled
- Example: "2025-11-01"

### Target Date

Expected completion date.

- Format: YYYY-MM-DD (ISO 8601)
- Use "TBD" if not yet scheduled
- Should align with sprint end dates
- Example: "2025-11-15"

### Estimated Effort

Time estimate for completing the epic.

- Format: "X-Y hours" or "X-Y days"
- Range accounts for uncertainty
- Examples: "6-8h", "2-3 days", "1-2 weeks"

### Actual Effort

Actual time spent (tracked only for completed epics).

- Format: "X hours" or "X days"
- Used for improving future estimates
- Example: "7.5h", "2.5 days"

---

## Usage Instructions

### Adding a New Epic

1. Copy a row from the Backlog section
2. Assign the next available ID (EPIC-XXX)
3. Fill in Title, T-shirt, and Function Points
4. Leave Status as "Backlog"
5. Set Assignee to "Unassigned" initially
6. Use "TBD" for dates until sprint planning

### Moving Epic to Active Sprint

1. Update Status to "In Progress"
2. Assign to team or individual
3. Set Start Date and Target Date
4. Move row to Active Sprint section
5. Update Iteration field

### Completing an Epic

1. Update Status to "Done"
2. Record Actual Effort
3. Move row to Completed section
4. Update project version (One Epic = One Minor Version)

### Blocking an Epic

1. Update Status to "Blocked"
2. Move row to Blocked section
3. Add Blocker description
4. Set Expected Resolution date
5. Track blocker resolution

---

## Maintenance Schedule

- **Daily**: Update Status for active epics
- **Weekly**: Review and reprioritize Backlog
- **Sprint Planning**: Assign epics to upcoming sprint
- **Sprint Review**: Move completed epics to Done section
- **Monthly**: Archive old completed epics to separate file

---

## Version Strategy Integration

**One Epic = One Minor Version**

When an epic is completed:

1. Epic completion triggers a minor version bump
2. Update `CHANGELOG.md` with epic details
3. Tag release: `v{MAJOR}.{MINOR}.0`
4. Example: Epic "User Authentication" → v1.1.0

See [Version Strategy Guide](../../docs/version-strategy.md) for details.

---

## Example: Complete Epic Entry

```markdown
| ID       | Title                      | Status      | Assignee | T-shirt | Function Points | Iteration | Start Date | Target Date | Estimated Effort |
| -------- | -------------------------- | ----------- | -------- | ------- | --------------- | --------- | ---------- | ----------- | ---------------- |
| EPIC-001 | User Authentication System | In Progress | @alice   | L       | 13              | Sprint 1  | 2025-11-01 | 2025-11-15  | 6-8h             |
```

**Epic Details**:

- **ID**: EPIC-001 (first epic in project)
- **Title**: Clear, specific name
- **Status**: Currently being worked on
- **Assignee**: Alice is responsible
- **T-shirt**: Large (1-2 weeks effort)
- **Function Points**: 13 (complex feature)
- **Iteration**: Planned for Sprint 1
- **Dates**: Starts Nov 1, targets Nov 15
- **Effort**: Estimated 6-8 hours total

---

## Tips for Effective Epic Management

### Do's

✅ Keep epic titles concise and descriptive  
✅ Update status regularly (at least weekly)  
✅ Break down XXL epics into smaller epics  
✅ Use consistent terminology across epics  
✅ Track dependencies between epics  
✅ Review and refine estimates after each epic  
✅ Celebrate completed epics with the team

### Don'ts

❌ Don't create epics that are too small (use stories instead)  
❌ Don't let backlog grow beyond 20-30 unstarted epics  
❌ Don't skip updating actual effort for completed epics  
❌ Don't assign multiple teams to one epic without clear coordination  
❌ Don't change epic scope without updating estimates  
❌ Don't forget to link epics to related stories and tasks

---

## Related Documentation

- **[Version Strategy](../../docs/version-strategy.md)** - How epics relate to releases
- **[FDD/SDD Guide](../../docs/fdd-sdd-guide.md)** - Feature-Driven Development approach
- **[Docs Structure](../../.docs-structure.md)** - Where to store epic documentation

---

**Template Version**: 1.0  
**Last Updated**: 2025-11-09
