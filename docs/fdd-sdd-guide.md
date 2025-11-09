# Feature-Driven Development (FDD) and Spec-Driven Development (SDD) Guide

This guide explains how BMAD implements both Feature-Driven Development (FDD) and Specification-Driven Development (SDD) methodologies to deliver high-quality software.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Feature-Driven Development (FDD)](#feature-driven-development-fdd)
3. [Specification-Driven Development (SDD)](#specification-driven-development-sdd)
4. [BMAD Implementation](#bmad-implementation)
5. [When to Use Each Approach](#when-to-use-each-approach)
6. [Story Creation Workflow](#story-creation-workflow)
7. [Acceptance Criteria Definition](#acceptance-criteria-definition)

---

## Introduction

BMAD combines two powerful methodologies to deliver features efficiently:

- **Feature-Driven Development (FDD)**: Organizes work around business features
- **Specification-Driven Development (SDD)**: Uses specifications as the source of truth for implementation

Together, they create a workflow where **features** drive what we build, and **specifications** drive how we build it.

---

## Feature-Driven Development (FDD)

### Overview

FDD is an iterative, agile methodology that organizes software development around **features** rather than technologies or layers.

### Core Principles

1. **Feature-Centric**: Everything is organized around features that provide value
2. **Client-Valued Functionality**: Features must be valuable to end users
3. **Short Iterations**: Build features in small, incremental steps
4. **Regular Builds**: Frequent integration and testing
5. **Progress Tracking**: Track completion of features, not just tasks

### FDD Process

```
1. Develop Overall Model
   ↓
2. Build Feature List
   ↓
3. Plan By Feature
   ↓
4. Design By Feature
   ↓
5. Build By Feature
```

### Features in BMAD

In BMAD, features are organized hierarchically:

**Epic** (Large Feature)

- Duration: 1-4 weeks
- Maps to: Minor version bump
- Example: "User Authentication System"

**Story** (Small Feature)

- Duration: 1-3 days
- Maps to: Implementation unit
- Example: "Login with email and password"

**Task** (Implementation Detail)

- Duration: 1-4 hours
- Maps to: Code changes
- Example: "Create login API endpoint"

---

## Specification-Driven Development (SDD)

### Overview

SDD treats specifications as **executable contracts** between stakeholders and developers. The specification is the authoritative source of truth.

### Core Principles

1. **Specifications First**: Write specs before code
2. **Living Documentation**: Specs are maintained, not abandoned
3. **Single Source of Truth**: Code implements the spec, not vice versa
4. **Testable Requirements**: Specs contain acceptance criteria that become tests
5. **Stakeholder Collaboration**: Specs are written collaboratively

### SDD Process

```
1. Write Specification
   ↓
2. Review & Approve Specification
   ↓
3. Generate Tests from Spec
   ↓
4. Implement to Satisfy Tests
   ↓
5. Validate Against Specification
```

### Reference

BMAD's SDD approach is inspired by GitHub's Spec-Kit:  
**[Spec-Driven Development](https://github.com/github/spec-kit/blob/main/spec-driven.md)**

---

## BMAD Implementation

### How BMAD Combines FDD and SDD

```
FDD (WHAT)          SDD (HOW)           Result
────────────        ─────────           ──────
Epic                Epic Spec           Feature Set
  ↓                   ↓                   ↓
Story               Story Spec          Implementation Unit
  ↓                   ↓                   ↓
Tasks               Acceptance          Code + Tests
                    Criteria
```

### The BMAD Workflow

1. **Epic Definition** (FDD)
   - Product Manager defines epic
   - Epic represents business value
   - Epic broken into stories

2. **Story Specification** (SDD)
   - Each story has detailed spec
   - Acceptance criteria defined
   - Technical approach documented

3. **Implementation** (FDD + SDD)
   - Developer implements story
   - Tests written from acceptance criteria
   - Code satisfies specification

4. **Validation** (SDD)
   - Tests validate implementation
   - Specification serves as acceptance
   - Stakeholder verifies business value

---

## When to Use Each Approach

### Use FDD When

✅ **Organizing Project Work**

- Breaking down large projects into manageable features
- Planning sprints around feature delivery
- Communicating progress to stakeholders
- Estimating project timelines

✅ **Customer Communication**

- Discussing what will be built
- Prioritizing features
- Demonstrating value delivery

### Use SDD When

✅ **Defining Requirements**

- Writing detailed acceptance criteria
- Creating implementation contracts
- Documenting technical decisions
- Ensuring testability

✅ **Team Coordination**

- Aligning developer understanding
- Creating shared mental models
- Reducing ambiguity
- Enabling parallel development

### Use Both When

✅ **Building Complex Features**

- Epic (FDD) defines the feature set
- Stories (FDD) break down the epic
- Story specs (SDD) detail implementation
- Acceptance criteria (SDD) define done

---

## Story Creation Workflow

### Step 1: Epic Planning (FDD)

**Product Manager** creates epic:

```markdown
# Epic: User Authentication System

## Business Value

Enable users to securely access their accounts.

## Features

1. Email/password login
2. Social authentication (Google, GitHub)
3. Password reset
4. Two-factor authentication
5. Session management
6. Account security settings
```

### Step 2: Story Breakdown (FDD)

**Product Manager + Tech Lead** break epic into stories:

```markdown
## Stories

1.1 - Login with email and password
1.2 - Social authentication with Google
1.3 - Social authentication with GitHub
1.4 - Password reset flow
1.5 - Two-factor authentication setup
1.6 - Two-factor authentication login
1.7 - Session management and timeout
1.8 - Account security dashboard
```

### Step 3: Story Specification (SDD)

**Product Manager + Developer** write story spec:

````markdown
# Story 1.1: Login with Email and Password

## User Story

As a registered user,  
I want to login with my email and password,  
So that I can access my account securely.

## Acceptance Criteria

1. **GIVEN** I am on the login page  
   **WHEN** I enter valid email and password  
   **THEN** I am redirected to my dashboard

2. **GIVEN** I am on the login page  
   **WHEN** I enter invalid credentials  
   **THEN** I see an error message "Invalid email or password"

3. **GIVEN** I have entered incorrect password 5 times  
   **WHEN** I try to login again  
   **THEN** my account is temporarily locked for 15 minutes

## Technical Specification

### API Endpoint

POST /api/auth/login

**Request**:

```json
{
  "email": "user@example.com",
  "password": "securepassword123"
}
```
````

**Response (Success)**:

```json
{
  "success": true,
  "token": "jwt-token-here",
  "user": { ... }
}
```

### Database Changes

None (uses existing users table)

### Security Requirements

- Passwords must be hashed with bcrypt
- Rate limiting: 10 requests per minute per IP
- Login attempts tracked for account lockout
- JWT tokens expire after 24 hours

````

### Step 4: Implementation (FDD + SDD)

**Developer** implements using specification:

```typescript
// Test written from acceptance criteria
describe('Login', () => {
  it('should redirect to dashboard with valid credentials', async () => {
    // Implements AC #1
    const response = await login('user@example.com', 'validpassword');
    expect(response.success).toBe(true);
    expect(window.location.pathname).toBe('/dashboard');
  });

  it('should show error with invalid credentials', async () => {
    // Implements AC #2
    const response = await login('user@example.com', 'wrongpassword');
    expect(response.error).toBe('Invalid email or password');
  });

  it('should lock account after 5 failed attempts', async () => {
    // Implements AC #3
    for (let i = 0; i < 5; i++) {
      await login('user@example.com', 'wrongpassword');
    }
    const response = await login('user@example.com', 'wrongpassword');
    expect(response.error).toContain('temporarily locked');
  });
});
````

---

## Acceptance Criteria Definition

### The Three C's

Good acceptance criteria follow the **Three C's**:

1. **Card**: Written on story card (or document)
2. **Conversation**: Refined through discussion
3. **Confirmation**: Testable and verifiable

### Given-When-Then Format

```
GIVEN [initial context]
WHEN [action/event]
THEN [expected outcome]
```

**Example**:

```
GIVEN I am logged in as an admin
WHEN I navigate to the user management page
THEN I see a list of all users with edit/delete actions
```

### Acceptance Criteria Guidelines

✅ **DO**:

- Use clear, unambiguous language
- Make criteria testable
- Cover both happy path and edge cases
- Include non-functional requirements (performance, security)
- Specify error conditions
- Use concrete examples

❌ **DON'T**:

- Write implementation details
- Use vague terms ("should work well")
- Forget edge cases
- Mix multiple scenarios in one criterion
- Skip error handling

### Example: Complete Story with Acceptance Criteria

```markdown
# Story 2.3: Export User Data to CSV

## User Story

As an admin,  
I want to export user data to CSV,  
So that I can analyze user trends in Excel.

## Acceptance Criteria

### AC1: Export All Users

**GIVEN** I am logged in as an admin  
**WHEN** I click "Export to CSV" button on users page  
**THEN** A CSV file downloads with all user data

### AC2: CSV Format

**GIVEN** The CSV export is generated  
**WHEN** I open the file  
**THEN** It contains columns: ID, Name, Email, Created Date, Status

### AC3: Large Dataset Handling

**GIVEN** There are 10,000+ users in the system  
**WHEN** I export to CSV  
**THEN** The download completes within 10 seconds

### AC4: Permission Check

**GIVEN** I am logged in as a regular user (not admin)  
**WHEN** I try to access the export function  
**THEN** I see a "Permission denied" error message

### AC5: Empty Data

**GIVEN** There are no users in the system  
**WHEN** I export to CSV  
**THEN** I get a CSV with headers only and a message "No data to export"

## Technical Notes

- Use streaming for large datasets
- Format dates as YYYY-MM-DD
- Escape special characters in CSV
- Set appropriate MIME type for download
```

---

## BMAD Workflow Summary

### 1. Discovery Phase

**Approach**: FDD  
**Artifacts**: Product brief, epic list  
**Owner**: Product Manager, Analyst

### 2. Planning Phase

**Approach**: FDD + SDD  
**Artifacts**: PRD, epic specifications, story breakdown  
**Owner**: Product Manager

### 3. Solutioning Phase

**Approach**: SDD  
**Artifacts**: Architecture doc, technical specifications  
**Owner**: Architect

### 4. Implementation Phase

**Approach**: FDD + SDD  
**Artifacts**: Story specifications, code, tests  
**Owner**: Developer

### 5. Validation Phase

**Approach**: SDD  
**Artifacts**: Test reports, acceptance validation  
**Owner**: Test Architect, Scrum Master

---

## Benefits of FDD + SDD

### For Product Managers

✅ Clear feature visibility and progress tracking  
✅ Ability to prioritize by business value  
✅ Reduced requirements ambiguity  
✅ Easier stakeholder communication

### For Developers

✅ Clear, testable requirements  
✅ Reduced back-and-forth with PM  
✅ Confidence in what to build  
✅ Better test coverage

### For QA/Test Architects

✅ Acceptance criteria become test cases  
✅ Clear definition of "done"  
✅ Reduced manual test documentation  
✅ Automated validation possible

### For Organizations

✅ Faster feature delivery  
✅ Higher quality code  
✅ Better requirement traceability  
✅ Reduced rework

---

## Tools and Templates

### BMAD Templates

- **[Epic Backlog Template](../src/utility/templates/EPIC_BACKLOG_TEMPLATE.md)** - Track epics and features
- **[User Story Template](../src/modules/bmm/workflows/2-plan-workflows/tech-spec/user-story-template.md)** - Story format
- **[PRD Template](../src/modules/bmm/workflows/2-plan-workflows/prd/prd-template.md)** - Requirements documentation

### BMAD Workflows

- `*workflow-init` - Initialize project tracking
- `*create-prd` - Create Product Requirements Document
- `*tech-spec` - Create technical specifications
- `*create-story` - Generate story from spec

---

## Best Practices

### FDD Best Practices

1. **Keep Features Small**: 1-3 days of work maximum
2. **Feature Complete**: Each feature should be shippable
3. **Regular Integration**: Integrate features frequently
4. **Track Progress**: Use epic backlog to visualize completion

### SDD Best Practices

1. **Specs Before Code**: Always write spec first
2. **Keep Specs Current**: Update specs with code changes
3. **Collaborative Writing**: Include PM, Dev, QA in spec creation
4. **Testable Criteria**: Every AC should become an automated test

---

## Common Pitfalls

### ❌ Feature Bloat

**Problem**: Features become too large, taking weeks to complete.  
**Solution**: Split into smaller features. Use epic for large work, stories for small.

### ❌ Specification Drift

**Problem**: Code diverges from specification over time.  
**Solution**: Treat specs as living documents. Update specs when requirements change.

### ❌ Over-Specification

**Problem**: Specs contain too much implementation detail.  
**Solution**: Focus on "what" and "why", not "how". Leave implementation details to developers.

### ❌ Under-Specification

**Problem**: Specs too vague, developers unclear on requirements.  
**Solution**: Use concrete examples, acceptance criteria, and edge cases.

---

## Related Documentation

- **[Naming Conventions](./naming-conventions-guide.md)** - File and code naming
- **[Version Strategy](./version-strategy.md)** - Epic-to-version mapping
- **[Docs Structure](../.docs-structure.md)** - Where to store specs

---

## References

- **Spec-Driven Development**: https://github.com/github/spec-kit/blob/main/spec-driven.md
- **Feature-Driven Development**: https://en.wikipedia.org/wiki/Feature-driven_development
- **User Story Format**: https://www.agilealliance.org/glossary/user-stories/
- **Gherkin Syntax** (Given-When-Then): https://cucumber.io/docs/gherkin/

---

**Last Updated**: 2025-11-09  
**Version**: 1.0
