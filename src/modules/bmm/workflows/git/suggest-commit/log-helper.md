# Workflow Logs Helper Instructions

## Purpose

This document provides instructions for appending workflow time tracking data to the persistent log file `.bmad/workflow-logs.yaml`.

## Log File Location

```
{output_folder}/.bmad/workflow-logs.yaml
```

## Log File Structure

```yaml
# Workflow execution history
# This file tracks all workflow executions with time tracking enabled

workflow_history:
  - workflow: 'dev-story'
    started: '2025-11-09T14:30:00-03:00'
    finished: '2025-11-09T16:45:00-03:00'
    duration: '2 hours 15 minutes'
    commit_suggested: 'feat(auth): implement user login'

  - workflow: 'story-done'
    started: '2025-11-09T16:50:00-03:00'
    finished: '2025-11-09T16:52:00-03:00'
    duration: '2 minutes'
    commit_suggested: 'chore(bmm): mark story 1-2 as done'
```

## Instructions for Appending

### Step 1: Check if file exists

<action>Check if {output_folder}/.bmad/ directory exists</action>
<action>If directory doesn't exist, create it: `mkdir -p {output_folder}/.bmad`</action>

<action>Check if {output_folder}/.bmad/workflow-logs.yaml file exists</action>

### Step 2: Initialize file if needed

<check if="file doesn't exist">
  <action>Create new file with initial structure:</action>
  
```yaml
# Workflow execution history
# This file tracks all workflow executions with time tracking enabled

workflow_history: []

````
</check>

### Step 3: Read existing content

<action>Read COMPLETE file content from {output_folder}/.bmad/workflow-logs.yaml</action>
<action>Parse YAML structure</action>
<action>Locate the `workflow_history` array</action>
<action>Preserve ALL existing entries</action>

### Step 4: Prepare new entry

<action>Create new entry object with required fields:</action>

```yaml
workflow: "{workflow_name}"
started: "{start_date}"      # ISO 8601 format
finished: "{finish_date}"    # ISO 8601 format
duration: "{time_spent}"     # Human readable
commit_suggested: "{primary_message}"
````

### Step 5: Append entry

<action>Add new entry to the END of workflow_history array</action>
<action>Maintain chronological order (oldest to newest)</action>
<action>Preserve formatting and indentation</action>
<action>Ensure valid YAML syntax</action>

### Step 6: Write back to file

<action>Write updated content back to {output_folder}/.bmad/workflow-logs.yaml</action>
<action>Verify file was written successfully</action>

## Example Append Operation

**Before:**

```yaml
workflow_history:
  - workflow: 'dev-story'
    started: '2025-11-09T14:30:00-03:00'
    finished: '2025-11-09T16:45:00-03:00'
    duration: '2 hours 15 minutes'
    commit_suggested: 'feat(auth): implement user login'
```

**After:**

```yaml
workflow_history:
  - workflow: 'dev-story'
    started: '2025-11-09T14:30:00-03:00'
    finished: '2025-11-09T16:45:00-03:00'
    duration: '2 hours 15 minutes'
    commit_suggested: 'feat(auth): implement user login'

  - workflow: 'story-done'
    started: '2025-11-09T16:50:00-03:00'
    finished: '2025-11-09T16:52:00-03:00'
    duration: '2 minutes'
    commit_suggested: 'chore(bmm): mark story 1-2 as done'
```

## Error Handling

### File doesn't exist

→ Create directory and file with initial structure

### File is corrupted or invalid YAML

→ Backup corrupted file (rename to workflow-logs.yaml.backup)
→ Create new file with current entry only
→ Warn user about backup

### Insufficient permissions

→ Warn user about permission error
→ Display entry that couldn't be saved
→ Continue workflow execution

### Disk space issues

→ Warn user about disk space
→ Skip appending to file
→ Continue workflow execution

## Data Retention

The workflow-logs.yaml file grows over time. Consider these maintenance options:

**Manual cleanup:**
Users can manually archive or trim old entries from the file.

**Automated rotation:**
Future enhancement could implement automatic log rotation (e.g., monthly archives).

**Analysis tools:**
Future enhancement could provide tools to analyze workflow logs and generate velocity reports.

## Privacy Considerations

**What's logged:**

- Workflow names
- Timestamps (ISO 8601 with timezone)
- Duration calculations
- Suggested commit messages

**What's NOT logged:**

- File contents
- User credentials
- Sensitive project data
- Personal information

## Usage in Suggest-Commit Workflow

The suggest-commit workflow invokes these instructions in step 8:

```markdown
<step n="8" goal="Persist Time Tracking">
  <action>Load helper instructions: {installed_path}/log-helper.md</action>
  <action>Follow log-helper instructions to append tracking data</action>
  ...
</step>
```

## See Also

- **Time Tracking Template**: `src/modules/bmm/workflows/tracking/workflow-time-tracking-template.yaml`
- **Workflow Engine**: `src/core/tasks/workflow.xml` (step 3a for time tracking)
- **Date Format Standard**: `docs/date-format-standard.md`
