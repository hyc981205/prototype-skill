# 01 — 新建原型规范

原型开发的入口 skill。每当用户说"新建原型"、"做一个 XXX 工具"、"搭一个 XXX 页面"时，按此规范执行。

> **全局流程约束**：任何原型开发必须遵循 `prototype-workflow` 技能定义的流程——先逐层确认需求 → 记录需求 → 技术方案 → 开发 → 追踪变更。禁止跳过需求澄清直接写代码。

## frontend-design 调用约束（必须）

**每次原型开发必须调用内置 `frontend-design` skill** 生成 UI。这是默认行为，不可跳过。

极少数例外（需用户明确确认后才可跳过）：
- 纯工具型原型（JSON Formatter、编码转换等）
- 用户明确说"不需要设计"、"用基础样式就行"

**调用方式**: 使用 `Skill` 工具，参数 `skill: "frontend-design"`，将用户的原型需求描述作为 args 传入。

**调用时机**: 在需求确认后、正式编码前调用。拿到 frontend-design 生成的 HTML/CSS 后，以此为蓝本放入原型目录，JS 逻辑按 `04-scripting.md` 规范自行编写。

## ui-ux-pro-max 调用约束（必须）

当用户要求视觉优化、设计审查、风格选择、UX 改进时，**必须调用 `ui-ux-pro-max` skill**（详见 `skills/06-ui-ux-pro-max.md`）。

- 新建原型时，先调用 `frontend-design` 生成初稿，再调用 `ui-ux-pro-max` 审查优化
- 已有原型优化时，直接调用 `ui-ux-pro-max` 获取设计方向建议
- **调用方式**: 使用 `Skill` 工具，参数 `skill: "ui-ux-pro-max"`

## 标准流程

### Step 1：确认名称
- 目录名使用中文，简洁直白，如 `数据筛选面板`、`日报生成器`
- 如果是英文工具名，直接用英文，如 `JSON Formatter`

### Step 2：复制模板
- 将 `_template/` 整个复制到 `prototypes/工具名/`
- 保留 `index.html`、`style.css`、`script.js` 三个文件

### Step 3：初始化 NOTES.md
在新原型目录下创建 `NOTES.md`，记录：
```markdown
# 工具名
- 创建日期：YYYY-MM-DD
- 目的：一句话描述这个原型要解决什么问题
- 使用场景：谁在什么情况下用
- 数据：需要什么模拟数据 / 从哪里来
```

### Step 4：更新 README.md
在 `prototypes/README.md` 的原型索引表中新增一行。

### Step 5：创建 CODE_ARCHITECTURE.md
开发完成后，按 `CODE_ARCHITECTURE.md` 格式规范创建该原型的代码架构文档，包含 HTML 骨架、CSS 结构、JS 函数/变量/事件绑定的完整索引。确保后续开发时可通过此文件快速定位代码区域。

## 约束
- 原型目录永远不嵌套（`工具名/` 直接包含文件，不再有子目录）
- 如果原型需要图片/图标，放在 `工具名/assets/` 下
- 不做多余的初始化工作：不创建 package.json、不引入构建工具
