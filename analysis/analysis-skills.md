# Comprehensive Skill Analysis: Claude Game Studios

**Analyzed**: 37 skills in `/tmp/claude-game-studios/.claude/skills/`
**Date**: 2026-03-22

---

## Individual Skill Assessments

### 1. architecture-decision
- **Purpose**: Creates Architecture Decision Records (ADRs) documenting technical decisions
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write` | `argument-hint: "[title]"`
- **Content**: Well-structured ADR template covering status, context, constraints, alternatives, consequences, performance implications, migration plan, and validation criteria
- **Game dev usefulness**: HIGH. ADRs are essential for any non-trivial project. Works for game dev specifically (performance implications section) and generally
- **Quality**: 8/10. Thorough template, clear workflow. Auto-numbering by scanning existing ADRs is a nice touch

### 2. asset-audit
- **Purpose**: Audits game assets for naming conventions, file sizes, format compliance, orphaned/missing assets
- **Frontmatter**: `allowed-tools: Read, Glob, Grep` | `argument-hint: "[category|all]"`
- **Content**: Checks naming conventions (art: `[category]_[name]_[variant]_[size].[ext]`), file standards (POT textures, audio formats), orphaned assets, missing asset references
- **Game dev usefulness**: VERY HIGH. Asset management is one of the most painful parts of game development. Finding orphaned assets alone saves hours
- **Quality**: 8/10. Specific naming patterns, concrete format checks, actionable output format

### 3. balance-check
- **Purpose**: Analyzes game balance data for outliers, broken progressions, degenerate strategies, economy imbalances
- **Frontmatter**: `allowed-tools: Read, Glob, Grep` | `argument-hint: "[system-name|path-to-data-file]"`
- **Content**: Domain-specific analysis for combat (DPS, TTK, dominant strategies), economy (faucets/sinks, infinite loops), progression (XP curves, dead zones), loot (pity timers, acquisition rates)
- **Game dev usefulness**: VERY HIGH. Game-specific and deeply relevant. Balance analysis is a core game design activity that is tedious to do manually
- **Quality**: 9/10. Shows genuine game design knowledge. The analysis categories (combat, economy, progression, loot) cover the main balance domains. Mentions degenerate strategies and pity timer math

### 4. brainstorm
- **Purpose**: Guided game concept ideation from zero to structured game concept document
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, WebSearch, AskUserQuestion` | `argument-hint: "[genre or theme hint, or 'open']"`
- **Content**: 6-phase process: Creative Discovery, Concept Generation, Core Loop Design, Pillars and Boundaries, Player Type Validation, Scope and Feasibility. Uses professional frameworks (MDA, Bartle, Quantic Foundry, Self-Determination Theory)
- **Game dev usefulness**: VERY HIGH. This is the kind of structured creative facilitation that game studios actually use. The verb-first design, mashup method, and experience-first design are real techniques
- **Quality**: 10/10. Best skill in the collection. Demonstrates deep knowledge of game design theory. The 30-second/5-minute/session/progression loop decomposition is exactly how professional game designers think. Mentions specific frameworks by name and applies them correctly. The anti-pillars concept is particularly valuable. Excellent collaborative protocol with clear handoff to downstream skills

### 5. bug-report
- **Purpose**: Creates structured bug reports or analyzes code for potential bugs
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write` | `argument-hint: "[description] or analyze [path]"`
- **Content**: Full bug report template (severity S1-S4, priority P1-P4, reproduction steps, technical context) plus code analysis mode for detecting common bugs
- **Game dev usefulness**: MEDIUM-HIGH. Bug reporting is universal, not game-specific. But the template is well-suited to game QA workflows (scene/level, game state fields)
- **Quality**: 7/10. Solid template but fairly standard. The dual-mode (report vs analyze) is a good design choice

### 6. changelog
- **Purpose**: Auto-generates changelogs from git commits, sprint data, and design docs. Produces internal and player-facing versions
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Bash` | Uses `context:` for git log and tags
- **Content**: Dual output: internal (technical, with commit hashes and owners) and player-facing (friendly, no jargon). Good categorization (features, improvements, bug fixes, balance changes, known issues)
- **Game dev usefulness**: HIGH. The dual internal/player-facing changelog is specifically valuable for games where community communication matters
- **Quality**: 8/10. Smart use of `context:` frontmatter for git data. Clear guidelines about not exposing internal references in player-facing notes

### 7. code-review
- **Purpose**: Architectural and quality code review for game codebases
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Bash` | `argument-hint: "[path]"`
- **Content**: Checks coding standards (cyclomatic complexity, method length, DI), architectural compliance (dependency direction, layer separation), SOLID principles, and game-specific issues (frame-rate independence, delta time, hot path allocations, resource cleanup)
- **Game dev usefulness**: HIGH. The game-specific concerns section (delta time, no allocations in hot paths, resource cleanup) distinguishes this from a generic code review
- **Quality**: 8/10. The game-specific checklist items show real game programming knowledge

### 8. design-review
- **Purpose**: Reviews game design documents for completeness, consistency, implementability
- **Frontmatter**: `allowed-tools: Read, Glob, Grep` | `argument-hint: "[path-to-design-doc]"`
- **Content**: 8-section completeness check (Overview, Player Fantasy, Detailed Rules, Formulas, Edge Cases, Dependencies, Tuning Knobs, Acceptance Criteria). Cross-system consistency verification. Implementability assessment. Contextual next-step recommendations based on document type
- **Game dev usefulness**: VERY HIGH. Enforcing GDD quality standards is critical and often overlooked. The "is it precise enough for a programmer to implement without guessing" check is excellent
- **Quality**: 9/10. The contextual next-step logic (different recommendations for concept docs vs system GDDs) shows sophisticated workflow integration

### 9. design-system
- **Purpose**: Guided, section-by-section GDD authoring for individual game systems
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Task, AskUserQuestion, TodoWrite` | `argument-hint: "<system-name>"`
- **Content**: Massive skill (497 lines). 7 phases: Parse/Validate, Gather Context, Create Skeleton, Section-by-Section Design, Post-Design Validation, Specialist Agent Routing, Recovery/Resume. Each of the 8 GDD sections has specific guidance, cross-reference instructions, and specialist agent delegation
- **Game dev usefulness**: VERY HIGH. This is the core GDD authoring workflow. The incremental write-to-file approach (surviving session interruptions) is brilliant for long design sessions
- **Quality**: 10/10. Exceptional. The dependency-aware context gathering, specialist agent routing table, cross-referencing protocol, and session recovery mechanism make this a production-grade workflow tool. The "Question -> Options -> Decision -> Draft -> Approval -> Write" cycle per section is exactly right for collaborative design

### 10. estimate
- **Purpose**: Task effort estimation with complexity analysis, risk factors, and confidence levels
- **Frontmatter**: `allowed-tools: Read, Glob, Grep` | `argument-hint: "[task-description]"`
- **Content**: Multi-factor analysis (code complexity, scope, risk), three-point estimation (optimistic/expected/pessimistic), historical comparison, sub-task breakdown
- **Game dev usefulness**: MEDIUM. Estimation is universal, not game-specific. But it reads design docs and sprint data for context
- **Quality**: 7/10. Solid estimation framework. The guidelines section is good (never single-point estimates, round to half-days, break tasks over 10 days)

### 11. gate-check
- **Purpose**: Phase gate validation for advancing between development stages
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Bash, Write` | `argument-hint: "[target-phase]"`
- **Content**: 7 production stages (Concept through Release) with specific artifact and quality checks for each gate transition. Writes `production/stage.txt` on pass. Comprehensive checklists for each gate
- **Game dev usefulness**: VERY HIGH. Phase gates are standard practice in game studios. The specific artifact requirements at each gate (e.g., "at least 1 prototype in `prototypes/` with a README" for Pre-Production -> Production) enforce discipline
- **Quality**: 9/10. Excellent integration with other skills (recommends specific skills for missing artifacts). The distinction from `/project-stage-detect` is clearly stated

### 12. hotfix
- **Purpose**: Emergency fix workflow with audit trail, approval collection, and rollback planning
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Bash` | `argument-hint: "[bug-id or description]"` | Explicit invocation only
- **Content**: Severity triage (S1/S2 only), hotfix record creation, branch management, minimal change philosophy, approval workflow via subagents (lead-programmer, qa-tester, producer), rollback plan requirement
- **Game dev usefulness**: HIGH. Live games need disciplined hotfix processes. The 4-hour escalation rule is practical
- **Quality**: 8/10. Good process guardrails. The "minimum change only" rule and mandatory rollback plan are mature engineering practices

### 13. launch-checklist
- **Purpose**: Complete launch readiness validation across all departments
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write` | `argument-hint: "[launch-date or 'dry-run']"` | Explicit invocation only
- **Content**: 7 sections covering Code, Content, QA, Store/Distribution, Infrastructure, Community/Marketing, and Operations. Go/no-go decision framework with sign-offs
- **Game dev usefulness**: VERY HIGH. This is a comprehensive game launch checklist that covers things studios actually forget (on-call schedule, rollback plan, DDoS protection, soak testing)
- **Quality**: 9/10. Extremely thorough. Covers areas that many checklists miss (GDPR, age ratings, community moderation readiness, war room setup). The "dry-run" mode is smart

### 14. localize
- **Purpose**: Localization workflow: scan for issues, extract strings, validate translations, report status
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Bash` | `argument-hint: "[scan|extract|validate|status]"`
- **Content**: 4 subcommands covering the full localization pipeline. Scans for anti-patterns (hardcoded strings, positional placeholders, LTR assumptions, text in images). Coverage matrix generation
- **Game dev usefulness**: HIGH. Localization is essential for commercial games and often done poorly. Catching RTL issues early is particularly valuable
- **Quality**: 8/10. Practical subcommands. The anti-pattern detection (date formatting, number formatting, text direction) goes beyond basic string extraction

### 15. map-systems
- **Purpose**: Decompose a game concept into systems, map dependencies, prioritize design order, create systems index
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, AskUserQuestion, TodoWrite` | `argument-hint: "[optional: 'next' or system-name]"`
- **Content**: 7 phases: Read Concept, Systems Enumeration (explicit + implicit), Dependency Mapping (with cycle detection), Priority Assignment, Create Systems Index, Design Individual Systems (handoff to /design-system), Next Steps
- **Game dev usefulness**: VERY HIGH. Systems decomposition is a critical pre-production activity. The implicit system inference (e.g., "Inventory" implies item database, equipment slots, weight rules, serialization) is extremely valuable
- **Quality**: 10/10. The dependency layer model (Foundation -> Core -> Feature -> Presentation -> Polish) is well-reasoned. Circular dependency detection, bottleneck identification, and clean handoff to `/design-system` show a cohesive design pipeline

### 16. milestone-review
- **Purpose**: Comprehensive milestone progress review with go/no-go recommendation
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write` | `argument-hint: "[milestone-name|current]"`
- **Content**: Feature completeness tracking, quality metrics, code health, risk assessment, velocity analysis, scope recommendations (protect/at-risk/cut candidates)
- **Game dev usefulness**: HIGH. Standard production management tool, well-adapted for game development
- **Quality**: 7/10. Solid but somewhat generic project management. The scope recommendation categories (protect/at-risk/cut candidates) are practical

### 17. onboard
- **Purpose**: Generates contextual onboarding documents for new contributors or agents
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write` | `argument-hint: "[role|area]"`
- **Content**: Role-specific onboarding covering project summary, architecture, standards, current state, sprint context, dependencies, pitfalls, first tasks
- **Game dev usefulness**: MEDIUM. Useful but simple. In the context of AI agents joining a project, this is more relevant than for human onboarding
- **Quality**: 6/10. Good template but relatively thin compared to other skills. Could be more specific about game development roles

### 18. patch-notes
- **Purpose**: Generate player-facing patch notes from git history and sprint data
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Bash` | `argument-hint: "[version] [--style brief|detailed|full]"`
- **Content**: 3 style levels (brief, detailed, full with developer commentary). Developer-to-player language translation examples. Categorization into player-facing groups
- **Game dev usefulness**: HIGH. Player-facing patch notes are a specific game industry need. The developer-to-player translation examples are practical
- **Quality**: 7/10. The three style tiers are a nice touch. Overlaps significantly with `changelog` (which also generates player-facing notes)

### 19. perf-profile
- **Purpose**: Structured performance profiling workflow with optimization recommendations
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Bash` | `argument-hint: "[system-name or 'full']"`
- **Content**: Budget-based profiling (CPU, memory, rendering, I/O). Code analysis for common game performance issues (hot path allocations, unoptimized physics queries, draw calls, overdraw, missing LODs)
- **Game dev usefulness**: VERY HIGH. Performance is make-or-break for games. The game-specific targets (draw calls, overdraw, particle systems, LODs) distinguish this from generic profiling
- **Quality**: 8/10. Practical distinction between static analysis (this skill) and runtime profiling. The "quick wins" category is useful for prioritization

### 20. playtest-report
- **Purpose**: Structured playtest report template and analysis
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write` | `argument-hint: "[new|analyze path]"`
- **Content**: Comprehensive playtest template covering session info, first impressions, gameplay flow (worked well, pain points, confusion, delight), bugs, feature-specific feedback, quantitative data, overall assessment
- **Game dev usefulness**: VERY HIGH. Playtesting is the most important validation activity in game development. Structured collection prevents the "it felt weird" non-feedback problem
- **Quality**: 8/10. The "First 5 minutes" section specifically is excellent -- first impressions are disproportionately important. The quantitative data section (deaths, time per area, features discovered vs missed) adds rigor

### 21. project-stage-detect
- **Purpose**: Auto-detect project development stage and identify gaps
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Bash` | `argument-hint: "[optional: role filter]"`
- **Content**: Scans design docs, source code, production artifacts, prototypes, architecture docs, and tests. 7-stage classification. Role-filtered recommendations. Checks `production/stage.txt` as explicit override
- **Game dev usefulness**: HIGH. "Where are we?" is a common question. The role-based filtering (programmer vs designer vs producer) is practical
- **Quality**: 8/10. Good heuristics for stage detection. The collaborative gap identification (asking questions rather than just listing missing files) is well-designed

### 22. prototype
- **Purpose**: Rapid prototyping workflow with structured report (PROCEED/PIVOT/KILL recommendation)
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Bash` | `argument-hint: "[concept-description]"`
- **Content**: Isolation protocol (prototype code never imports from production, never refactored into production). Hypothesis -> Approach -> Result -> Recommendation workflow. Mandatory "PROTOTYPE - NOT FOR PRODUCTION" headers
- **Game dev usefulness**: VERY HIGH. Prototyping is essential for game dev and the isolation rules prevent a common anti-pattern (prototype code leaking into production)
- **Quality**: 9/10. The PROCEED/PIVOT/KILL recommendation framework with evidence is excellent. The time-box guidance and "if scope grows, stop and reassess" rule show production maturity

### 23. release-checklist
- **Purpose**: Pre-release validation checklist with platform-specific sections
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write` | `argument-hint: "[platform: pc|console|mobile|all]"` | Explicit invocation only
- **Content**: Build verification, quality gates, content completeness, platform-specific requirements (PC: Steam Deck, controller support; Console: TRC/TCR/Lotcheck; Mobile: app store guidelines), store metadata, launch readiness
- **Game dev usefulness**: HIGH. But overlaps significantly with `launch-checklist`
- **Quality**: 7/10. Good platform-specific sections. However, heavy overlap with `launch-checklist` is a design issue

### 24. retrospective
- **Purpose**: Sprint or milestone retrospective with velocity analysis and action items
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write` | Uses `context:` for git log
- **Content**: Velocity trends, estimation accuracy analysis, carryover tracking, technical debt trending, previous action items follow-up, process improvements
- **Game dev usefulness**: MEDIUM-HIGH. Retrospectives are standard agile practice, not game-specific. But well-adapted for game dev workflows
- **Quality**: 8/10. The estimation accuracy analysis (which types of tasks are consistently mis-estimated) and carryover tracking are advanced features

### 25. reverse-document
- **Purpose**: Generate design or architecture docs from existing code/prototypes
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Bash` | `argument-hint: "<type> <path>"`
- **Content**: 3 modes (design, architecture, concept). Collaborative protocol: analyze code, ask about intent, present findings, draft with approval. Metadata marking for reverse-documented files. Detailed example session
- **Game dev usefulness**: VERY HIGH. "Code first, document later" is the reality of most game projects. The intent-questioning protocol prevents documenting accidents as design decisions
- **Quality**: 9/10. The example session walkthrough is excellent. The "never assume intent" principle and the metadata marking (`status: reverse-documented`) are mature design choices

### 26. scope-check
- **Purpose**: Detect scope creep by comparing current scope against original plan
- **Frontmatter**: `allowed-tools: Read, Glob, Grep` | Uses `context:` for git diff stats
- **Content**: Original vs current scope comparison, bloat scoring (percentage-based), risk assessment (schedule, quality, integration), cut/defer/keep/flag recommendations
- **Game dev usefulness**: HIGH. Scope creep is one of the top reasons games ship late or fail. Quantifying it is critical
- **Quality**: 8/10. The quantified bloat score with clear thresholds (10%/25%/50%) is actionable. The four-bucket recommendation (cut/defer/keep/flag) is practical

### 27. setup-engine
- **Purpose**: Configure game engine and version, detect LLM knowledge gaps, populate engine reference docs
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, WebSearch, WebFetch, Task` | `argument-hint: "[engine version] or no args"`
- **Content**: Guided engine selection with comparison matrix (Godot/Unity/Unreal). Version pinning in CLAUDE.md. Knowledge gap detection (version vs LLM training cutoff). Engine-specific naming conventions. WebSearch for versions beyond training data. Refresh subcommand
- **Game dev usefulness**: VERY HIGH. The knowledge gap detection and reference doc population for versions beyond the LLM's training data is genuinely innovative. This solves a real problem with AI-assisted game development
- **Quality**: 10/10. Excellent. The knowledge gap analysis is unique and directly addresses a limitation of LLM-assisted development. Engine-specific naming conventions are correct and detailed. The refresh subcommand for keeping docs current is forward-thinking

### 28. sprint-plan
- **Purpose**: Sprint planning with capacity management, priority tiers, and risk tracking
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit` | Uses `context:` for listing sprint dirs | `argument-hint: "[new|update|status]"`
- **Content**: Must Have / Should Have / Nice to Have priority tiers, 20% buffer for unplanned work, carryover tracking, risk register integration, sprint status reporting
- **Game dev usefulness**: MEDIUM-HIGH. Standard sprint planning, not game-specific. But integrates with game dev artifacts (design docs, milestones)
- **Quality**: 7/10. Solid but standard agile sprint planning. The 20% buffer is a good default

### 29. start
- **Purpose**: First-time onboarding that routes users to the right workflow based on their current state
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, AskUserQuestion` | `argument-hint: "[no arguments]"`
- **Content**: Silent project state detection, 4-option routing (no idea / vague idea / clear concept / existing work), recommended workflow paths for each option, edge case handling (mismatched selection vs actual state)
- **Game dev usefulness**: HIGH. Critical entry point that prevents new users from being overwhelmed by 37 skills
- **Quality**: 9/10. Excellent UX design. The silent detection + question-first approach avoids overwhelming users. The edge case handling (user picks "no idea" but project has code) shows attention to real usage patterns

### 30. team-audio
- **Purpose**: Orchestrate audio team through audio pipeline (direction -> sound design -> technical -> code integration)
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Bash, Task, AskUserQuestion, TodoWrite` | `argument-hint: "[feature or area]"`
- **Content**: 4-step pipeline: Audio Direction (sonic identity, emotional tone), Sound Design (SFX specs, audio events, mixing groups), Technical Implementation (middleware, bus structure, memory budgets), Code Integration (audio manager, adaptive music)
- **Game dev usefulness**: HIGH. Audio is often underserved in game dev. The pipeline structure mirrors real audio team workflows
- **Quality**: 7/10. Good pipeline structure but relatively thin instructions per step compared to other skills. Relies heavily on agent expertise

### 31. team-combat
- **Purpose**: Orchestrate combat team through full feature development pipeline
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Bash, Task, AskUserQuestion, TodoWrite` | `argument-hint: "[combat feature]"`
- **Content**: 6-phase pipeline: Design, Architecture, Implementation (parallel), Integration, Validation, Sign-off. 6 team roles (game-designer, gameplay-programmer, ai-programmer, technical-artist, sound-designer, qa-tester)
- **Game dev usefulness**: HIGH. Combat is the most complex system in many games and benefits from structured cross-discipline coordination
- **Quality**: 7/10. Good pipeline with parallelism in Phase 3. But the individual phase instructions are relatively brief

### 32. team-level
- **Purpose**: Orchestrate level design team for complete area/level creation
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Bash, Task, AskUserQuestion, TodoWrite` | `argument-hint: "[level name or area]"`
- **Content**: 5-step pipeline: Narrative Context (narrative-director + world-builder), Layout/Encounter Design (level-designer), Systems Integration (systems-designer), Visual Direction (art-director), QA Planning (qa-tester)
- **Game dev usefulness**: HIGH. Level design requires cross-discipline coordination that is hard to manage informally
- **Quality**: 7/10. Good role assignments per step. The narrative-first approach (Step 1) is the right ordering for story-driven games

### 33. team-narrative
- **Purpose**: Orchestrate narrative team for story content, world lore, and narrative-driven level design
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Task, AskUserQuestion, TodoWrite` | `argument-hint: "[narrative content]"`
- **Content**: 5-phase pipeline: Narrative Direction, World Foundation (parallel: world-builder + writer), Level Narrative Integration, Review/Consistency, Polish (localization readiness, string keys)
- **Game dev usefulness**: MEDIUM-HIGH. Relevant for narrative-heavy games. The consistency checking and canon level management are valuable
- **Quality**: 7/10. The 120-character dialogue line limit and localization readiness in the polish phase show practical awareness

### 34. team-polish
- **Purpose**: Orchestrate polish team for performance, visual, audio, and QA hardening
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Bash, Task, AskUserQuestion, TodoWrite` | `argument-hint: "[feature or area]"`
- **Content**: 6-phase pipeline: Assessment, Optimization, Visual Polish (parallel), Audio Polish (parallel), Hardening (soak/stress testing), Sign-off with before/after metrics
- **Game dev usefulness**: HIGH. Polish is the phase that separates amateur from professional games. Structuring it prevents the "random bug fixing" anti-pattern
- **Quality**: 7/10. Good parallelization of visual and audio polish. The soak test and stress test requirements are mature

### 35. team-release
- **Purpose**: Orchestrate release team from candidate to deployment
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Bash, Task, AskUserQuestion, TodoWrite` | `argument-hint: "[version or 'next']"`
- **Content**: 7-phase pipeline: Release Planning, Release Candidate, Quality Gate (parallel), Localization/Performance, Go/No-Go, Deployment, Post-Release (monitoring, bug reports)
- **Game dev usefulness**: HIGH. Structured release process with proper sign-offs is essential for commercial games
- **Quality**: 7/10. Good pipeline with 48-hour post-release monitoring requirement. Integrates with other skills (/release-checklist, /changelog)

### 36. team-ui
- **Purpose**: Orchestrate UI team from wireframe to final implementation
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write, Edit, Bash, Task, AskUserQuestion, TodoWrite` | `argument-hint: "[UI feature]"`
- **Content**: 5-phase pipeline: UX Design (flows, wireframes, accessibility), Visual Design, Implementation (with key rules: UI never owns game state, no hardcoded strings), Review (parallel UX + art), Polish
- **Game dev usefulness**: HIGH. UI is critical for games and the "UI NEVER owns or modifies game state" rule prevents a common architectural mistake
- **Quality**: 8/10. The accessibility requirements (text scaling, colorblind mode, keyboard-only navigation) and the dual input method support (KB/mouse + gamepad) show real game UI experience

### 37. tech-debt
- **Purpose**: Track, categorize, and prioritize technical debt with a maintained register
- **Frontmatter**: `allowed-tools: Read, Glob, Grep, Write` | `argument-hint: "[scan|add|prioritize|report]"`
- **Content**: 4 subcommands (scan, add, prioritize, report). 6 debt categories (architecture, code quality, test, documentation, dependency, performance). Priority scoring formula: `(impact * frequency) / fix_effort`
- **Game dev usefulness**: MEDIUM-HIGH. Tech debt tracking is universal, not game-specific. But the scan targets (500+ line files, 50+ line functions) are practical
- **Quality**: 7/10. The priority scoring formula is a nice quantitative touch. The 3-sprint age flagging prevents debt from being permanently ignored

---

## Overall Assessment

### Are these 37 skills genuinely distinct and useful?

**Mostly yes, with 2 notable redundancies:**

1. **`release-checklist` vs `launch-checklist`**: These overlap heavily. `release-checklist` covers build verification, quality gates, platform-specific requirements, store metadata, and launch readiness. `launch-checklist` covers code readiness, content, QA, store/distribution, infrastructure, community/marketing, and operations. While `launch-checklist` is broader (includes community, marketing, infrastructure sections), the core sections (build, QA, store, legal) are duplicated. These should be merged into one skill with scope options.

2. **`changelog` vs `patch-notes`**: Both generate player-facing release notes from git history. `changelog` produces both internal and player-facing versions. `patch-notes` focuses on player-facing with 3 style tiers. The overlap is significant -- `patch-notes` is essentially a more polished version of the player-facing half of `changelog`. Could be merged with `changelog` gaining the style tiers.

**The remaining 35 skills are genuinely distinct**, covering different phases, activities, and concerns in game development.

### Do they follow Claude Code's skill definition format correctly?

**Yes, all 37 skills follow the format correctly:**
- All have valid YAML frontmatter with `---` delimiters
- All include `name`, `description`, `user-invocable: true`, and `allowed-tools`
- All include `argument-hint` (correct field name)
- Several use the `context:` field for git data (changelog, retrospective, scope-check, sprint-plan) -- correctly formatted with `!` prefix for shell commands
- Three skills use `Explicit invocation only` annotations (hotfix, launch-checklist, release-checklist) -- good practice for high-stakes actions
- Tool allowlists are appropriately scoped (read-only skills don't get Write/Edit)

### Would a game developer actually use these?

**Yes, with caveats:**

**Daily/weekly use** (high-frequency):
- `brainstorm`, `design-system`, `map-systems`, `prototype`, `sprint-plan`, `code-review`, `bug-report`

**Phase-specific use** (at the right time):
- `setup-engine` (once per project), `gate-check` (at phase transitions), `launch-checklist`/`release-checklist` (at release), `retrospective` (end of sprint), `balance-check` (during tuning phase)

**Occasional use** (when needed):
- `hotfix` (emergencies), `reverse-document` (inherited codebases), `onboard` (new team members), `localize` (pre-release)

**Team orchestration skills** (team-*) are ambitious but depend entirely on the quality of the agent definitions they delegate to. A solo developer would not use these, but a team using AI agents for different roles could find them valuable.

### Redundancy Analysis

| Redundant Pair | Overlap Level | Recommendation |
|---|---|---|
| release-checklist + launch-checklist | HIGH (70%+) | Merge into one skill with `--scope minimal\|full` |
| changelog + patch-notes | MEDIUM (50%) | Merge; add style tiers to changelog |
| project-stage-detect + gate-check | LOW (different purpose) | Keep separate -- diagnostic vs prescriptive |
| milestone-review + retrospective | LOW (different timing) | Keep separate -- forward-looking vs backward-looking |

### Quality Score: 8.5/10

**Strengths:**
- Deep game design domain knowledge throughout (MDA, Bartle, self-determination theory, pity timers, degenerate strategies, TTK analysis)
- Coherent workflow pipeline: `start` -> `brainstorm` -> `setup-engine` -> `map-systems` -> `design-system` -> `prototype` -> `sprint-plan` -> ... -> `gate-check` -> ... -> `launch-checklist`
- Consistent collaborative protocol across all skills (question -> options -> decision -> draft -> approval)
- Proper `AskUserQuestion` usage at decision points
- Session recovery built into complex skills (design-system)
- Read-before-write discipline (no skill auto-generates without context gathering first)
- `context:` frontmatter used correctly for pre-loading git data

**Weaknesses:**
- 2 clear redundancies (release-checklist/launch-checklist, changelog/patch-notes)
- Team orchestration skills (team-*) are structurally identical -- all follow the same "pipeline of subagent delegations" pattern with only the roles and steps changing. A generic "team-orchestrate" skill with configuration could replace all 7
- Some skills are not game-specific (estimate, onboard, tech-debt, retrospective, sprint-plan) and could be replaced by generic Claude Code skills
- No skill addresses multiplayer/networking, procedural generation, or shader/VFX authoring specifically (though these are covered indirectly through team skills and code review)

### Top 5 Best Skills

1. **brainstorm** (10/10) -- Professional-grade creative facilitation with real game design frameworks. The crown jewel
2. **design-system** (10/10) -- Production-grade GDD authoring with session recovery, cross-referencing, and specialist delegation
3. **map-systems** (10/10) -- Dependency-aware systems decomposition with implicit system inference. The bridge between concept and implementation
4. **setup-engine** (10/10) -- Uniquely solves the LLM knowledge gap problem for engine versions. Innovative
5. **reverse-document** (9/10) -- Addresses the reality that most code is written before it is documented. The intent-questioning protocol is excellent

### Top 5 Weakest Skills

1. **onboard** (6/10) -- Thin template, relatively generic, not much game-specific value
2. **estimate** (7/10) -- Standard estimation framework, not game-specific
3. **patch-notes** (7/10) -- Redundant with changelog's player-facing output
4. **release-checklist** (7/10) -- Redundant with launch-checklist
5. **sprint-plan** (7/10) -- Standard agile sprint planning, minimal game-specific adaptation

### Architectural Observations

The skills form a coherent **game development lifecycle pipeline**:

```
/start -> /brainstorm -> /setup-engine -> /map-systems -> /design-system -> /design-review
    -> /prototype -> /gate-check -> /sprint-plan -> [team-* skills for implementation]
    -> /perf-profile -> /balance-check -> /playtest-report -> /scope-check
    -> /milestone-review -> /retrospective -> /gate-check (polish)
    -> /localize -> /release-checklist -> /launch-checklist -> /changelog -> /patch-notes
```

This lifecycle awareness is the collection's greatest strength. Skills reference each other at appropriate handoff points, creating a guided workflow that prevents users from skipping critical steps. The `gate-check` skill acts as the workflow enforcer, blocking advancement when artifacts are missing and recommending the specific skill needed to create them.

The `context:` frontmatter field is used sparingly but correctly (4 skills), pre-loading git data that the skill needs for analysis without requiring a separate Bash call.

### Final Verdict

This is a **well-crafted, deeply knowledgeable skill system** that demonstrates genuine understanding of both game development processes and Claude Code's skill architecture. It is not generic filler -- the game-specific knowledge (balance analysis, core loop decomposition, engine knowledge gap detection, asset pipeline auditing) would be difficult to replicate from generic templates. The 2 redundancies and 7 structurally identical team-* skills are the main areas for improvement.

**Recommended for production use** with the two redundancy merges applied.
