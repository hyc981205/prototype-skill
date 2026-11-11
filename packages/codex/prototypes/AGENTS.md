# 前端原型工作区

定位：**快速原型设计 | 零构建 HTML/CSS/JS | 浏览器直接打开 | 高保真辅助用外部库**

本文件是 Codex 工作区规则，与根目录 `../AGENTS.md` 配合使用。

## 核心规则
- 每个原型一个独立目录，中文命名，放在工作区根目录下
- 新建原型：复制 `_template/` 到新目录，按 `skills/01-scaffold.md` 规范初始化
- **变更已有原型**：每次修改前必须先读该原型的 `CODE_ARCHITECTURE.md`，定位代码区域后再编码；修改完成后同步更新该文件
- 所有原型必须是纯静态文件，浏览器直接打开即可运行
- 外部库仅在「高保真需要」时引入，遵循 `skills/05-external-libs.md`

## 原型变更流程（强制 — 禁止跳过）

对已有原型做任何修改（新增需求、修 bug、重构、视觉优化），必须逐项执行：

### ① 阅读 CODE_ARCHITECTURE.md（前置，不可跳过）
修改任何代码文件前，先读该原型目录下的 `CODE_ARCHITECTURE.md`。
- 读后用**一句话总结当前架构要点并写进对话**（例：「当前架构：树表 7 列，甘特图三区，RACI 悬浮卡片」），证明已读。
- 文件不存在则先创建。
- 未完成本步，禁止读写代码文件。

### ② 阅读 REQUIREMENTS.md（前置，不可跳过）
读 `REQUIREMENTS.md`，了解需求演变历史，明确本次要改什么、对应上一次记录在哪。

### ③ 需求变更记录（编码前，不可跳过）
把本次变更追加到 `REQUIREMENTS.md`：
```
### YYYY-MM-DD — [新增/修改/删除/修复] 变更标题
- 变更内容
- 变更原因
```
未记录不得进入编码。

### ④ 编码实现
按既有架构约定编码，保持文件间风格一致，只读需要改的代码段。

### ⑤ 同步更新 CODE_ARCHITECTURE.md（提交前，不可跳过）
重新打开 `CODE_ARCHITECTURE.md`，逐项比对并与代码对齐：
- 新增/重命名/删除的函数
- 状态变量和常量
- 数据模型字段变动
- 事件绑定增减
- HTML 骨架变化（新增/删除的列、容器、弹窗）
- CSS 类名增减

### ⑥ 提交版本
`git add` → `git commit` → `git tag` → `git push`。agent 无 shell 时，输出完整命令清单由用户执行。

## 原型开发流程（新建原型）

-2. 读相关 `skills/` 约定（按需，不要全读）：
  - 新建原型 → `01-scaffold.md`、`02-html-template.md`
  - 视觉优化 → `06-ui-ux-pro-max.md`；设计 skill 本体在 `../.codex/skills/`，
    都不可用时读 `../references/design-fallback.md`
  - 引入外部库 → `05-external-libs.md`
  - 其余约定见本文件「技术规范」节
-1. 读 `REQUIREMENTS.md`（如已有）与 `CODE_ARCHITECTURE.md`（如已有）
0. 需求澄清：逐层确认领域模型、数据模型、UI 交互，直到无歧义；若上游已有 `DESIGN_SPEC.md`，直接转写
1. 需求记录：把确认的需求写进 `REQUIREMENTS.md`
2. 技术方案：UI 布局、组件拆分、模拟数据设计，用户确认后再动手
3. 开发实现
4. 需求变更：开发中/完成后新增的需求，一律追加到 `REQUIREMENTS.md`
5. 同步更新文档（必须）：`CODE_ARCHITECTURE.md` 必须与最新代码一致

## 技术规范

### HTML
- `lang="zh-CN"`，必设 `viewport` 与 `charset="UTF-8"`
- `style.css` 在 `<head>` 引入，`script.js` 在 `</body>` 前引入
- 使用 `<header> <main> <section>` 等语义标签；表单用 `<form>` + `<label>`；按钮用 `<button>`
- 布局模式：控制面板型（toolbar + workspace + 面板）、仪表盘型、单页表单型

### CSS
- 通用 reset：`*, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }`
- `:root` 定义 CSS Variables 主题，所有颜色走变量
- 组件类：`.btn` `.btn-primary` `.form-input` `.card` `.state-empty` `.state-loading` `.state-error`
- Flexbox / Grid，禁 `float` 和 `!important`

### JS
- IIFE + `'use strict'` 包裹全部代码，无全局泄漏
- DOM 引用缓存；事件优先委托（`closest()`）
- 禁止 `innerHTML` 拼接用户可见内容，用 `textContent` / `createElement`
- 搜索输入 300ms 防抖
- 禁 Redux/Vuex/Pinia

## UI/UX 设计智能

涉及视觉优化、设计质量提升时：
- 设计规则见 `skills/06-ui-ux-pro-max.md`
- 设计 skill 本体在 `../.codex/skills/ui-ux-pro-max/` → 先读其 `SKILL.md`；
  能执行命令时用
  `python ../.codex/skills/ui-ux-pro-max/scripts/search.py "<关键词>" --design-system`
- 生成高保真初稿 → 读 `../.codex/skills/frontend-design/SKILL.md`
- 以上都不可用 → 按 `../references/design-fallback.md` 的检查清单执行

## 需求审计追踪
- 每个原型目录必须有 `REQUIREMENTS.md`
- 需求确认后写入要点；每次修改追加一条（日期 + 类型 + 内容 + 原因）
- 不删除条目，保持完整审计链

## Git 版本管理（有 Git 时必须）

### 发布版本
用户说「备份」「发布版本」「我需要一个版本」时：
1. `git add -A`
2. `git commit -m "<原型名> vX.X: <变更摘要>"`
3. `git tag -a <原型名>/vX.X -m "<版本说明>"`
4. `git push origin master --tags`（远程仓库自行配置）

### Tag 规则
- 格式 `<原型目录名>/vX.X`，每个原型独立版本线
- 初始 `v1.0`；小幅修改 `v1.1`、`v1.2`；重大改动/重构 `v2.0`
- 版本号与用户协商确定

### 约束
- 禁止在原型目录下建本地备份子目录（如 `v1.0/`），用 Git tag
- 每次 commit 只对应一个原型，不跨原型混合
- `.gitignore` 忽略 `.claude/`、系统文件

## 技术栈
HTML5 + CSS3 + 原生 JavaScript（ES6+）；CSS Variables 主题；外部库 CDN；无构建、无框架

## 目录结构
```
prototypes/
├── AGENTS.md               # 工作区规则
├── README.md               # 原型索引
├── skills/                 # 6 个约定文件
│   ├── 01-scaffold.md
│   ├── 02-html-template.md
│   ├── 03-styling.md
│   ├── 04-scripting.md
│   ├── 05-external-libs.md
│   └── 06-ui-ux-pro-max.md
├── _template/              # 空白模板
│   ├── index.html / style.css / script.js
│   └── CODE_ARCHITECTURE.md
└── 工具名/
    ├── index.html / style.css / script.js
    ├── NOTES.md
    ├── REQUIREMENTS.md
    └── CODE_ARCHITECTURE.md
```
