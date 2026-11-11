# 04 — JS 交互模式

所有原型的脚本代码放在 `script.js` 中，遵循此规范。

## 代码组织（必须）

使用 IIFE 包裹，避免全局污染：

```js
(function () {
  'use strict';

  // ── 状态 ─────────────────────────
  // ...

  // ── DOM 引用 ─────────────────────
  // ...

  // ── 事件绑定 ─────────────────────
  // ...

  // ── 工具函数 ─────────────────────
  // ...

  // ── 初始化 ───────────────────────
  // ...
})();
```

如果原型逻辑较复杂（>200 行），拆分为清晰分区并用注释分隔，不拆成多个 JS 文件。

## DOM 操作

- 查询统一用 `document.querySelector()` / `querySelectorAll()`
- 禁止使用 `innerHTML` 拼接用户可见内容（XSS 风险），使用 `textContent` 或 `createElement` + `appendChild`
- 事件绑定优先使用事件委托，减少重复 `addEventListener`

```js
// 事件委托模式（推荐）
document.querySelector('#list').addEventListener('click', function (e) {
  const btn = e.target.closest('.list-item-action');
  if (!btn) return;
  // 处理点击...
});
```

## 模拟数据

原型需要数据时，在 JS 文件中定义 `MOCK_DATA` 常量：

```js
const MOCK_DATA = {
  list: [
    { id: 1, name: '示例一', status: 'active' },
    { id: 2, name: '示例二', status: 'inactive' },
  ],
  total: 42,
};
```

- 数据量够展示即可（列表 5-15 条，够翻页/筛选演示）
- 数据结构和真实 API 返回结构对齐，方便后续对接
- 模拟异步延迟（300-800ms）展示 loading 状态

## 状态管理

简单原型用变量 + DOM 直写：

```js
let currentFilter = 'all';
let currentPage = 1;

function render() {
  // 根据 currentFilter + currentPage 重新渲染
}
```

复杂原型（多组件联动、多视图状态）可以用简易观察者模式：

```js
const state = {
  data: [],
  filters: {},
  set(key, value) {
    this[key] = value;
    render();
  }
};
```

**禁止引入 Redux、Vuex、Pinia 等状态管理库。**

## 表单交互
- 回车键提交：`form` 内监听 `submit` 事件
- 输入防抖：搜索/筛选输入框使用 300ms debounce
- 校验反馈：错误信息显示在控件旁边的 `.form-error` 元素中

## 文件输出
- 导出文件使用 `Blob` + `URL.createObjectURL` + `<a download>`
- 不引入 `file-saver` 等第三方库
