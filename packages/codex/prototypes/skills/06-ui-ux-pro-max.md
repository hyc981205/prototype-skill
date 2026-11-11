# 06 — UI/UX Pro Max 设计智能

集成 `ui-ux-pro-max` skill 到原型设计工作流。提供设计智能数据库（50+ 样式、161 配色、57 字体配对、99 UX 准则）用于 UI 质量优化。

> **本包（Codex）说明**：以下「使用 Skill 工具」的写法不适用。直接读
> `../.codex/skills/ui-ux-pro-max/SKILL.md`；能执行命令时用
> `python ../.codex/skills/ui-ux-pro-max/scripts/search.py "<关键词>" --design-system`。
> 两者都不可用则走 `../references/design-fallback.md`。

## 调用条件（必须）

以下情况 **必须** 调用此 skill：
- 用户要求优化/美化/改善已有原型的视觉效果
- 新建原型或重构 UI 时，与 `frontend-design` 配合使用
- 选择配色方案、字体系统、间距系统
- 审查 UI 代码的用户体验、可访问性、视觉一致性
- 实现动画、响应式布局、导航结构
- UI 看起来"不够专业"但原因不明确——先搜索 `ux` 领域诊断
- 实现暗色模式、图表数据可视化
- 做产品级设计决策（信息层级、品牌表达、风格选择）

## 调用方式

使用 `Skill` 工具，参数 `skill: "ui-ux-pro-max"`，将用户需求作为 args 传入。

Skill 提供的核心能力：
- `--design-system` 生成完整设计系统推荐（样式、配色、字体、效果）
- `--domain style` 搜索 50+ UI 风格及 CSS 关键词
- `--domain color` 按产品类型搜索配色方案
- `--domain typography` 搜索字体配对（含 Google Fonts import）
- `--domain ux` 搜索 UX 最佳实践和反模式
- `--domain chart` 图表类型和库推荐

## 优先级规则

视觉问题按以下优先级处理：

| 优先级 | 类别 | 关键检查 |
|--------|------|----------|
| 1 | 可访问性 | 对比度 ≥4.5:1, 焦点环, 键盘导航 |
| 2 | 触摸与交互 | 最小 44×44px 触摸区, 加载反馈 |
| 3 | 性能 | WebP/AVIF, 懒加载, 预留空间防 CLS |
| 4 | 风格选择 | 匹配产品类型, 一致性, SVG 图标 |
| 5 | 布局与响应式 | 移动优先断点, 无水平滚动 |
| 6 | 字体与颜色 | 基准 16px, 行高 1.5, 语义化颜色 token |
| 7 | 动画 | 持续 150-300ms, 尊重 prefers-reduced-motion |
| 8 | 表单与反馈 | 可见标签, 错误就近显示 |

## 无需调用的情况
- 纯后端逻辑、API、数据库设计
- 非视觉脚本或自动化任务
- 与界面无关的性能优化
- 仅涉及文案修改

## 与其他 skill 的协作

- `frontend-design`：生成高保真 UI 初稿。`ui-ux-pro-max` 在需要设计决策、风格指南、UX 审查时补充调用
- `03-styling.md`：CSS 编码规范基线。`ui-ux-pro-max` 提供更高层次的设计方向
- 新建原型时，先调用 `frontend-design` 生成初稿，再调用 `ui-ux-pro-max` 审查优化
