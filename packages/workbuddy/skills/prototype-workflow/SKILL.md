---
name: prototype-workflow
display_name: 前端原型工作流
display_name_en: Prototype Workflow
description: "在 prototypes/ 工作区新建或修改零构建 HTML/CSS/JS 原型，强制执行需求记录、架构文档同步与版本管理。触发词：新建原型、做一个工具页面、给原型加功能、修原型的 bug、优化原型 UI、原型需求变更、重构原型。"
description_zh: "管理 prototypes/ 下的原型开发流程，新建与修改都必须走固定步骤并同步文档。"
description_en: "Manages the prototype workflow under prototypes/ with mandatory documentation and versioning steps."
examples_zh: ["新建一个备件领用原型", "给备件领用加状态筛选", "把工作台原型的数据表改成树表"]
examples_en: ["Create a spare-parts checkout prototype", "Add status filtering to the checkout prototype", "Change the dashboard table into a tree table"]
version: 1.0.0
author: "hyc"
---

# 前端原型工作流

管 `prototypes/` 工作区下的全部原型开发。新建和修改都必须按本文件步骤执行，不得跳过。
技术规范与完整规则见 `references/workspace-rules.md`。

## 首次使用：准备工作区

若项目的 `prototypes/` 不存在：

1. 把本技能 `assets/workspace/` 下的内容复制到 `<项目>/prototypes/`
2. 确认 `prototypes/` 内有：`skills/01–06`、`_template/`、`README.md`
3. 在 `prototypes/` 初始化 Git（需要版本管理时）

```bash
cp -r assets/workspace/. <项目>/prototypes/
cd <项目>/prototypes && git init
```

## 判断路线

- 用户说「新建」「做一个 XXX 工具」→ 走 A
- 用户要改已有原型（加功能、修 bug、调样式、重构、需求变更）→ 走 B

---

## A. 新建原型

1. **确认名称与范围**：中文目录名；目的；需要的模拟数据类型与量级。
2. **读约定**：读 `references/workspace-rules.md` 与 `prototypes/skills/01-scaffold.md`、
   `02-html-template.md`。
3. **初始化**：复制 `prototypes/_template/` 到 `prototypes/<名称>/`；改 `<title>`。
4. **写文档**：创建 `NOTES.md`（日期/目的/场景/数据）；若上游有 `DESIGN_SPEC.md`，
   把需求转写进 `REQUIREMENTS.md`。
5. **设计确认**：先给组件树/布局图/列结构 + 模拟数据结构，用户确认后再编码。
6. **编码**：遵循 `references/workspace-rules.md` 的 HTML/CSS/JS 规范。
7. **写 `CODE_ARCHITECTURE.md`**：文件索引、HTML 骨架、CSS 类、JS 函数/状态/事件。
8. **更新索引**：在 `prototypes/README.md` 原型表追加一行。
9. **版本化**：commit + tag（见文末）。

---

## B. 修改已有原型（六步，禁止跳过）

### ① 读架构
读 `prototypes/<名称>/CODE_ARCHITECTURE.md`，并用一句话向用户复述当前架构
（例：「当前架构：树表 7 列，甘特图三区，关闭弹窗级联」）。文件不存在就先通读源码补一份。
未完成本步，不得读写代码文件。

### ② 读需求
读 `prototypes/<名称>/REQUIREMENTS.md`，明确本次改什么、对应上一次记录在哪。

### ③ 追加变更记录
在 `REQUIREMENTS.md` 末尾追加：

```markdown
### YYYY-MM-DD — [新增/修改/删除/修复] 标题
- 变更内容
- 变更原因
```

未记录不得编码。

### ④ 编码
只读需要改的代码段，保持既有架构与风格。

### ⑤ 同步架构文档（提交前）
重新打开 `CODE_ARCHITECTURE.md`，逐项核对并与代码对齐：
- 新增/重命名/删除的函数
- 状态变量与常量
- 数据模型字段
- 事件绑定增减
- HTML 骨架（列、容器、弹窗）
- CSS 类名

### ⑥ 提交版本
```bash
git add -A
git commit -m "<原型名> vX.Y: <摘要>"
git tag -a <原型名>/vX.Y -m "<版本说明>"
git push origin master --tags
```
无 Git 或无权限时，输出完整命令清单让用户执行。

---

## 版本规则

- Tag 格式 `<原型目录名>/vX.Y`，每个原型独立版本线
- 初始 v1.0；功能新增 +0.1；修 bug 递增小版本；重构/破坏性改动 +1.0
- 每次提交只对应一个原型，不跨原型混合
- 禁止在原型目录建 `v1.0/` 之类备份目录

## 产出检查

- [ ] 原型目录含六个文件：`index.html` `style.css` `script.js` `NOTES.md`
      `REQUIREMENTS.md` `CODE_ARCHITECTURE.md`
- [ ] 双击 `index.html` 可打开，控制台无报错
- [ ] `CODE_ARCHITECTURE.md` 与代码一致
- [ ] README 索引已更新
- [ ] 本次需求有记录

## 与其他技能的分工

- 需求还说不清 → 先用 `prototype-design-interview` 产出 `DESIGN_SPEC.md` 与 `PROMPT.md`
- 视觉优化 → 用 `ui-ux-pro-max` 审查，`frontend-design` 出高保真初稿
- 本技能只负责执行流程与文档，不重新做需求访谈
