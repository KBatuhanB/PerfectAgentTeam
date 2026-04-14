# PerfectAgentTeam

A production-grade multi-agent configuration system for AI-assisted software development. PerfectAgentTeam gives you a structured team of specialized AI agents — each with a defined role, clear responsibilities, and a shared contract — that coordinate to produce secure, tested, and documented code.

Available in **English** and **Turkish (Türkçe)**, in two team sizes per language.

---

## What Problem Does This Solve?

Most AI coding workflows rely on a single agent making all decisions. This leads to inconsistent output, skipped security checks, no documentation, and no accountability chain. PerfectAgentTeam solves this with:

- **Role separation** — each agent owns exactly one responsibility
- **Mandatory review gates** — security and quality reviewers must approve before delivery
- **Conditional test loop** — test-engineer activates only when needed, controlled by solution-architect or user directive
- **Global contract** — every agent is bound to the same coding standards, comment rules, and quality gates defined in `copilot-instructions.md`

---

## Repository Structure

```
PerfectAgentTeam/
├── English/
│   ├── CoreTeam/          # 7-agent team (recommended)
│   │   └── .github/
│   │       ├── copilot-instructions.md
│   │       └── agents/
│   │           ├── chief-orchestrator.agent.md
│   │           ├── solution-architect.agent.md
│   │           ├── backend-engineer.agent.md
│   │           ├── frontend-engineer.agent.md
│   │           ├── security-reviewer.agent.md
│   │           ├── quality-reviewer.agent.md
│   │           └── test-engineer.agent.md
│   └── BigTeam/           # Extended team variant
│
├── Türkçe/
│   ├── CoreTeam/          # Same 7-agent team, all content in Turkish
│   └── BigTeam/
│
├── .agents/skills/        # Skills for GitHub Copilot (VS Code)
├── .claude/skills/        # Skills for Claude Code (Anthropic CLI)
├── .windsurf/skills/      # Skills for Windsurf (Codeium IDE)
└── skills-lock.json       # Pinned skill versions and integrity hashes
```

---

## The Agent Team

### CoreTeam (Recommended)

Seven agents, each with a single responsibility:

| Agent | Role | Tools |
|-------|------|-------|
| **chief-orchestrator** | Entry point. Analyzes tasks, selects agents, manages review & test loops, delivers results | read, search, agent, todo |
| **solution-architect** | Technical planning, architecture decisions, data flow, test decision | read, search, edit, agent, todo |
| **backend-engineer** | API, business logic, database, services — production-grade backend code | read, search, edit, execute, agent, todo |
| **frontend-engineer** | UI components, pages, state, API integration — with design quality standards | read, search, edit, execute, agent, todo, web |
| **security-reviewer** | OWASP Top 10 audit, input validation, auth/authz, sensitive data — read-only | read, search, agent, todo |
| **quality-reviewer** | SOLID, clean code, naming, comments, architectural alignment — read-only | read, search, agent, todo |
| **test-engineer** | Targeted unit/integration/E2E tests — activates only when required | read, search, edit, execute, agent, todo, web |

### Workflow at a Glance

```
User → chief-orchestrator
          ├── Simple task  → engineer → quality-reviewer → deliver
          └── Complex task → solution-architect (plan)
                               → engineer(s) (implement)
                               → security-reviewer + quality-reviewer (parallel review)
                               → [solution-architect: test required?]
                               → [test-engineer] (if needed)
                               → deliver + summary
```

---

## Included Skills

Four production skills are bundled with the team and auto-activate in the right context:

| Skill | Used By | Activates When |
|-------|---------|----------------|
| **frontend-design** | frontend-engineer | Writing any UI/component/page — enforces distinctive, non-generic design |
| **webapp-testing** | test-engineer | E2E browser testing via Playwright |
| **mcp-builder** | backend-engineer | Building MCP servers to connect external APIs to LLMs |
| **doc-coauthoring** | solution-architect | Writing technical specs, ADRs, RFCs, architecture documents |

---

## Installation

### Requirements

- [VS Code](https://code.visualstudio.com/) with the [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) and [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extensions
- GitHub Copilot subscription (Individual, Business, or Enterprise)
- Node.js 18+ (for the `skills` CLI)

### Step 1 — Clone the repository

```bash
git clone https://github.com/your-org/PerfectAgentTeam.git
cd PerfectAgentTeam
```

### Step 2 — Copy the team files into your project

Copy the `.github/` folder from your chosen team variant into the root of your project:

```bash
# English CoreTeam (recommended)
cp -r English/CoreTeam/.github /path/to/your-project/.github

# Turkish CoreTeam
cp -r Türkçe/CoreTeam/.github /path/to/your-project/.github
```

Your project should now have:
```
your-project/
└── .github/
    ├── copilot-instructions.md
    └── agents/
        ├── chief-orchestrator.agent.md
        ├── solution-architect.agent.md
        ├── backend-engineer.agent.md
        ├── frontend-engineer.agent.md
        ├── security-reviewer.agent.md
        ├── quality-reviewer.agent.md
        └── test-engineer.agent.md
```

### Step 3 — Install skills

Skills must be installed so agents can reference them. Install them globally using the `skills` CLI:

```bash
npx skills add anthropics/skills --skill frontend-design
npx skills add anthropics/skills --skill webapp-testing
npx skills add anthropics/skills --skill mcp-builder
npx skills add anthropics/skills --skill doc-coauthoring
```

This installs them to `~/.agents/skills/` where VS Code Copilot automatically discovers them.

> **What are skills?** Skills are structured prompt instruction files (`SKILL.md`) that agents read when a relevant task arises. They are not plugins or extensions — they are embedded guidance that shapes agent behavior in specific domains.

### Step 4 — Verify in VS Code

1. Open your project in VS Code
2. Open Copilot Chat (`Ctrl+Shift+I`)
3. In agent mode, type `@chief-orchestrator` — the agent should be available
4. Start a task: `@chief-orchestrator build a user authentication API`

---

## Customization

### Adjusting agent behavior

Each `.agent.md` file is self-contained. Open any agent file and edit its prompt sections directly:
- Change the tools list in the YAML frontmatter
- Add or remove agent-to-agent references
- Modify the rules or workflow sections

### Changing the language

The English and Turkish variants are functionally identical. The only difference is the language of instructions and the code comment rule:
- **English version**: all code comments must be in English
- **Turkish version**: all code comments must be in Turkish (`// Türkçe yorum zorunlu`)

### Adding new skills

1. Find or create a skill (see [anthropics/skills](https://github.com/anthropics/skills))
2. Install it: `npx skills add anthropics/skills --skill skill-name`
3. Add a section to the relevant agent file describing when and how to use it
4. Add an entry to the skill inventory table in `copilot-instructions.md` Section 18
5. Update `skills-lock.json` with the skill's hash

### Modifying quality gates

The global contract is defined in `.github/copilot-instructions.md`. Key sections:
- **Section 6** — Test loop rules and user directive overrides
- **Section 10** — Review Gate, Test Gate, Delivery Gate
- **Section 15** — Red lines (rules that can never be violated)

---

## The `.agents`, `.claude`, and `.windsurf` Directories

This repo ships skill files in three parallel directories. Each one targets a different AI coding tool:

| Directory | Tool | Purpose |
|-----------|------|---------|
| `.agents/skills/` | **GitHub Copilot** (VS Code) | Copilot discovers skills here at the workspace level |
| `.claude/skills/` | **Claude Code** (Anthropic CLI) | Claude Code reads skills from `.claude/` for project context |
| `.windsurf/skills/` | **Windsurf** (Codeium IDE) | Windsurf reads skills from `.windsurf/` for its Cascade AI |

All three directories contain the same skill files. If you only use one tool, you only need that tool's directory — the others can be safely ignored or deleted.

> The `.github/agents/` agent files are GitHub Copilot-specific and follow the [GitHub Copilot Custom Agents](https://docs.github.com/en/copilot/customizing-copilot/using-github-copilot-chat-in-your-ide) specification. For Claude Code or Windsurf, you would adapt the agent prompts into those tools' own formats (`CLAUDE.md` / Windsurf rules).

---

## Design Principles

- **Security first** — security-reviewer has veto power on every code change
- **No single-agent trust** — every implementation goes through at least one independent reviewer
- **Conditional testing** — tests are only written when they add real value; the system does not self-burden with unnecessary test load
- **Comment every line** — undocumented code is rejected; every line must explain why it exists
- **Deterministic decisions** — same input, same conditions → same decision chain, every time
- **Step-by-step progress** — never write the entire system at once; verify each step before moving to the next

---

## License

MIT — use freely, modify as needed, no attribution required.
