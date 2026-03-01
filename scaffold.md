You are a senior systems architect setting up a production-grade Claude Code 
workspace. Your job is to scaffold a complete, optimized baseline folder 
structure for a new project.

## CONTEXT YOU MUST GATHER FIRST

Before creating anything, ask me for:
1. Project name
2. Tech stack (languages, frameworks, databases)
3. Team size (solo, small team, enterprise)
4. Project type (API, mobile app, web app, CLI tool, library, monorepo)
5. Do I need MCP integrations? (GitHub, Jira, Slack, etc.)
6. Deployment target (cloud provider, on-prem, edge, mobile store)

Do not proceed until you have all 6 answers.

---

## WHAT TO CREATE

### 1. GLOBAL LAYER (~/.claude/)
Only scaffold this if it does not already exist. Never overwrite existing 
global files. If they exist, output a DIFF of what to ADD.

Create:
- ~/.claude/CLAUDE.md
- ~/.claude/settings.json
- ~/.claude/skills/ with these universal skills:
  - git-commit/SKILL.md
  - code-review/SKILL.md
  - security-audit/SKILL.md
  - documentation/SKILL.md
- ~/.claude/commands/
  - standup.md
  - ticket.md
  - refactor.md

### 2. PROJECT LAYER (./project-name/)
Scaffold the full project .claude/ directory and all supporting files.

Create:
- CLAUDE.md (project root)
- .mcp.json (if MCP integrations requested)
- .claude/
  - settings.json
  - settings.local.json (with placeholder content)
  - .gitignore (must include settings.local.json)
  - skills/ (generate based on tech stack)
  - commands/ (generate based on project type)
  - agents/ (generate based on team size and complexity)
  - hooks/ (pre-edit.sh, post-edit.sh, post-session.sh)

### 3. SUBDIRECTORY CLAUDE.md FILES
If the project has distinct layers (frontend/backend/infra/packages), 
create a CLAUDE.md in each with layer-specific conventions.

---

## CONTENT RULES

### CLAUDE.md files must include:
- Project/layer overview (2-3 sentences max)
- Tech stack with versions
- Directory structure map
- Build, test, lint, and run commands
- Code style and conventions
- Branch naming and commit format
- Critical files never to touch
- Environment variable reference (names only, never values)
- Links to key reference files using @filepath syntax

### SKILL.md files must include:
YAML frontmatter with:
  - name: (lowercase, hyphens only)
  - description: (specific triggers, what it does AND when to use it)
  - allowed-tools: (locked down to minimum required)

Body with:
  - Clear step-by-step instructions
  - Concrete examples
  - Edge cases
  - References to any bundled scripts or templates

### settings.json must include:
- PreToolUse hooks (block edits on main/master branch)
- PostToolUse hooks (auto-format on write/edit based on stack)
- Stop hook (session summary logger)
- Permissions locked to minimum required tools

### Hook scripts must:
- Be executable (chmod +x noted in output)
- Have error handling
- Output structured JSON responses for block/allow decisions
- Be stack-aware (run the right formatter for the right file type)

### commands/ files must:
- Have a clear one-line description comment at the top
- Accept arguments where useful ($1, $2 syntax)
- Be scoped correctly (root vs subdirectory)

### agents/ files must:
- Have a focused context scope (no cross-domain noise)
- Define what tools the agent can use
- Define what it must NOT do

---

## OUTPUT FORMAT

Deliver in this exact order:

1. DIRECTORY TREE
   Full annotated tree with (use case) beside every file and folder.
   Show both ~/.claude/ and ./project-name/ trees.

2. FILE CONTENTS
   Output every file's full content in labeled code blocks.
   Order: CLAUDE.md files first, then settings.json, then skills, 
   then commands, then agents, then hooks.
   
   Label each block:
   ### FILE: path/to/file.ext

3. SETUP COMMANDS
   Bash script block with every mkdir, touch, chmod needed to 
   instantiate the structure. Idempotent -- safe to run multiple times.

4. .gitignore ADDITIONS
   What to add to the project root .gitignore.

5. NEXT STEPS
   3-5 bullet points of what to customize after scaffolding.
   Be specific to the tech stack provided.

---

## CONSTRAINTS

- Never put secrets or values in any file, only variable names
- Never use Windows-style paths
- All hook scripts must be POSIX-compatible bash
- SKILL.md descriptions must be specific enough that Claude 
  auto-invokes them correctly -- no vague descriptions
- settings.local.json must always be gitignored
- Every generated file must be production-ready, not a placeholder
- Do not generate TODO comments -- generate actual content
- If the tech stack has an official formatter, wire it into post-edit hooks
- If the project is a monorepo, generate .claude/skills/ in each package

---

## QUALITY CHECK BEFORE OUTPUT

Before delivering output, verify:
- [ ] All SKILL.md files have specific description triggers
- [ ] settings.json hooks cover PreToolUse AND PostToolUse AND Stop
- [ ] CLAUDE.md files are concise (under 150 lines each)
- [ ] No file contains placeholder TODO content
- [ ] .gitignore includes settings.local.json
- [ ] Hook scripts have executable flag noted
- [ ] Global layer checks for existing files before overwriting
- [ ] Subdirectory CLAUDE.md files exist for each distinct layer

If any check fails, fix it before outputting.
