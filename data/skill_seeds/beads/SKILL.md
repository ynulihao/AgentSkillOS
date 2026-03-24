---
name: beads
description: "Use when tracking complex, multi-session work with dependency graphs and persistent memory that survives conversation compaction. Provides git-backed issue tracking via the bd CLI with priority management (P0-P4), blocker detection, epic workflows, and audit trails. Trigger phrases: 'create task for', 'what's ready to work on', 'show task', 'track this work', 'what's blocking', 'update status'."
allowed-tools: "Read,Bash(bd:*)"
version: "0.34.0"
author: "Steve Yegge <https://github.com/steveyegge>"
license: "MIT"
---

# Beads - Persistent Task Memory for AI Agents

**bd (beads)** replaces markdown task lists with a dependency-aware graph stored in git. Unlike TodoWrite (session-scoped), bd persists across compactions and tracks complex dependencies.

**When to Use bd vs TodoWrite**:
- "Will I need this context in 2 weeks?" / "Could history get compacted?" / "Does this have blockers?" → **bd**
- "Will this be done in this session?" / "Simple checklist for right now?" → **TodoWrite**

## Prerequisites

**Required**:
- **bd CLI**: Version 0.34.0 or later installed and in PATH
- **Git Repository**: Current directory must be a git repo
- **Initialization**: `bd init` must be run once (humans do this, not agents)

**Verify Installation**:
```bash
bd --version  # Should return 0.34.0 or later
```

**First-Time Setup** (humans run once):
```bash
cd /path/to/your/repo
bd init  # Creates .beads/ directory with database
```

**Optional**:
- **BEADS_DIR** environment variable for alternate database location
- **Daemon** for background sync: `bd daemon --start`

## Instructions

### Session Start Protocol

**Every session, start here:**

#### Step 1: Check for Ready Work

```bash
bd ready
```

Shows tasks with no open blockers, sorted by priority (P0 → P4).

**What this shows**:
- Task ID (e.g., `myproject-abc`)
- Title
- Priority level
- Issue type (bug, feature, task, epic)

**Example output**:
```
claude-code-plugins-abc [P1] [task] open
  Implement user authentication

claude-code-plugins-xyz [P0] [epic] in_progress
  Refactor database layer
```

#### Step 2: Pick Highest Priority Task

Choose the highest priority (P0 > P1 > P2 > P3 > P4) task that's ready.

#### Step 3: Get Full Context

```bash
bd show <task-id>
```

Displays:
- Full task description
- Dependency graph (what blocks this, what this blocks)
- Audit trail (all status changes, notes)
- Metadata (created, updated, assignee, labels)

#### Step 4: Start Working

```bash
bd update <task-id> --status in_progress
```

Marks task as actively being worked on.

#### Step 5: Add Notes as You Work

```bash
bd update <task-id> --notes "Completed: X. In progress: Y. Blocked by: Z"
```

**Critical for compaction survival**: Write notes as if explaining to a future agent with zero conversation context.

**Note Format** (best practice):
```
COMPLETED: Specific deliverables (e.g., "implemented JWT refresh endpoint + rate limiting")
IN PROGRESS: Current state + next immediate step
BLOCKERS: What's preventing progress
KEY DECISIONS: Important context or user guidance
```

---

### Task Creation Workflow

#### When to Create Tasks

Create bd tasks when:
- User mentions tracking work across sessions
- User says "we should fix/build/add X"
- Work has dependencies or blockers
- Exploratory/research work with fuzzy boundaries

#### Basic Task Creation

```bash
bd create "Task title" -p 1 --type task
```

**Arguments**:
- **Title**: Brief description (required)
- **Priority**: 0-4 where 0=critical, 1=high, 2=medium, 3=low, 4=backlog (default: 2)
- **Type**: bug, feature, task, epic, chore (default: task)

**Example**:
```bash
bd create "Fix authentication bug" -p 0 --type bug
```

#### Create with Description

```bash
bd create "Implement OAuth" -p 1 --description "Add OAuth2 support for Google, GitHub, Microsoft. Use passport.js library."
```

#### Epic with Children

```bash
# Create parent epic
bd create "Epic: OAuth Implementation" -p 0 --type epic
# Returns: myproject-abc

# Create child tasks
bd create "Research OAuth providers" -p 1 --parent myproject-abc
bd create "Implement auth endpoints" -p 1 --parent myproject-abc
bd create "Add frontend login UI" -p 2 --parent myproject-abc
```

---

### Update & Progress Workflow

#### Change Status

```bash
bd update <task-id> --status <new-status>
```

**Status Values**:
- `open` - Not started
- `in_progress` - Actively working
- `blocked` - Stuck, waiting on something
- `closed` - Completed

**Example**:
```bash
bd update myproject-abc --status blocked
```

#### Add Progress Notes

```bash
bd update <task-id> --notes "Progress update here"
```

**Appends** to existing notes field (doesn't replace).

#### Change Priority

```bash
bd update <task-id> -p 0  # Escalate to critical
```

#### Add Labels

```bash
bd label add <task-id> backend
bd label add <task-id> security
```

Labels provide cross-cutting categorization beyond status/type.

---

### Dependency Management

#### Add Dependencies

```bash
bd dep add <child-id> <parent-id>
```

**Meaning**: `<parent-id>` blocks `<child-id>` (parent must be completed first).

**Dependency Types**:
- **blocks**: Parent must close before child becomes ready
- **parent-child**: Hierarchical relationship (epics and subtasks)
- **discovered-from**: Task A led to discovering task B
- **related**: Tasks are related but not blocking

**Example**:
```bash
# Deployment blocked by tests passing
bd dep add deploy-task test-task  # test-task blocks deploy-task
```

#### View Dependencies

```bash
bd dep list <task-id>
```

Shows:
- What this task blocks (dependents)
- What blocks this task (blockers)

#### Circular Dependency Prevention

bd automatically prevents circular dependencies. If you try to create a cycle, the command fails.

---

### Completion Workflow

#### Close a Task

```bash
bd close <task-id> --reason "Completion summary"
```

**Best Practice**: Always include a reason describing what was accomplished.

**Example**:
```bash
bd close myproject-abc --reason "Completed: OAuth endpoints implemented with Google, GitHub providers. Tests passing."
```

#### Check Newly Unblocked Work

After closing a task, run:

```bash
bd ready
```

Closing a task may unblock dependent tasks, making them newly ready.

#### Close Epics When Children Complete

```bash
bd epic close-eligible
```

Automatically closes epics where all child tasks are closed.

---

### Git Sync Workflow

#### All-in-One Sync

```bash
bd sync
```

**Performs**:
1. Export database to `.beads/issues.jsonl`
2. Commit changes to git
3. Pull from remote (merge if needed)
4. Import updated JSONL back to database
5. Push local commits to remote

**Use when**: End of session, before handing off to teammate, after major progress.

#### Export Only

```bash
bd export -o backup.jsonl
```

Creates JSONL backup without git operations.

#### Import Only

```bash
bd import -i backup.jsonl
```

Imports JSONL file into database.

#### Background Daemon

```bash
bd daemon --start  # Auto-sync in background
bd daemon --status # Check daemon health
bd daemon --stop   # Stop auto-sync
```

Daemon watches for database changes and auto-exports to JSONL.

---

### Find & Search Commands

#### Find Ready Work

```bash
bd ready
```

Shows tasks with no open blockers.

#### List All Tasks

```bash
bd list --status open           # Only open tasks
bd list --priority 0            # Only P0 (critical)
bd list --type bug              # Only bugs
bd list --label backend         # Only labeled "backend"
bd list --assignee alice        # Only assigned to alice
```

#### Show Task Details

```bash
bd show <task-id>
```

Full details: description, dependencies, audit trail, metadata.

#### Search by Text

```bash
bd search "authentication"      # Search titles and descriptions
bd search login --status open   # Combine with filters
```

#### Find Blocked Work

```bash
bd blocked
```

Shows all tasks that have open blockers preventing them from being worked on.

#### Project Statistics

```bash
bd stats
```

Shows:
- Total issues by status (open, in_progress, blocked, closed)
- Issues by priority (P0-P4)
- Issues by type (bug, feature, task, epic, chore)
- Completion rate

---

### Complete Command Reference

| Command | Purpose |
|---------|---------|
| `bd ready` | Find unblocked tasks |
| `bd list [--status/--priority/--type/--label]` | View tasks with filters |
| `bd show <id>` | Get task details and dependency graph |
| `bd search <query>` | Text search across tasks |
| `bd blocked` | Find stuck work |
| `bd stats` | Project metrics and completion rate |
| `bd create "Title" -p N --type T` | Create task (types: bug/feature/task/epic/chore) |
| `bd update <id> --status/--notes/-p` | Change status, add notes, or reprioritize |
| `bd close <id> --reason "..."` | Mark task done |
| `bd dep add <child> <parent>` | Add dependency (parent blocks child) |
| `bd label add <id> <label>` | Tag with labels |
| `bd reopen <id>` | Reopen closed task |
| `bd epic close-eligible` | Auto-close epics with all children done |
| `bd sync` | Git sync (export, commit, pull, import, push) |
| `bd export/import` | JSONL backup/restore |
| `bd daemon --start/--stop` | Background sync |
| `bd delete <id> --force` | Delete issues |
| `bd compact` | Archive old closed tasks |

---

## Output

- **Task IDs**: Format `<prefix>-<hash>` (e.g., `myproject-abc`)
- **Status summaries**: `5 open, 2 in_progress, 1 blocked, 47 closed`
- **Dependency graphs**: Visual tree showing blockers and dependents
- **Audit trails**: Complete history of status changes, notes, and decisions

---

## Error Handling

| Error | Solution |
|-------|----------|
| `bd: command not found` | Install: `npm install -g @beads/bd` or `brew install steveyegge/beads/bd` |
| `No .beads database found` | Run `bd init` (humans do this once, not agents) |
| `Task not found: <id>` | Use `bd list` or `bd search <query>` to find correct ID |
| `Circular dependency detected` | Restructure graph; use `bd dep list <id>` to review |
| JSONL merge conflicts | `bd sync --merge` for auto-resolution |
| `Database is locked` | `bd daemon --stop && bd daemon --start` |
| Sync failures | Check `git fetch` connectivity and credentials |

---

## Examples

### Example 1: Multi-Session Feature (Epic with Children)

**User Request**: "We need to implement OAuth, this will take multiple sessions"

**Agent Response**:
```bash
# Create epic
bd create "Epic: OAuth Implementation" -p 0 --type epic
# Returns: claude-code-plugins-abc

# Create child tasks
bd create "Research OAuth providers (Google, GitHub, Microsoft)" -p 1 --parent claude-code-plugins-abc
# Returns: claude-code-plugins-abc.1

bd create "Implement backend auth endpoints" -p 1 --parent claude-code-plugins-abc
# Returns: claude-code-plugins-abc.2

bd create "Add frontend login UI components" -p 2 --parent claude-code-plugins-abc
# Returns: claude-code-plugins-abc.3

# Add dependencies (backend must complete before frontend)
bd dep add claude-code-plugins-abc.3 claude-code-plugins-abc.2

# Start with research
bd update claude-code-plugins-abc.1 --status in_progress
```

**Result**: Work structured, ready to resume after compaction.

---

### Example 2: Tracking Blocked Work

```bash
bd update task-xyz --status blocked --notes "API /auth returns 503"
bd create "Fix /auth endpoint 503 error" -p 0 --type bug  # Returns: blocker-id
bd dep add task-xyz blocker-id  # Link dependency
bd ready  # Find other unblocked work
```

### Example 3: Session Resume After Compaction

```bash
# Session 1: Add detailed notes before compaction
bd update myproject-auth --notes "COMPLETED: JWT integrated. IN PROGRESS: Token refresh. NEXT: Rate limiting"
# [Conversation compacted]

# Session 2: Full context preserved
bd ready && bd show myproject-auth  # Notes survive compaction
```

---

## Resources

**Database**: Uses `.beads/` by default. Set `BEADS_DIR=/path/to/alternate/beads` for alternate location.

**Team sync**: Use `bd sync` at session start/end to coordinate through git.

### Reference Documentation

- `{baseDir}/references/CLI_REFERENCE.md` - Complete command syntax
- `{baseDir}/references/WORKFLOWS.md` - Detailed workflow patterns
- `{baseDir}/references/DEPENDENCIES.md` - Dependency system deep dive
- `{baseDir}/references/RESUMABILITY.md` - Compaction survival guide
- `{baseDir}/references/ADVANCED_WORKFLOWS.md` - Compaction, epics, templates
- `{baseDir}/references/GIT_INTEGRATION.md` - Merge conflicts, daemon, branching
- `{baseDir}/references/TEAM_COLLABORATION.md` - Multi-user, worktrees, prefixes
- `{baseDir}/references/BOUNDARIES.md` - bd vs TodoWrite comparison

Full documentation: https://github.com/steveyegge/beads
