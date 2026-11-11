# 05 — 外部库引入规则

为保真需要引入第三方库时的选择和约束。

## 引入原则

1. **默认不用** — 原型优先用纯 HTML/CSS/JS 实现
2. **高保真例外** — 仅当用户明确要求"做得逼真"或"接近上线效果"时才引入
3. **CDN 优先** — 所有外部库通过 CDN 引入，不下载、不构建
4. **版本锁定** — 必须指定具体版本号，不使用 `@latest`

## CDN 白名单

以下库可在需要时通过 CDN 引入。未列出的库必须征得用户同意。

| 场景 | 可用库 | CDN 示例（版本锁定） |
|---|---|---|
| 响应式框架 | Vue 3 | `unpkg.com/vue@3.4.0/dist/vue.global.prod.js` |
| UI 组件库 | Element Plus | `unpkg.com/element-plus@2.5.0/dist/index.full.min.js` + CSS |
| 图表 | ECharts 5 | `cdn.jsdelivr.net/npm/echarts@5.4.3/dist/echarts.min.js` |
| 图标 | Lucide | `unpkg.com/lucide@0.300.0/dist/umd/lucide.js` |
| 日期处理 | dayjs | `cdn.jsdelivr.net/npm/dayjs@1.11.10/dayjs.min.js` |
| 工具函数 | Lodash | `cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js` |

## CDN 引入位置

- CSS 在 `<head>` 中引入，位于 `style.css` **之前**（以便变量覆盖）
- JS 在 `<body>` 底部引入，位于 `script.js` **之前**

```html
<head>
  <!-- 外部库 CSS 先引入 -->
  <link rel="stylesheet" href="https://unpkg.com/element-plus@2.5.0/dist/index.css">
  <!-- 自己样式最后引入，可覆盖第三方变量 -->
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div id="app">...</div>

  <!-- 外部库 JS 先引入 -->
  <script src="https://unpkg.com/vue@3.4.0/dist/vue.global.prod.js"></script>
  <script src="https://unpkg.com/element-plus@2.5.0/dist/index.full.min.js"></script>
  <!-- 自己脚本最后引入 -->
  <script src="script.js"></script>
</body>
```

## 禁止使用的库
- jQuery（历史债务，不引入）
- Bootstrap（整体框架太重，用 CSS Variables 体系替代）
- React / Angular（单原型不需要框架级别的复杂度）
- 任何需要构建/编译的库

## 判断流程

```
需要引入外部库？
├─ 纯 HTML/CSS 能实现？ → 不用，自己写
├─ 只有少量交互（如 tab 切换、模态框）？ → 不用，自己写
├─ 需要图表（柱状/折线/饼图）？ → 引入 ECharts
├─ 需要完整 UI 组件（表格/日期选择/下拉树）？ → 引入 Element Plus
└─ 其他情况？ → 先问用户
```
