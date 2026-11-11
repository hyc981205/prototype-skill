---
name: prototype-workflow
description: "Manage the frontend prototype development workflow under the prototypes/ workspace: create a new prototype (zero-build HTML/CSS/JS, one directory per prototype containing index.html, style.css, script.js, NOTES.md, REQUIREMENTS.md, CODE_ARCHITECTURE.md) or modify an existing one under a strict read-architecture -> record-requirement -> code -> sync-architecture -> git tag process. 触发场景：'新建原型'、'做个 XXX 工具/页面/面板'、'给 XX 原型加个功能'、原型里'修 bug'、'优化 XX 原型的 UI'、'需求变更'、'重构原型'、'发布版本/发版本'，以及任何读写 prototypes/<name>/ 目录的任务；也用于 prototype-design-interview 产出 DESIGN_SPEC.md + PROMPT.md 之后的执行阶段。不触发：prototypes/ 之外的普通编码任务、只问有哪些原型的只读查询、只要一个独立 HTML 页面且不建原型工作区（那种情况用 html 技能）。"
---

# prototype-workflow

Governs all work inside the `prototypes/` workspace. Every change — new prototype or
modification — follows a repeatable, auditable process that leaves accurate documentation
behind.

## 本技能的依赖（自包含）

原版依赖项目里的 `prototypes/CLAUDE.md` 与 `prototypes/skills/01–06.md`。本技能已把这些
内容打包进来，**不需要项目里再有一份**（项目里存在时以项目内版本为准，冲突时以项目内
为准并提示用户）：

| 依赖 | 本技能内位置 | 何时读 |
|---|---|---|
| 工作区技术规范与变更流程（原 `prototypes/CLAUDE.md`） | `references/project-conventions.md` | 每次触发时必读 |
| 新建原型规范（原 `skills/01-scaffold.md`） | `references/01-scaffold.md` | 新建原型时 |
| HTML 骨架规范 | `references/02-html-template.md` | 新建原型时 |
| CSS 约定 | `references/03-styling.md` | 写样式时 |
| JS 交互约定 | `references/04-scripting.md` | 写 JS 时 |
| 外部库白名单 | `references/05-external-libs.md` | 需要引第三方库时 |
| 视觉优化调用规范 | `references/06-ui-ux-pro-max.md` | 视觉/UX 优化时 |
| 空白原型模板 `_template/` | `assets/_template/` | 新建原型时复制 |
| 原型索引模板 `README.md` | `assets/prototypes-README.md` | 初始化工作区时 |

同工作区的辅助技能：`prototype-design-interview`（需求模糊时的访谈）、
`frontend-design`（UI 初稿）、`ui-ux-pro-max`（设计数据库，检索脚本在
`.user_skills/ui-ux-pro-max/scripts/search.py`）。
调用它们的方式是先用 Read 打开该技能的 `SKILL.md`，再按其内容执行——不是靠一个
"Skill 工具"。

---

## Step 0 — 确认工作区（每次触发先做，不可跳过）

1. 判断当前项目目录下是否存在 `prototypes/`。
2. **存在** → Read `prototypes/CLAUDE.md`（若存在）；同时读本技能的
   `references/project-conventions.md`，两者一致时按项目内版本执行。
3. **不存在** → 询问用户把原型工作区建在哪里（默认当前项目目录），确认后初始化：
   - 建 `prototypes/` 目录
   - 把 `references/project-conventions.md` 的内容写成 `prototypes/CLAUDE.md`
   - 把 `assets/prototypes-README.md` 的内容写成 `prototypes/README.md`
   - 把 `assets/_template/` 整个复制成 `prototypes/_template/`
   - 需要版本管理时在 `prototypes/` 里 `git init`，并加一个忽略系统文件的 `.gitignore`
   - 初始化完再往下一步走，不要跳过
4. 无论哪条路径，都要确认自己已读过规范再动代码。

---

## Workflow A: 新建原型

用户说"新建"或要求从零做一个原型时走这条。

### A1 — 确认名称与范围

与用户确认：
- **名称** — 目录名，中文（如 `日报生成器`、`备件领用`），英文工具名可直接用英文
- **目的** — 这个原型解决什么问题
- **数据** — 需要什么模拟数据、规模多大（约 5–15 条记录、30 个节点等）

### A2 — 读规范

读 `references/01-scaffold.md` 与 `references/02-html-template.md`。

### A3 — 建目录并初始化

复制 `assets/_template/` 到 `<项目>/prototypes/<名称>/`，然后：
- 改 `<title>` 为原型名
- 按 01-scaffold 建 `NOTES.md`（创建日期、目的、使用场景、数据说明）
- 建 `REQUIREMENTS.md` 写初始需求

### A4 — 设计与确认

写代码之前：
1. 给出 UI 布局方案（组件树、布局示意、列结构）
2. 与用户确认模拟数据结构
3. 拿到用户认可再动手。视觉要求高时先加载 `frontend-design` 产出初稿，
   再按需加载 `ui-ux-pro-max` 做设计决策

### A5 — 编码

按 `references/project-conventions.md` 的规范：
- 纯 HTML5/CSS3/JS，零构建
- CSS Variables 主题体系
- IIFE + `'use strict'`
- 用户可见内容禁用 `innerHTML`（XSS）
- 事件委托

### A6 — 建 CODE_ARCHITECTURE.md

写完后建 `CODE_ARCHITECTURE.md`：HTML 骨架（完整树）、CSS 组件类、JS 架构（函数、状态、事件）。

### A7 — 更新 README 索引

在 `prototypes/README.md` 的原型索引表追加一行。

### A8 — 版本

按 `references/project-conventions.md` 的 Git 规则：
```
git add -A
git commit -m "<名称> v1.0: <摘要>"
git tag -a <名称>/v1.0 -m "<说明>"
```
推送 `git push origin master --tags` 前确认远程已配置并与用户确认——不要擅自推送。

---

## Workflow B: 修改已有原型

新增功能、修 bug、UI 优化、重构、需求变更都走这条。**每一步都不能跳。**

### 编码前

**B1 — 读架构文档**
- Read `prototypes/<名称>/CODE_ARCHITECTURE.md`
- 用一句话向用户复述："当前架构：X 列树表，Y 区甘特图，Z 功能…"
- 文件不存在 → 先读全部源码，把现状补写出来再改

**B2 — 读需求历史**
- Read `prototypes/<名称>/REQUIREMENTS.md`，定位最近一条需求，明确这次要改什么

**B3 — 记录变更**
- 在 `REQUIREMENTS.md` 追加：
  ```
  ### YYYY-MM-DD — <变更类型>: <标题>
  - <变更说明>
  ```
- 变更类型：[新增]/[修改]/[删除]/[修复]
- 没写完这条不许开始编代码

### 编码

**B4 — 实现**
- 借架构文档定位要改的函数/类/容器
- 只读需要的那几段代码，不要整文件读
- 与既有架构保持一致，遵守 `references/project-conventions.md`

### 编码后（提交前）

**B5 — 同步架构文档**
- 重开 `CODE_ARCHITECTURE.md`，逐项比对：
  - HTML 骨架：新增/删除的容器、列、弹窗
  - CSS 类：增删的组件
  - JS：增删改的函数、状态变量、事件
- 不一致立即补。跳过即流程违规

### Git

**B6 — 版本与推送**
- 版本号：新增功能 → 小版本 +0.1（1.0→1.1）；重构/破坏性变更 → 主版本 +1（1.0→2.0）；修 bug → 补丁位；拿不准与用户协商
- 先看 `git status`、diff，再提交
- 提交信息 `<名称> v<版本>: <摘要>`，标签 `<名称>/v<版本>`
- 推送前与用户确认

---

## 常见违规（自查）

- 改已有原型却没先说一句话复述架构 → 违规，回 B1
- 没写 `REQUIREMENTS.md` 就开始编码 → 违规，回 B3
- 编码后没更新 `CODE_ARCHITECTURE.md` 就提交 → 违规，回 B5
- 一次提交混进多个原型 → 违规，拆分提交
- 在原型目录下建 `v1.0/` 这类备份子目录 → 禁止，用 Git tag
- 引入不在白名单里的库、用 `innerHTML` 拼用户内容、上构建工具 → 违规
