# 前端原型 — Claude Code 工作区

本目录即开即用。项目级 `.claude/skills/` 已装好四个技能，Claude Code 在本目录启动时自动加载：

| 技能 | 用途 |
|---|---|
| `prototype-design-interview` | 需求模糊时的结构化访谈，产出 `DESIGN_SPEC.md` + `PROMPT.md` |
| `prototype-workflow` | 新建/修改原型的强制流程与文档同步 |
| `frontend-design` | 高保真 UI 初稿 |
| `ui-ux-pro-max` | 可检索设计数据库（Python） |

## 开始工作前

1. 读 `prototypes/CLAUDE.md` —— 工作区技术规范与变更流程，**强制遵守**
2. 判断走哪条路：
   - 用户说不清界面/字段 → 调用 `prototype-design-interview`
   - 需求已清楚 / 改已有原型 → 调用 `prototype-workflow`
3. 原型一律建在 `prototypes/<中文名>/` 下

## 硬约束（摘要）

- 访谈阶段不写代码；开发阶段不重复澄清需求
- 改已有原型：先读 `CODE_ARCHITECTURE.md` 并一句话复述 → 读 `REQUIREMENTS.md` →
  追加变更记录 → 编码 → 提交前同步架构文档
- 技术栈：纯 HTML/CSS/JS，零构建；CSS 变量；IIFE + `'use strict'`；禁 `innerHTML` 拼用户内容
- 外部库只走 `prototypes/skills/05-external-libs.md` 白名单，CDN + 锁版本

## 快速命令

```
新建原型：新建一个原型，叫 XXX，……（若说不清，会自动进入访谈）
加功能：  给 XXX 原型加一个 …… 功能
发版本：  发布 vX.X
设计优化：调用 ui-ux-pro-max 审查这个原型的视觉
```
