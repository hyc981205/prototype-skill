# _template — 代码架构（模板基线）

> 生成日期：2026-05-16 | 所有原型由此模板派生

## 文件索引

| 文件 | 行数 | 用途 |
|------|------|------|
| `index.html` | 31 | 三栏布局骨架（工具栏 + 左面板 + 主面板） |
| `style.css` | 178 | 标准 CSS Variables 主题体系 + 通用组件样式 |
| `script.js` | 12 | 空 IIFE 骨架，4 个注释分区 |

---

## HTML 骨架

```
#app
├── header.toolbar
│   ├── h1.toolbar-title ("原型名称")
│   └── div.toolbar-actions (空占位)
└── main.workspace
    ├── aside.panel.panel-left (左面板，280px 固定宽)
    └── section.panel.panel-main (主面板，flex:1)
```

仅有 `#app` 一个 ID。无数据属性。所有类名仅供 CSS 使用。

---

## CSS 结构

### :root 变量
完全遵循 `03-styling.md` 规范标准主题：
- 主色：靛蓝 `#4F46E5` / `#4338CA` / `#EEF2FF`
- 中性色：`#F9FAFB` / `#FFFFFF` / `#E5E7EB` / `#111827` / `#6B7280`
- 功能色：`#059669` / `#D97706` / `#DC2626`
- 间距：4~32px (xs~xl)
- 字号：12~24px (sm~2xl)
- 圆角：4/8/12px (sm/md/lg)

### 组件类

| 类名 | 用途 |
|------|------|
| `.btn` | 基础按钮（边框/背景/悬浮过渡） |
| `.btn-primary` | 主色调实心按钮 |
| `.form-input` `.form-select` | 表单控件（聚焦主色边框+发光） |
| `.form-label` | 标签（小号+大写+次要色） |
| `.card` | 卡片容器（白色+边框+圆角+阴影+内边距） |
| `.state-loading` `.state-empty` `.state-error` | 三种空状态占位（居中+次要色） |

### 布局规范
- `.toolbar` flex row, 56px 高, 底部边框
- `.workspace` flex row, `calc(100vh - 56px)`
- `.panel-left` 固定 280px, `flex-shrink:0`, 右边框
- `.panel-main` `flex:1`, 自适应
- 禁止 float, 禁止 !important

---

## JS 架构 (script.js)

### 骨架结构
```js
(function () {
  'use strict';

  // ── DOM REFS ──

  // ── EVENTS ──

  // ── UTILITIES ──

  // ── INIT ──
  console.log('原型已就绪');
})();
```

### 约定模式
- IIFE + 严格模式，无全局泄漏
- 4 个分区：DOM 缓存 → 事件绑定 → 工具函数 → 初始化
- 无预先定义的事件或 DOM 引用
- `console.log('原型已就绪')` 作为初始化标记

---

## 模板用途说明

- 新原型开发时复制此目录，按 `01-scaffold.md` 初始化
- HTML 三栏布局适合"列表+详情"模式
- CSS 变量体系保证跨原型视觉一致
- JS 骨架签入后按 `04-scripting.md` + `06-ui-ux-pro-max.md` 扩展
