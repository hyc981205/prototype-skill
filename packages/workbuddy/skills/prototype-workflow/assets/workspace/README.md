# 前端原型工作区

快速原型设计，每个工具一个独立目录，浏览器直接打开。

规范见 `prototype-workflow` 技能（`references/workspace-rules.md`）。

## 原型索引

每新建一个原型，在此表追加一行。

| 原型 | 目录 | 创建日期 | 说明 |
|------|------|----------|------|
| （示例）领料工具 | 领料工具 | YYYY-MM-DD | 为配置清单领用资源实体，支持库存与领用记录 |

## 新建原型

对 agent 说「新建一个 XXX 原型」，或按 `skills/01-scaffold.md` 手动操作：

```bash
cp -r _template/ 你的工具名/
```

然后按 `prototype-workflow` 技能的流程推进。
