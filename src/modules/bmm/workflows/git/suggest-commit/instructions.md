# Suggest Commit Workflow Instructions

<critical>The workflow execution engine is governed by: {project-root}/bmad/core/tasks/workflow.xml</critical>
<critical>You MUST have already loaded and processed: {installed_path}/workflow.yaml</critical>
<critical>Communicate all responses in {communication_language}</critical>

<workflow>

<critical>This workflow generates Conventional Commits-formatted commit messages by analyzing git status and provides time tracking for workflow execution</critical>

<step n="1" goal="Analyze Git Status">

<action>Execute terminal command: `git status --short`</action>
<action>Capture the output showing modified, added, and deleted files</action>

<check if="no files modified">
  <output>✨ No files to commit

The working directory is clean. No changes detected.

Run this workflow after making code changes to generate commit messages.
</output>
<action>HALT</action>
</check>

<action>Parse file paths from git status output</action>
<action>Categorize files by top-level directory:

- Files starting with `src/` → category: "src"
- Files starting with `docs/` → category: "docs"
- Files starting with `tools/` → category: "tools"
- Files starting with `test/` or `tests/` → category: "test"
- Files starting with `.github/` → category: "ci"
- Package files (package.json, package-lock.json) → category: "deps"
- Config files (.eslintrc, .prettierrc, etc.) → category: "config"
- Other files → category: "other"
  </action>

<action>Count files in each category</action>
<action>Determine primary category (category with most files)</action>
<action>If files span multiple categories equally, mark as "multiple"</action>

</step>

<step n="2" goal="Determine Commit Scope">

<check if="primary category is 'src'">
  <action>Analyze src/ file paths to detect module:</action>
  <action>- Files in `src/core/` → scope: "core"</action>
  <action>- Files in `src/modules/bmm/` → scope: "bmm"</action>
  <action>- Files in `src/modules/bmd/` → scope: "bmd"</action>
  <action>- Files in `src/modules/bmb/` → scope: "bmb"</action>
  <action>- Files in `src/modules/cis/` → scope: "cis"</action>
  <action>- Files in `src/modules/bmgd/` → scope: "bmgd"</action>
  <action>- Mixed modules → scope: "modules"</action>
</check>

<check if="primary category is 'docs'">
  <action>Set scope: "docs"</action>
</check>

<check if="primary category is 'tools'">
  <action>Set scope: "tools"</action>
</check>

<check if="primary category is 'test'">
  <action>Set scope: "test"</action>
</check>

<check if="primary category is 'ci'">
  <action>Set scope: "ci"</action>
</check>

<check if="primary category is 'deps'">
  <action>Set scope: "deps"</action>
</check>

<check if="primary category is 'config'">
  <action>Set scope: "config"</action>
</check>

<check if="primary category is 'multiple' or 'other'">
  <action>Set scope: "multiple"</action>
  <action>Prepare list of affected categories for user to choose from</action>
</check>

</step>

<step n="3" goal="Select Commit Type">

<action>Display commit type menu to user:</action>

<output>
📝 **Select Conventional Commit Type:**

1. **feat** - A new feature
2. **fix** - A bug fix
3. **docs** - Documentation only changes
4. **test** - Adding missing tests or correcting existing tests
5. **refactor** - Code change that neither fixes a bug nor adds a feature
6. **chore** - Changes to the build process or auxiliary tools
7. **perf** - Code change that improves performance
8. **ci** - Changes to CI configuration files and scripts
9. **build** - Changes that affect the build system or external dependencies
10. **style** - Changes that do not affect the meaning of the code (formatting, etc.)
    </output>

<ask>Enter commit type number (1-10):</ask>
<action>Capture user's choice and map to commit type string</action>
<action>Validate selection is between 1-10</action>
<check if="invalid selection">
<action>Display error and ask again</action>
</check>

</step>

<step n="4" goal="Generate Commit Subject">

<check if="scope is 'multiple'">
  <ask>Multiple categories affected. Enter custom scope (or press Enter to use 'multiple'):</ask>
  <action>If user provides custom scope, use it; otherwise use 'multiple'</action>
</check>

<ask>Enter commit subject (what changed?):</ask>
<action>Capture user's subject input</action>
<action>Trim whitespace from subject</action>
<action>Ensure subject starts with lowercase (unless proper noun)</action>
<action>Remove trailing period if present</action>
<action>Verify subject is in imperative mood (add, not added; fix, not fixed)</action>

<action>Calculate subject length</action>
<check if="subject length > 50 characters">
<output>⚠️ Warning: Subject is {length} characters (recommended max: 50)

Consider shortening for better git log readability.</output>
</check>

</step>

<step n="5" goal="Generate Commit Messages">

<action>Build primary commit message: `{type}({scope}): {subject}`</action>

<check if="scope is 'multiple' or files span multiple directories">
  <action>Generate alternative suggestions:</action>
  <action>- Alternative 1: Use most prominent directory as scope</action>
  <action>- Alternative 2: Use broader category (e.g., "core" instead of specific module)</action>
  <action>- Alternative 3: Use "project" as scope for cross-cutting changes</action>
</check>

<output>
✅ **Suggested Commit Message:**

```
{primary_message}
```

**Alternative Suggestions:**
{alternatives_if_applicable}

**Files to be committed:**
{list_of_modified_files}
</output>

</step>

<step n="6" goal="Display Commit Command">

<output>
📋 **To commit these changes, run:**

```bash
git add .
git commit -m "{primary_message}"
```

Or stage specific files first:

```bash
git add {specific_files}
git commit -m "{primary_message}"
```

</output>

<ask>Would you like to see alternative commit messages? [y/n]</ask>

<check if="user responds yes">
  <output>
**Alternative Commit Messages:**

Option 1: `{alternative_1}`
Option 2: `{alternative_2}`  
Option 3: `{alternative_3}`

To use an alternative:

```bash
git commit -m "option_text_here"
```

  </output>
</check>

</step>

<step n="7" goal="Display Time Tracking" optional="true">

<ask>Display workflow time tracking summary? [y/n]</ask>

<check if="user responds yes AND {workflow_name} is provided">
  <action>Calculate duration between {start_date} and {finish_date}</action>
  <action>Format duration as human-readable (e.g., "2 hours 15 minutes")</action>
  
  <output>
⏱️  **Workflow Time Tracking:**

Workflow: {workflow_name}
Started: {start_date}
Finished: {finish_date}
Duration: {time_spent}
</output>
</check>

<check if="{workflow_name} is NOT provided">
  <output>
⏱️  Time tracking not available (workflow not tracked).
  
To enable time tracking, invoke this workflow from tracked workflows like dev-story or story-done.
  </output>
</check>

</step>

<step n="8" goal="Persist Time Tracking">

<check if="{workflow_name} is provided AND time tracking enabled">
  <action>Load helper instructions: {installed_path}/log-helper.md</action>
  <action>Follow log-helper instructions to append tracking data to {output_folder}/.bmad/workflow-logs.yaml</action>
  <action>Create .bmad directory if it doesn't exist</action>
  <action>Append new entry with:
  - workflow: {workflow_name}
  - started: {start_date}
  - finished: {finish_date}
  - duration: {time_spent}
  - commit_suggested: {primary_message}
  </action>
  
  <output>✅ Time tracking saved to {output_folder}/.bmad/workflow-logs.yaml</output>
</check>

</step>

<step n="9" goal="Completion">

<output>
🎉 **Commit message generated successfully!**

**Next Steps:**

1. Review the suggested commit message
2. Stage your files (`git add`)
3. Commit with the generated message
4. Push to remote if ready (`git push`)

📚 For more information on Conventional Commits, see:

- {project-root}/docs/conventional-commits-guide.md
- https://www.conventionalcommits.org/
  </output>

</step>

</workflow>
