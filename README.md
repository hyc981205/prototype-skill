# Prototype Skill

一套**可移植的前端原型协作 Skill**：把「模糊需求 → 设计规格 → 零构建可运行原型」的流程，
分别封装成 Codex、Claude Code、豆包工作、WorkBuddy 四个即插即用包。

> 目标不是让 AI 写更多代码，而是让它先问对问题、留下可追溯的文档，再按统一规范产出
> 能双击打开的原型。

---

## 它解决什么问题

直接用对话让 AI 做原型，通常有三个坑：

1. **问不对**：用户说不清界面，AI 直接开写，产出一版错的原型，返工成本高
2. **记不住**：改了几轮之后，没人知道为什么这么设计、上一版长什么样
3. **不一致**：同一个工作区里的原型，风格、文件结构、技术栈各不相同

这套 Skill 的对策：

| 问题 | 机制 |
|---|---|
| 说不清需求 | 三轮结构化访谈：问现状、给草图选项、回声确认、明确不做 |
| 需求丢失 | `REQUIREMENTS.md` 只追加不删，形成审计链 |
| 改代码乱动 | 修改前必读 `CODE_ARCHITECTURE.md`，改完必须同步 |
| 风格漂移 | `_template/` 标准基线 + CSS 变量主题 + 统一文件结构 |
| 版本混乱 | 每个原型独立 Git tag 版本线 |

---

## 工作流

```
阶段 A：设计访谈                     阶段 B：开发
─────────────────                   ─────────────
现状 / 痛点 / 使用者                 读取规范 → 复制模板
      ↓                                    ↓
布局草图 + 主区字段                   布局与数据确认
      ↓                                    ↓
明确不做 + 视觉基调                   编码 → CODE_ARCHITECTURE.md
      ↓                                    ↓
DESIGN_SPEC.md + PROMPT.md    ───→    索引更新 → commit / tag
```

**两段不混**：访谈不写代码，开发不重复澄清需求。

---

## 四个包

| 包 | 适用 | 形态 | 设计检索 |
|---|---|---|---|
| [`packages/codex`](packages/codex) | Codex CLI / IDE | `AGENTS.md` + `.codex/skills/` + `prototypes/` | ✅ Python |
| [`packages/claude-code`](packages/claude-code) | Claude Code | `CLAUDE.md` + `.claude/skills/` + `prototypes/` | ✅ Python |
| [`packages/doubao`](packages/doubao) | 豆包工作 / 知识库型 agent | 5 份知识库 + 系统提示词 + 模板 | ❌ 中文准则 |
| [`packages/workbuddy`](packages/workbuddy) | 腾讯 WorkBuddy | 4 个 Skill + 市场 zip | ✅ Python |

每个包都有独立的 `使用说明.md`，包内文件路径自洽，可单独复制或打包使用。

### Codex

```bash
cp -r packages/codex/AGENTS.md packages/codex/references \
      packages/codex/.codex packages/codex/prototypes  <项目>/
```

### Claude Code

```bash
cp -r packages/claude-code/.claude packages/claude-code/prototypes <项目>/
cp packages/claude-code/CLAUDE.md <项目>/
```

### 豆包工作

上传 `packages/doubao/知识库/` 下 5 个文件，把 `系统提示词.md` 粘到自定义指令。

### WorkBuddy

```powershell
Copy-Item -Recurse -Force "packages/workbuddy/skills/*" "$env:USERPROFILE\.workbuddy\skills"
```

`packages/workbuddy/上传市场/` 内是可直接上传技能市场的 zip（`SKILL.md` 在包根）。

---

## 一个原型长什么样

```
prototypes/<原型名>/
├── index.html              # 双击即可打开
├── style.css               # CSS 变量主题
├── script.js               # IIFE + 'use strict'
├── NOTES.md                # 目的 / 场景 / 数据
├── REQUIREMENTS.md         # 需求审计链
└── CODE_ARCHITECTURE.md    # 代码索引，改前必读
```

技术约定：

- 纯 HTML5 / CSS3 / 原生 JS，**零构建、无框架**
- 颜色全部走 `:root` 变量；Flex/Grid；禁 `float`、禁 `!important`
- 禁 `innerHTML` 拼用户可见内容；事件委托；搜索 300ms 防抖
- 外部库默认不用，只走 CDN 白名单并锁定版本

---

## 目录结构

```
.
├── README.md
├── LICENSE
├── packages/
│   ├── codex/
│   ├── claude-code/
│   ├── doubao/
│   └── workbuddy/
└── docs/
    ├── 测试说明.md        # 3 个场景的人工测试脚本
    ├── 评分卡.md          # 逐项打分与偏差分类
    └── evals.json         # 机器可读评测用例
```

---

## 设计能力来源

包内 `design-skills/` 与 `.codex/skills/` 下包含两个第三方设计 Skill：

- **frontend-design** — 高保真 UI 生成。许可证见各包内 `LICENSE.txt`
- **ui-ux-pro-max** — 可检索设计数据库，MIT License，Copyright (c) 2024 Next Level Builder

本仓库只做流程封装与跨平台适配，不修改这两个 Skill 的内容。

---

## 测试

见 [`docs/测试说明.md`](docs/测试说明.md)。三个场景覆盖：

1. 空白头脑 + 有现状材料 —— 是否先问现状、给具体选项
2. 模糊想法 → 开发 → 改需求 —— 是否遵守六步变更流程
3. 用户全程「不知道」 —— 是否给默认值而不是反复追问

用 [`docs/评分卡.md`](docs/评分卡.md) 打分，任红线项不通过即该场景不通过。

---

## License

[MIT](LICENSE)

第三方 Skill 的许可证随包附带，以各自文件为准。
