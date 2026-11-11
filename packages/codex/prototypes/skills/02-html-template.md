# 02 — HTML 骨架规范

每个原型的 `index.html` 必须遵循此规范。

## 标准骨架

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>工具名</title>
  <link rel="stylesheet" href="style.css">
  <!-- 外部库 CDN 在此引入（按 skill 05 规范） -->
</head>
<body>
  <div id="app">
    <!-- 页面结构 -->
  </div>

  <!-- 外部库 JS 在此引入（body 底部） -->
  <script src="script.js"></script>
</body>
</html>
```

## 必须遵守

| 规则 | 说明 |
|---|---|
| `lang="zh-CN"` | 默认中文 |
| `viewport` | 必须设置，适配移动端 |
| `charset` | 必须 `UTF-8` |
| CSS 优先外部 | `style.css` 在 `<head>` 中引入 |
| JS 在 body 底部 | 所有 `<script>` 放在 `</body>` 前 |
| 外部库在 script.js 之前 | 先引 CDN 库，再引自己的 `script.js` |

## 语义化要求
- 使用 `<header>`、`<main>`、`<nav>`、`<section>`、`<footer>` 等语义标签
- 表单必须使用 `<form>` + `<label>` 绑定，而非裸 `<input>`
- 按钮使用 `<button>`，不使用 `<div onclick>`

## 工具类布局结构

常见布局模式：

**控制面板型**（筛选/配置类工具）：
```html
<div id="app">
  <header class="toolbar">  <!-- 顶部工具栏：标题 + 快捷操作 --> </header>
  <main class="workspace">
    <aside class="panel panel-left">  <!-- 左侧：筛选/配置面板 --> </aside>
    <section class="panel panel-main"> <!-- 中间：主内容区 --> </section>
    <aside class="panel panel-right"> <!-- 右侧：详情/属性面板 --> </aside>
  </main>
  <footer class="statusbar"> <!-- 底部状态栏 --> </footer>
</div>
```

**仪表盘型**（数据展示类工具）：
```html
<div id="app">
  <header class="toolbar"> <!-- 标题 + 全局操作 --> </header>
  <main class="dashboard">
    <!-- 网格布局的卡片区域 -->
  </main>
</div>
```

**单页表单型**（输入→输出类工具）：
```html
<div id="app">
  <main class="form-container">
    <section class="input-area"> <!-- 输入区 --> </section>
    <section class="action-bar"> <!-- 操作按钮 --> </section>
    <section class="output-area"> <!-- 输出/结果区 --> </section>
  </main>
</div>
```
