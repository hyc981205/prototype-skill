# 03 — CSS 样式约定

所有原型的样式代码放在 `style.css` 中，遵循此规范。

> **高保真 UI**: 当用户对视觉效果有要求时，参见 `01-scaffold.md` 中的 `frontend-design` 调用约束——由内置 skill 生成高质量样式，本文件作为默认基线。
> **视觉优化**: 当需要设计决策（配色、字体、间距、风格）或 UX 审查时，调用 `ui-ux-pro-max` skill（详见 `references/06-ui-ux-pro-max.md`）。

## CSS Reset（必须）

```css
*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

## CSS Variables 主题体系（必须）

在 `:root` 中定义，所有颜色必须使用变量引用：

```css
:root {
  /* 主色系 */
  --color-primary: #4F46E5;
  --color-primary-hover: #4338CA;
  --color-primary-light: #EEF2FF;

  /* 中性色 */
  --color-bg: #F9FAFB;
  --color-surface: #FFFFFF;
  --color-border: #E5E7EB;
  --color-text: #111827;
  --color-text-secondary: #6B7280;

  /* 功能色 */
  --color-success: #059669;
  --color-warning: #D97706;
  --color-error: #DC2626;

  /* 间距 */
  --spacing-xs: 4px;
  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;
  --spacing-xl: 32px;

  /* 字号 */
  --text-sm: 12px;
  --text-base: 14px;
  --text-lg: 16px;
  --text-xl: 20px;
  --text-2xl: 24px;

  /* 圆角 */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;

  /* 阴影 */
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-md: 0 4px 6px rgba(0,0,0,0.07);
  --shadow-lg: 0 10px 15px rgba(0,0,0,0.1);
}
```

## 全局基础样式

```css
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC",
               "Microsoft YaHei", sans-serif;
  font-size: var(--text-base);
  color: var(--color-text);
  background: var(--color-bg);
  line-height: 1.6;
  min-height: 100vh;
}
```

## 布局规范
- 主布局使用 Flexbox，复杂网格使用 CSS Grid
- 控制面板型布局使用 flex 三栏：`flex: 0 0 280px`（侧栏固定宽）+ `flex: 1`（主区自适应）
- 禁止使用 `float` 布局
- 禁止使用 `!important`（除非覆盖第三方库样式）

## 组件样式约定
- 按钮统一 `.btn` 基础类 + `.btn-primary` / `.btn-secondary` / `.btn-danger` 变体
- 表单控件统一 `.form-input`、`.form-select`、`.form-label` 类
- 卡片容器统一 `.card` 类
- loading/empty/error 三种空状态统一 `.state-loading` `.state-empty` `.state-error`

## 响应式规范
- 桌面优先，关键断点：`768px`（平板）、`480px`（手机）
- 侧栏在小屏自动折叠或隐藏
- 不追求像素级移动端适配，保持可用即可
