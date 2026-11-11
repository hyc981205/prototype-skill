# 前端原型工作区

定位：**快速原型设计 | 零构建 HTML/CSS/JS | 浏览器直接打开 | 高保真辅助用外部库**

## 核心规则
- 每个原型一个独立目录，中文命名，放在 `prototypes/` 根目录下
- 新建原型：复制 `_template/` 到新目录，按 `skills/01-scaffold.md` 规范初始化
- **变更已有原型**：每次修改前必须先阅读该原型的 `CODE_ARCHITECTURE.md`，定位代码区域后再编码。修改完成后同步更新 `CODE_ARCHITECTURE.md`（见下方流程）
- 所有原型必须是纯静态文件，浏览器直接打开即可运行
- 外部库仅当"高保真需要"时引入，按 `skills/05-external-libs.md` 约束

## 原型变更流程（强制 — 禁止跳过）

任何时候对一个已有原型进行修改（新增需求、修复 bug、重构、视觉优化），**必须逐项执行以下步骤。跳过任意一步视为流程违规，不允许编码或提交**。

### 步骤 ① — 阅读 CODE_ARCHITECTURE.md（前置，不可跳过）
在修改任何代码文件之前，必须先用 `Read` 工具阅读该原型目录下的 `CODE_ARCHITECTURE.md`。
- 阅读后，**必须用一句话总结当前架构要点并写入会话**（例如："当前架构：树表 7 列，甘特图三区布局，RACI 悬浮卡片，关闭弹窗级联"），以证明已阅读。
- 若文件不存在，需先创建它。
- ⚠️ 禁止直接请求工具调用（Read/Edit/Write/Bash）修改代码文件，除非已完成此步骤。

### 步骤 ② — 阅读 REQUIREMENTS.md（前置，不可跳过）
用 `Read` 工具阅读 `REQUIREMENTS.md`，了解完整的需求演变历史，明确"当前要改什么"。
- 阅读后，明确本次变更对应的上一次需求记录位置。
- 禁止在不了解已有需求上下文的情况下直接编码。

### 步骤 ③ — 需求变更记录（修改前，不可跳过）
将本次变更需求追加到 `REQUIREMENTS.md`，格式：`### YYYY-MM-DD — 变更标题` + 条目列表。
- 未记录之前，不允许进入编码阶段。
- 变更类型标注：[新增]/[修改]/[删除]

### 步骤 ④ — 编码实现
按已有的架构约定编码，保持文件间风格一致。

### 步骤 ⑤ — 同步更新 CODE_ARCHITECTURE.md（提交前，不可跳过）
git commit 之前，必须重新打开 `CODE_ARCHITECTURE.md`，逐项比对以下内容是否与最新代码一致。不一致的立即补充：
- 新增/重命名/删除的函数
- 状态变量和常量
- 数据模型字段变动（dateMap、raciMap 等）
- 事件绑定增减
- HTML 骨架变化（新增/删除的列、容器、弹窗等）
- CSS 类名增减
- ⚠️ 跳过此步骤直接提交的，视为流程违规。

### 步骤 ⑥ — 提交版本
`git add` → `git commit` → `git tag` → `git push`

## 原型开发流程（新建原型）

-3. **阅读 REQUIREMENTS.md（前置）** — 先阅读该原型目录下的 `REQUIREMENTS.md`，了解完整的需求演变历史，明确"要改什么"。禁止在不了解已有需求上下文的情况下直接编码。
-2. **阅读 CODE_ARCHITECTURE.md（前置）** — 再阅读该原型目录下的 `CODE_ARCHITECTURE.md`，快速了解代码结构、文件分工、关键函数和状态变量，定位"在哪改"。若文件不存在，先创建之。
-1. **阅读相关 skills/ 约定文件（按需）** — 视当前任务按需阅读 `skills/` 下的相关文件。全量阅读 6 个文件效率低，仅在以下场景阅读对应文件：
    - **新建原型** → 读 `01-scaffold.md`、`02-html-template.md`
    - **视觉优化** → 读 `06-ui-ux-pro-max.md`
    - **引入外部库** → 读 `05-external-libs.md`
    - 其余约定已在本文件"技术规范"节定义，无需重复阅读。
0. **需求澄清** — 与用户反复沟通，逐层确认领域模型、数据模型、UI 交互，直到需求明确无歧义
1. **需求记录** — 将确认的需求写入 `REQUIREMENTS.md`（按"需求审计追踪"规范）
2. **技术方案** — 给出 UI 布局方案、组件拆分、模拟数据设计，用户确认后再动手
3. **开发实现** — 按确认的方案编码
4. **需求变更** — 开发过程中或完成后，用户提出的任何新增/修改需求，追加到 `REQUIREMENTS.md`
5. **同步更新文档（必须）** — 代码修改完成后，同步更新 `CODE_ARCHITECTURE.md`：新增/重命名/删除的函数、状态变量、数据模型字段变动、事件绑定变化、HTML 骨架变化，必须反映到架构文档中。确保下次开发时 `CODE_ARCHITECTURE.md` 仍与代码真实结构一致。

## 技术规范

本工作区统一遵守以下约定，定义在 CLAUDE.md 中以避免每次会话重复读取 skills/ 文件：

### HTML 规范（源自 skills/02）
- `lang="zh-CN"`，必设 `viewport` 和 `charset="UTF-8"`
- `style.css` 在 `<head>` 引入，`script.js` 在 `</body>` 前引入
- 使用语义标签 `<header>` `<main>` `<section>` 等
- 布局模式：控制面板型（toolbar + workspace + 左右面板）、仪表盘型、单页表单型

### CSS 规范（源自 skills/03）
- 通用 reset：`*, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }`
- CSS Variables 主题体系在 `:root` 中定义
- 组件类命名：`.btn` `.btn-primary` `.form-input` `.card` `.state-empty` `.state-loading` `.state-error`
- 布局用 Flexbox 或 Grid，禁止 `float` 和 `!important`

### JS 规范（源自 skills/04）
- IIFE + `'use strict'` 包裹全部代码，无全局泄漏
- DOM 引用统一用 `document.querySelector()` / `getElementById()`，缓存变量
- 事件绑定优先使用委托模式（`closest()`），减少重复 addEventListener
- 禁止 `innerHTML` 拼接用户可见内容（XSS 风险），使用 `textContent` 或 `createElement`
- 搜索输入使用 300ms 防抖
- 禁止引入 Redux/Vuex/Pinia 等状态管理库

## UI/UX 设计智能

当原型项目涉及 UI 视觉优化、设计质量提升时，必须调用 `ui-ux-pro-max` skill（详见 `skills/06-ui-ux-pro-max.md`）。
该 skill 提供设计智能数据库（50+ 样式、161 配色、57 字体配对、99 UX 准则），用于：
- 新建原型时的设计方向确认
- 已有原型的视觉质量审查和优化
- 配色、字体、间距等设计决策
- 可访问性、交互模式、响应式行为的 UX 审查

## 需求审计追踪
- 每个原型目录下必须有 `REQUIREMENTS.md`，作为该原型的需求审计文件
- **需求确认后**：将确认的需求要点写入 `REQUIREMENTS.md`（日期 + 需求描述）
- **每次修改/新增需求**：追加一条记录（日期 + 变更类型[修改/新增/删除] + 变更内容 + 变更原因）
- 格式：`### YYYY-MM-DD` 标题 + 条目列表
- 此文件不删除条目，保持完整的审计链，可追溯需求的演变过程

## Git 版本管理（必须）

本项目使用 Git 管理版本。仓库位于 `prototypes/` 目录。

### 发布版本（Release）
当用户说"备份"、"发布版本"、"我需要一个版本"时，执行以下操作：

1. `git add -A` 暂存所有更改
2. `git commit -m "<原型名> vX.X: <变更摘要>"` 提交
3. `git tag -a <原型名>/vX.X -m "<版本说明>"` 打标签

**Tag 命名规则**：`<原型目录名>/vX.X`
- 示例：`资源分配工具/v1.0`、`资源分配工具/v1.1`、`批注列表/v1.0`
- 每个原型有独立的版本线，互不干扰
- 初始版本 `v1.0`，小幅修改递增小版本 `v1.1`、`v1.2`...
- 重大改动/重构递增主版本 `v2.0`、`v3.0`...
- 由 Claude 和用户协商确定版本号

### 查看历史版本
- `git tag` — 列出所有发布版本（按原型分组）
- `git checkout <原型名>/v1.0` — 切换到该原型的 v1.0 版本（只读对比用）
- `git checkout master` — 回到最新开发状态

### 推送至 GitHub
远程仓库：`https://github.com/<你的账号>/prototype.git`（首次使用时替换为你自己的仓库）

发布版本后，将本地提交和 tag 同步到 GitHub：
```
git push origin master --tags
```

### 约束
- **禁止**在原型目录下创建本地备份子目录（如 `v1.0/`），Git tag 已替代此功能
- 每次 commit 对应当前正在开发的原型，不跨原型混合提交
- 新建原型无需额外配置，提交 + 打 tag 即自动纳入版本管理
- `.gitignore` 已配置忽略 `.claude/`、系统文件
- 发布版本后应推送到 GitHub，确保云端备份

## 技术栈
- HTML5 + CSS3 + 原生 JavaScript（ES6+）
- CSS Variables 主题体系（`skills/03-styling.md`）
- 外部库 CDN 引入（`skills/05-external-libs.md`）
- 无构建工具、无框架（除非必须在 skill 05 白名单内）

## 目录结构
```
prototypes/
├── CLAUDE.md              # 本文件
├── README.md              # 原型索引
├── skills/                # 6 项 skill 文件
│   ├── 01-scaffold.md     # 新建原型规范
│   ├── 02-html-template.md # HTML 骨架规范
│   ├── 03-styling.md      # CSS 样式约定
│   ├── 04-scripting.md    # JS 交互模式
│   ├── 05-external-libs.md # 外部库引入规则
│   └── 06-ui-ux-pro-max.md # 视觉优化调用规范
├── _template/             # 空白原型模板
│   ├── index.html
│   ├── style.css
│   ├── script.js
│   └── CODE_ARCHITECTURE.md  # 模板架构文档
└── 工具名/                # 每个原型独立目录
    ├── index.html
    ├── style.css
    ├── script.js
    ├── NOTES.md
    ├── REQUIREMENTS.md     # 需求审计追踪
    └── CODE_ARCHITECTURE.md # 代码架构索引（前置必读）
```
