---
name: prototype-workflow
description: >
  Manage frontend prototype development workflow under the `prototypes/` directory.
  This skill enforces a strict development process for both **new prototype creation**
  and **existing prototype modification**. Trigger whenever the user mentions:
  - "新建原型" / "create a new prototype"
  - "给 XX 原型加个功能" / "add a feature to the XX prototype"
  - "修 bug" / "fix an issue" in a prototype
  - "优化 XX 原型的 UI" / "improve the UI of XX prototype"
  - "需求变更" / "requirement change" related to any prototype
  - "重构" / "refactor" within prototypes/
  - Any task that reads from or writes to the `prototypes/<name>/` directory
  Do NOT trigger for general coding tasks outside prototypes/ or for read-only queries
  about prototypes (e.g., "show me what prototypes exist").
  This skill works alongside `prototypes/CLAUDE.md` — respect both sets of rules.
---

# prototype-workflow

This skill governs all work inside the `prototypes/` workspace. It ensures every
change — whether creating a new prototype or modifying an existing one — follows
a repeatable, auditable process that leaves behind accurate documentation.

## How to use this skill

When the user gives you a task involving prototypes/, determine which workflow
applies (**New Prototype** or **Modify Existing**), then follow the steps below
in order. Do not skip any step unless the user explicitly tells you to.

Read `prototypes/CLAUDE.md` for the full technical conventions (HTML/CSS/JS
norms, UI/UX skill requirements, Git version management rules). Those rules
complement the workflow steps here.

---

## Workflow A: New Prototype

Use when the user says "新建" or asks you to create a new prototype from scratch.

### Step 1 — Confirm name and scope

Confirm with the user:
- **Name** — directory name, use Chinese (e.g., `日报生成器`, `数据筛选面板`)
- **Purpose** — what problem does this prototype solve?
- **Data** — what mock data is needed? Scale (~30 nodes, 5-15 records, etc.)

### Step 2 — Read scaffold conventions

Read these two files:
1. `prototypes/skills/01-scaffold.md` — new prototype initialization rules
2. `prototypes/skills/02-html-template.md` — HTML skeleton conventions

### Step 3 — Create directory and initialize

Copy `prototypes/_template/` to `prototypes/<name>/` and modify:
- `<title>` to match the prototype name
- Create `NOTES.md` with creation date, purpose, usage scenario, data description
- Create `REQUIREMENTS.md` with initial requirements

### Step 4 — Design and validate

Before writing code:
1. Present a UI layout plan (component tree, layout diagram, column structure)
2. Confirm mock data structure with the user
3. Get user approval before coding

### Step 5 — Code

Implement following the technical conventions in `prototypes/CLAUDE.md`:
- Pure HTML5/CSS3/JS, no build tools
- CSS Variables theme system
- IIFE + `'use strict'` for JS
- No `innerHTML` for user-visible content (XSS risk)
- Event delegation pattern

### Step 6 — Create CODE_ARCHITECTURE.md

After coding, create `CODE_ARCHITECTURE.md` documenting:
- HTML skeleton (complete tree)
- CSS component classes
- JS architecture (functions, state, events)

### Step 7 — Update README.md

Add a row to `prototypes/README.md` prototype index table.

### Step 8 — Version control

Commit and tag following Git rules in `prototypes/CLAUDE.md`:
```
git add -A
git commit -m "<name> v1.0: <summary>"
git tag -a <name>/v1.0 -m "<description>"
git push origin master --tags
```

---

## Workflow B: Modify Existing Prototype

Use when the user requests any change to an existing prototype — new features,
bug fixes, UI polish, refactoring, or requirement changes. Do not skip any step.

### Before coding

**Step B1 — Read architecture file**
- Open `prototypes/<name>/CODE_ARCHITECTURE.md` with the Read tool
- Summarize to the user in one sentence: "当前架构：X列树表，Y区甘特图，Z功能..."
- If the file does not exist, create it by reading all source files and documenting
  the current structure before making changes

**Step B2 — Read requirements history**
- Open `prototypes/<name>/REQUIREMENTS.md` with the Read tool
- Identify the most recent requirement entry to understand the current context

**Step B3 — Record the change**
- Append a new entry to `REQUIREMENTS.md` with the format:
  ```
  ### YYYY-MM-DD — <change type>: <title>
  - <description of changes>
  ```
- Change types: [新增]/[修改]/[删除]/[修复]
- Do not start coding until this is done

### Coding

**Step B4 — Implement**
- Use the architecture document to locate which functions/classes/containers to change
- Read only the specific code sections you need (not the entire file)
- Keep changes consistent with existing architecture
- Follow the technical conventions in `prototypes/CLAUDE.md`

### After coding (before git commit)

**Step B5 — Update architecture document**
- Re-open `prototypes/<name>/CODE_ARCHITECTURE.md`
- Check each section against the current code:
  - HTML skeleton: new/removed containers, columns, modals
  - CSS classes: added/removed components
  - JS functions: added/removed/renamed functions, state variables, events
- Update anything that no longer matches the code
- This step is mandatory — skipping it violates the workflow

### Git commit

**Step B6 — Version and push**
- Determine version bump: feature addition → minor (1.0→1.1), breaking change →
  major (1.0→2.0), bug fix → patch (1.0→0.1). Negotiate with user if unsure.
- Check git status first, diff the changes, review commit style
- Commit message format: `<name> v<version>: <short summary>`
- Tag: `<name>/v<version>`
- Push: `git push origin master --tags`

---

## Git version rules

Reference from `prototypes/CLAUDE.md`:
- Each prototype has its own version line (独立版本线)
- Tag format: `<prototype-dir-name>/v<X>.<Y>` (e.g., `工作运行监控/v2.7`)
- Initial version is always v1.0
- Minor changes increment the patch version (v1.0 → v1.1)
- Major rewrites increment the major version (v1.0 → v2.0)
- Commit only the current prototype's files, do not mix prototypes
- After committing, always push to GitHub

## Architecture document template

Use this structure for any new CODE_ARCHITECTURE.md:

```markdown
# <prototype name> — 代码架构
> 生成日期：YYYY-MM-DD | 最后更新：YYYY-MM-DD

## 文件索引
| 文件 | 行数 | 用途 |

## HTML 骨架
[Complete tree of #app containers and key elements]

## CSS 结构
[Key component classes and their purposes]

## JS 架构
### 核心函数
| 函数 | 说明 |
### 事件绑定
| 事件 | 处理 |
### 筛选/渲染逻辑
```
