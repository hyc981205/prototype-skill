# 设计兜底指南（无 frontend-design / ui-ux-pro-max 时用）

原 skill 默认调用两个设计 skill。目标 agent 没有它们时，按本文件执行，不降低交付质量标准。

---

## 一、视觉问题处理优先级

按顺序检查，前一项不合格不要往后做：

| 优先级 | 类别 | 关键检查 |
|---|---|---|
| 1 | 可访问性 | 正文对比度 ≥ 4.5:1；有可见焦点环；键盘可完成主流程 |
| 2 | 触摸与交互 | 可点区域 ≥ 44×44px；异步操作有加载反馈 |
| 3 | 性能 | 图片按需；预留尺寸防 CLS；不引入无必要的库 |
| 4 | 风格一致性 | 同一原型内圆角、边框、阴影、色阶只用一套 |
| 5 | 布局与响应式 | 桌面优先，768px / 480px 断点；无水平滚动 |
| 6 | 字体与颜色 | 正文 14–16px，行高 1.5；颜色全部走 CSS 变量 |
| 7 | 动画 | 150–300ms；尊重 `prefers-reduced-motion` |
| 8 | 表单与反馈 | 标签可见；错误就近显示；禁用态可辨识 |

---

## 二、四套默认视觉基调

选一套，写进 `DESIGN_SPEC.md` 的 3.3 节，然后只改 CSS 变量，不改组件结构。

### 1. 专业冷色（默认，信息密集后台）
```
--color-primary: #4F46E5;  --color-bg: #F9FAFB;  --color-surface: #FFFFFF;
--color-border: #E5E7EB;   --color-text: #111827; --color-text-secondary: #6B7280;
圆角 4/8/12px，阴影克制，边框清晰，表格行高紧凑
```

### 2. 工业深色（监控大屏、工控）
```
--color-primary: #38BDF8;  --color-bg: #0F172A;  --color-surface: #1E293B;
--color-border: #334155;   --color-text: #E2E8F0; --color-text-secondary: #94A3B8;
数字用等宽字体（ui-monospace）；状态色高饱和；微弱发光仅用于异常态
```

### 3. 轻量明亮（现代 SaaS）
```
--color-primary: #2563EB;  --color-bg: #FFFFFF;  --color-surface: #F8FAFC;
--color-border: #E2E8F0;   --color-text: #0F172A; --color-text-secondary: #64748B;
大留白，圆角 10–16px，阴影柔和，主按钮实心、次按钮描边
```

### 4. 高对比企业风（报表、审批）
```
--color-primary: #1D4ED8;  --color-bg: #F3F4F6;  --color-surface: #FFFFFF;
--color-border: #9CA3AF;   --color-text: #111827; --color-text-secondary: #4B5563;
线框明确，表头浅灰底，数字右对齐等宽，状态用色块 + 文字
```

---

## 三、写 UI 前的五个决定

1. **主区形态**：表格 / 卡片 / 树 / 看板 / 日历 / 表单 —— 由 `question-bank.md` 的选项决定，不自由发挥
2. **信息层级**：主操作一个、次操作若干、破坏性操作靠右并二次确认
3. **密度**：后台工具默认紧凑（行高 36–40px）；给领导看的看板默认舒展
4. **状态齐全**：空、加载、错误、无结果四种状态必须有
5. **图标**：用内联 SVG 或 Lucide CDN，不用 emoji 当功能图标

---

## 四、交付前自检

- [ ] 1280px 宽下无水平滚动
- [ ] 双击 `index.html` 可直接运行，无控制台报错
- [ ] 所有颜色来自 `:root` 变量
- [ ] 主流程可只用键盘完成
- [ ] 四种空状态至少实现两种（空数据、无搜索结果）
- [ ] 同一页面没有第二种圆角/阴影语言
- [ ] 中文文案无占位符残留（如「TODO」「示例文本」）
