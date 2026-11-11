# 前端原型 — Codex 工作区

本目录即开即用。你要做的：把用户说不清的原型想法问清楚，产出 `DESIGN_SPEC.md` +
`PROMPT.md`；需求清楚后，按固定流程在 `prototypes/` 下产出零构建 HTML/CSS/JS 原型。

**本文件只做路由和硬约束。细节一律去读对应文件，不要凭印象编。**

---

## 0. 先判断走哪条路

| 用户情况 | 走 |
|---|---|
| 「没想清楚」「不知道要什么界面」「你问我答」「帮我理一下需求」 | 阶段 A 设计访谈 |
| 已能说清布局/字段，或已有 `DESIGN_SPEC.md` / `PROMPT.md` | 阶段 B 新建原型 |
| 改已有原型（加功能/修 bug/调样式/重构） | 阶段 B 修改原型 |

**顺序铁律**：A 只产出规格，不写代码；B 不再重复需求澄清。两段职责不混。

Codex 没有 Skill 工具，本包也不依赖它：下文所有「读 xxx / 运行 xxx」都是直接执行。
没有 `AskUserQuestion` 这类并排选择工具时，用「编号选项 + ```代码块 ASCII 草图```」呈现，
结尾写「回复编号，或说都不是」。

---

## 1. 阶段 A：设计访谈（产出规格，不写代码）

### 三条核心机制

1. **问现状，不问愿景。** 用户答不了「你想要什么界面」，答得了「你现在这件事怎么做」。
   Excel 表头、群里的来回、白板流程，本身就是免费需求：列名、字段、流程、例外都在里面。
   要问未来时先锚定现在：「现在筛的时候是手填还是下拉？」
2. **具象选择替代抽象描述。** 形状问题（布局、主区形态、密度、风格）必须给 2–4 个
   具体选项 + 草图，留「都不是」出口。开放题只留给业务、术语、例外。
3. **复述纠错替代从零生成。** 先推断、再给默认值 + 一句理由；每轮结束做回声确认：
   「我理解是……，对吗？」

**「我不知道」是合法答案。** 不重问。给推荐默认值 + 理由 + 标记为假设，继续往下。

### 三轮结构

**Round 1 意图层（一次一问）**
1. 使用时刻：「上一秒发生了什么，让你想打开它」
2. 使用者：自己 / 同事 / 团队 / 给领导看
3. **现状**：现在这件事怎么做？把表头、字段、流程尽量问出来（后续细节题的主要来源）
4. 痛点：现在最烦哪一步
5. 成功标准：哪件事变快或消失，就算有用

**Round 2 结构与任务层（一轮 3–4 问）**
1. 核心动作：进来第一件事做什么，最常做的三个动作（动词，不是名词）
2. 业务对象：让用户举一句具体例子，自己拆实体/字段/基数
3. 数据来源：系统导出 / 手填 / 复制粘贴
4. 布局方案：按整图形状给草图选项

**Round 3 细节层（一轮 3–4 问）**
1. 主区内容：念表头 / 一眼看哪几个信息 / 只留 3 列留哪 3 列
2. 状态与分支：默认全给让用户删
3. 视觉基调：专业冷色 / 工业深色 / 轻量明亮 / 高对比企业风
4. **明确不做：必须问。**

问题措辞、6 种布局草图、8 种工具形态、状态清单 → **先读 `references/question-bank.md`**。

### 收尾

1. 写 `DESIGN_SPEC.md` 到 `prototypes/<原型名>/`（可新建目录，不碰三个代码文件）
2. 写 `PROMPT.md` 到同目录。**必须能单独粘到空会话里成立**：不出现「参考设计规格」
   「见前文」「配套文档」；字段名、枚举值、状态清单写全。
3. 最后一次整体回声确认
4. 交接阶段 B，说明需求澄清已完成

模板与填写要求 → **读 `references/output-templates.md`**。

**阶段 A 禁止**：写代码、复制模板、执行 Git、创建 `REQUIREMENTS.md`。

### 自适应用户清晰度

| 情况 | 做法 |
|---|---|
| 开场就很清楚 | 跳过对应轮次，只补真缺口 |
| 全程「不知道」 | 给 2–3 个常见工具形态当骨架，选一个再改 |
| 只答关键词 | 二选一 / 「是/不是 + 我猜是 X」 |
| 话多跑题 | 先让说完，再收敛成一句回声确认 |

---

## 2. 阶段 B：开发工作流

完整规则 → **读 `prototypes/AGENTS.md`**。骨架：

### 新建
1. 确认中文目录名与范围
2. 读 `prototypes/skills/01-scaffold.md`、`02-html-template.md`
3. 复制 `prototypes/_template/` 到 `prototypes/<名称>/`
4. 写 `NOTES.md`；有 `DESIGN_SPEC.md` 就把需求转写进 `REQUIREMENTS.md`
5. 给布局方案 + 模拟数据结构，用户确认后再编码
6. 编码
7. 写 `CODE_ARCHITECTURE.md`
8. 更新 `prototypes/README.md` 索引
9. commit + tag（有 Git 时）

### 修改已有原型（六步，禁止跳过）
1. 先读 `prototypes/<名称>/CODE_ARCHITECTURE.md`，一句话复述架构写进对话
2. 读 `REQUIREMENTS.md`
3. 追加变更记录（日期 + [新增]/[修改]/[删除]/[修复]）
4. 编码
5. 提交前重读 `CODE_ARCHITECTURE.md`，逐项对齐函数/状态/事件/骨架/CSS 类
6. commit + tag

版本：新增 → +0.1；破坏性改动/重构 → +1；修 bug → 小版本递增。
Tag 格式 `<原型目录名>/vX.Y`。

---

## 3. 技术规范（硬约束）

- 纯 HTML5 / CSS3 / 原生 JS（ES6+），无构建工具、无框架
- 原型目录扁平：`index.html` `style.css` `script.js` `NOTES.md` `REQUIREMENTS.md` `CODE_ARCHITECTURE.md`
- CSS：`:root` 变量、颜色全走变量；Flex/Grid；禁 `float`、禁 `!important`
- JS：IIFE + `'use strict'`；禁 `innerHTML` 拼用户可见内容；事件委托；搜索 300ms 防抖
- 状态管理只用变量 + 重渲染；禁 Redux/Vuex/Pinia
- 外部库默认不用；必须用时只走 `prototypes/skills/05-external-libs.md` 白名单 + CDN + 锁版本
- 模拟数据 `MOCK_DATA`，列表 5–15 条，模拟 300–800ms 延迟
- 导出用 Blob + `a[download]`

细节 → `prototypes/skills/02` `03` `04` `05`。

### 设计能力

- 高保真 UI 初稿 → 读 `.codex/skills/frontend-design/SKILL.md`
- 配色/字体/风格/UX 审查 → 读 `.codex/skills/ui-ux-pro-max/SKILL.md`；
  能跑命令时：
  ```bash
  python .codex/skills/ui-ux-pro-max/scripts/search.py "<关键词>" --design-system
  ```
- 都不可用 → `references/design-fallback.md`
- 设计 skill 只决定方向；本包 CSS 技术规范优先

---

## 4. 文件地图

```
<项目根>/
├── AGENTS.md                      ← 本文件
├── 使用说明.md
├── references/
│   ├── question-bank.md           ← 访谈问题库、布局草图、工具形态
│   ├── output-templates.md        ← DESIGN_SPEC / PROMPT 模板
│   └── design-fallback.md         ← 设计兜底
├── .codex/skills/
│   ├── frontend-design/           ← SKILL.md
│   └── ui-ux-pro-max/             ← SKILL.md + data + scripts
└── prototypes/                    ← 工作区
    ├── AGENTS.md                  ← 工作区规则
    ├── README.md                  ← 原型索引
    ├── skills/01–06
    └── _template/
```

---

## 5. 完成判定

- 阶段 A：`DESIGN_SPEC.md` 五节无空节；`PROMPT.md` 可独立成立；「明确不做」有内容；用户确认过整体
- 阶段 B：原型能双击打开；`CODE_ARCHITECTURE.md` 与代码一致；`REQUIREMENTS.md` 有本次记录；
  README 索引已更新
