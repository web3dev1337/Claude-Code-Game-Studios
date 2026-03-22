# Security Analysis: claude-game-studios Hooks, Rules, and Scripts

**Analyst:** Claude Opus 4.6 (1M context)
**Date:** 2026-03-22
**Scope:** All shell scripts (8 hooks + 1 statusline), 11 rule files, 6 hook reference docs, 1 settings.json

---

## Executive Summary

**Overall Verdict: SAFE**

All 9 shell scripts are clean. Zero network calls (no curl, wget, nc, ssh, scp, or any outbound communication). Zero credential harvesting. Zero obfuscated code. Zero eval/exec of remote content. Zero data exfiltration vectors. Every script operates exclusively on local project files using standard git and filesystem commands.

The 11 rule files are straightforward coding standards with no hidden instructions or prompt injection attempts.

The settings.json configures sensible permissions (deny list blocks destructive commands and .env access).

---

## Shell Script Analysis

### 1. `.claude/hooks/session-start.sh`

**Event:** SessionStart (no stdin)
**Purpose:** Displays project context when a Claude session begins -- current branch, recent commits, active sprint, bug count, TODO/FIXME counts, and active session state recovery.
**Operations:**
- `git rev-parse --abbrev-ref HEAD` -- get current branch
- `git log --oneline -5` -- recent commits
- `ls -t production/sprints/sprint-*.md` -- find latest sprint file
- `ls -t production/milestones/*.md` -- find latest milestone
- `find ... -name "BUG-*.md"` -- count open bugs
- `grep -r "TODO" src/` / `grep -r "FIXME" src/` -- code health check
- `head -20 production/session-state/active.md` -- preview session state
- `wc -l` -- line count

**Network calls:** NONE
**Credential access:** NONE
**File exfiltration:** NONE
**Obfuscated code:** NONE
**Eval/exec:** NONE
**Verdict: SAFE** -- Pure read-only local operations for context display.

---

### 2. `.claude/hooks/detect-gaps.sh`

**Event:** SessionStart (no stdin)
**Purpose:** Detects missing documentation when code/prototypes exist. Checks for fresh projects, sparse design docs, undocumented prototypes, missing architecture docs, gameplay systems without design docs, and missing production planning.
**Operations:**
- `grep` on local markdown files for section detection
- `find src -type f` -- count source files by extension
- `find design/gdd -type f -name "*.md"` -- count design docs
- `find prototypes -mindepth 1 -maxdepth 1 -type d` -- list prototype directories
- `find docs/architecture -type f -name "*.md"` -- count ADRs
- File existence checks (`[ -f ... ]`, `[ -d ... ]`)

**Network calls:** NONE
**Credential access:** NONE
**File exfiltration:** NONE
**Obfuscated code:** NONE
**Eval/exec:** NONE
**Verdict: SAFE** -- Pure read-only filesystem analysis. Outputs advisory messages only.

---

### 3. `.claude/hooks/log-agent.sh`

**Event:** SubagentStart (receives JSON on stdin)
**Purpose:** Logs agent invocations to a local audit trail file.
**Operations:**
- `cat` to read stdin JSON
- `jq` or `grep` to parse agent_name from JSON
- `date` for timestamp
- `mkdir -p production/session-logs` -- create log directory
- `echo >> production/session-logs/agent-audit.log` -- append log entry

**Network calls:** NONE
**Credential access:** NONE -- only reads `agent_name` from the hook input JSON
**File exfiltration:** NONE -- writes only to local `production/session-logs/`
**Obfuscated code:** NONE
**Eval/exec:** NONE
**Verdict: SAFE** -- Simple local audit logging.

---

### 4. `.claude/hooks/validate-push.sh`

**Event:** PreToolUse (Bash matcher, receives JSON on stdin)
**Purpose:** Warns when pushing to protected branches (develop, main, master). Does NOT block -- only prints warnings to stderr.
**Operations:**
- `cat` to read stdin JSON
- `jq` or `grep` to extract command from JSON
- `grep -qE '^git[[:space:]]+push'` -- detect push commands
- `git rev-parse --abbrev-ref HEAD` -- get current branch
- String comparison against protected branch names
- Outputs warning to stderr

**Network calls:** NONE
**Credential access:** NONE
**File exfiltration:** NONE
**Obfuscated code:** NONE
**Eval/exec:** NONE
**Verdict: SAFE** -- Read-only branch check. Non-blocking (exit 0 always). The blocking exit 2 is commented out.

---

### 5. `.claude/hooks/session-stop.sh`

**Event:** Stop (no stdin)
**Purpose:** Logs session summary when Claude finishes. Archives active session state and records recent commits/modified files.
**Operations:**
- `date` for timestamp
- `mkdir -p production/session-logs` -- create log directory
- `git log --oneline --since="8 hours ago"` -- recent commits
- `git diff --name-only` -- modified files
- `cat production/session-state/active.md` -- read session state
- Appends to `production/session-logs/session-log.md`
- `rm production/session-state/active.md` -- clean up active state

**Network calls:** NONE
**Credential access:** NONE
**File exfiltration:** NONE -- writes only to local `production/session-logs/`
**Obfuscated code:** NONE
**Eval/exec:** NONE
**Verdict: SAFE** -- Local cleanup and logging only. Deletes only the project's own session state file.

---

### 6. `.claude/hooks/validate-commit.sh`

**Event:** PreToolUse (Bash matcher, receives JSON on stdin)
**Purpose:** Validates git commit commands by checking staged files for: design doc required sections, valid JSON in data files, hardcoded gameplay values, and TODO/FIXME without owner tags.
**Operations:**
- `cat` to read stdin JSON
- `jq` or `grep` to extract command
- `git diff --cached --name-only` -- list staged files
- `grep -qi` on design docs for required sections
- `python -m json.tool` -- validate JSON files
- `grep -nE` for hardcoded values and TODO patterns

**Network calls:** NONE
**Credential access:** NONE
**File exfiltration:** NONE
**Obfuscated code:** NONE
**Eval/exec:** Runs `python -m json.tool` on staged JSON files -- this is safe; `json.tool` is a stdlib JSON validator that only reads the file and checks syntax. It does NOT execute the JSON content.
**Blocking behavior:** Only blocks (exit 2) for invalid JSON. All other checks are warnings only (exit 0).
**Verdict: SAFE** -- Standard pre-commit validation. The python invocation is limited to JSON validation via stdlib.

---

### 7. `.claude/hooks/validate-assets.sh`

**Event:** PostToolUse (Write|Edit matcher, receives JSON on stdin)
**Purpose:** Checks asset naming conventions and JSON validity after files are written/edited in the `assets/` directory.
**Operations:**
- `cat` to read stdin JSON
- `jq` or `grep` to extract file_path
- `sed` to normalize path separators
- `grep -qE '[A-Z[:space:]-]'` -- check naming convention
- `python -m json.tool` -- validate JSON

**Network calls:** NONE
**Credential access:** NONE
**File exfiltration:** NONE
**Obfuscated code:** NONE
**Eval/exec:** Same safe `python -m json.tool` invocation as validate-commit.sh.
**Note:** PostToolUse hooks cannot block (exit code is ignored for blocking), so this is purely advisory.
**Verdict: SAFE** -- Non-blocking asset validation.

---

### 8. `.claude/hooks/pre-compact.sh`

**Event:** PreCompact (no stdin)
**Purpose:** Dumps session state before context window compression so critical state survives summarization.
**Operations:**
- `date` for timestamp
- `cat production/session-state/active.md` or `head -n 100` -- read session state (truncated to 100 lines)
- `git diff --name-only` -- unstaged changes
- `git diff --staged --name-only` -- staged changes
- `git ls-files --others --exclude-standard` -- untracked files
- `grep -n -E "TODO|WIP|PLACEHOLDER"` on design docs
- `mkdir -p production/session-logs`
- `echo >> production/session-logs/compaction-log.txt` -- log compaction event

**Network calls:** NONE
**Credential access:** NONE
**File exfiltration:** NONE
**Obfuscated code:** NONE
**Eval/exec:** NONE
**Verdict: SAFE** -- State preservation for context management. All local.

---

### 9. `.claude/statusline.sh`

**Event:** Called by settings.json statusLine configuration
**Purpose:** Generates a single-line status bar showing: context window usage %, model name, production stage, and optional Epic/Feature/Task breadcrumb.
**Operations:**
- `cat` to read stdin JSON (from Claude Code runtime)
- `jq` or `grep` to parse model name, context usage, cwd
- `sed` for path normalization
- `head -1 production/stage.txt` -- read production stage
- File existence checks for concept, systems index, tech preferences
- `grep` for engine configuration detection
- `find src -type f` -- count source files
- Reads `production/session-state/active.md` for STATUS block parsing
- `printf` to output status line

**Network calls:** NONE
**Credential access:** NONE
**File exfiltration:** NONE
**Obfuscated code:** NONE
**Eval/exec:** NONE
**Verdict: SAFE** -- Pure read-only status generation.

---

## settings.json Analysis

**File:** `.claude/settings.json`

**Permissions - Allow list:** Grants auto-approval for safe read-only git commands, `ls`, `dir`, `python -m json.tool`, and `python -m pytest`. All are safe.

**Permissions - Deny list:** Blocks destructive operations:
- `rm -rf *` -- prevents recursive deletion
- `git push --force*` / `git push -f *` -- prevents force pushes
- `git reset --hard*` -- prevents hard resets
- `git clean -f*` -- prevents clean operations
- `sudo *` -- prevents privilege escalation
- `chmod 777*` -- prevents insecure permissions
- `*>.env*` / `cat *.env*` / `type *.env*` / `Read(**/.env*)` -- prevents .env file access/exfiltration

**Hook configuration:** All hooks use relative paths (`.claude/hooks/*.sh`), have reasonable timeouts (5-15 seconds), and match expected Claude Code hook events.

**Verdict: SAFE** -- Well-designed security configuration. The deny list actively prevents common dangerous operations and credential exposure.

---

## Rule Files Analysis

### Summary Table

| File | Scope (paths glob) | Purpose | Prompt Injection | Verdict |
|------|-------------------|---------|-----------------|---------|
| `ai-code.md` | `src/ai/**` | AI system coding standards (2ms budget, data-driven params, debuggability) | NONE | SAFE |
| `data-files.md` | `assets/data/**` | JSON data file conventions (naming, schema, validation) | NONE | SAFE |
| `design-docs.md` | `design/gdd/**` | Design document required sections (8 mandatory sections) | NONE | SAFE |
| `engine-code.md` | `src/core/**` | Engine code standards (zero-alloc hot paths, thread safety, RAII) | NONE | SAFE |
| `gameplay-code.md` | `src/gameplay/**` | Gameplay code standards (data-driven values, delta time, no UI coupling) | NONE | SAFE |
| `narrative.md` | `design/narrative/**` | Narrative/lore consistency rules (canon levels, voice profiles, localization) | NONE | SAFE |
| `network-code.md` | `src/networking/**` | Network code standards (authoritative server, versioned messages, security) | NONE | SAFE |
| `prototype-code.md` | `prototypes/**` | Relaxed prototype standards (hardcoded values OK, but README required) | NONE | SAFE |
| `shader-code.md` | `assets/shaders/**` | Shader coding standards (naming, performance, cross-platform) | NONE | SAFE |
| `test-standards.md` | `tests/**` | Test naming and structure standards (arrange/act/assert) | NONE | SAFE |
| `ui-code.md` | `src/ui/**` | UI code rules (no game state mutation, localization, accessibility) | NONE | SAFE |

All 11 rule files:
- Use standard YAML frontmatter with `paths` globs for scoping
- Contain practical, domain-specific coding standards for game development
- Include clear examples where appropriate (correct vs incorrect patterns)
- Contain NO hidden instructions, NO prompt injection attempts, NO obfuscated content
- Are entirely declarative guidelines -- they do not execute anything

**Verdict: ALL SAFE** -- Straightforward coding standards documentation.

---

## Hook Reference Docs Analysis

The 6 files in `.claude/docs/hooks-reference/` are documentation-only markdown files:

| File | Content | Verdict |
|------|---------|---------|
| `hook-input-schemas.md` | Documents JSON payload schemas for each hook event type | SAFE - documentation only |
| `post-merge-asset-validation.md` | Reference implementation for post-merge asset checks | SAFE - documentation only (code is in a markdown code block, not executed) |
| `post-sprint-retrospective.md` | Describes sprint retrospective workflow (not a git hook) | SAFE - documentation only |
| `pre-commit-code-quality.md` | Reference implementation for code quality pre-commit hook | SAFE - documentation only |
| `pre-commit-design-check.md` | Reference implementation for design doc validation | SAFE - documentation only |
| `pre-push-test-gate.md` | Reference implementation for pre-push test gate | SAFE - documentation only |

**Important note:** The reference docs contain example shell scripts inside markdown code blocks. These are NOT executed -- they serve as implementation guides. The actual executed hooks are the `.sh` files in `.claude/hooks/`, which are analyzed above.

---

## Comprehensive Threat Checklist

| Threat Vector | Found? | Details |
|--------------|--------|---------|
| Network calls (curl, wget, nc, ssh, scp, rsync) | NO | Zero outbound network calls in any script |
| Data exfiltration | NO | All writes go to local `production/session-logs/` only |
| Environment variable harvesting | NO | No `env`, `printenv`, `$HOME/.ssh`, `$HOME/.aws` access |
| Credential access (.env, API keys, tokens) | NO | settings.json actively DENIES .env access |
| Obfuscated code (base64, hex, eval of encoded strings) | NO | All code is plain, readable bash |
| eval/exec of remote content | NO | No eval, no exec, no source of remote URLs |
| Arbitrary command execution from user input | NO | Commands parsed from JSON are only pattern-matched, never executed |
| Privilege escalation (sudo, chmod) | NO | settings.json denies sudo and chmod 777 |
| Destructive file operations | NO | Only `rm` is on `production/session-state/active.md` (own state file) |
| Hidden prompt injection in rules | NO | All 11 rules contain only domain-appropriate coding standards |
| Suspicious process spawning | NO | Only `python -m json.tool` (stdlib JSON validator) |
| Timing attacks or race conditions | NO | All hooks have short timeouts (5-15s) |
| Symlink attacks | NO | No symlink creation or following |

---

## Final Verdict

**ALL 9 SHELL SCRIPTS: SAFE**
**ALL 11 RULE FILES: SAFE**
**ALL 6 HOOK REFERENCE DOCS: SAFE**
**SETTINGS.JSON: SAFE (with good security deny list)**

This is a well-designed, security-conscious Claude Code configuration for game development. The hooks provide useful development workflow automation (documentation gap detection, commit validation, session logging, context preservation) without any security concerns. The settings.json deny list adds an extra layer of protection against destructive operations and credential exposure.
