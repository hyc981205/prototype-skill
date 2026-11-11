---
name: prototype-design-interview
description: >
  Guide a user from a vague idea to a buildable prototype design through structured
  interview questioning, then emit DESIGN_SPEC.md + PROMPT.md and hand off to
  prototype-workflow. Use this skill whenever the user wants a prototype but cannot
  yet describe it — e.g. "我想做个原型但没想清楚"、"不知道要做什么样的界面"、
  "帮我理一下需求"、"从零设计一个工具"、"需求还很模糊"、"你问我答"、
  "你引导我"、"我不知道怎么描述"、or any new-prototype request where the user
  cannot state layout, columns, fields, or interactions. Also use when the user asks
  to be interviewed, guided, or asked questions about a tool idea, even if they never
  say the word 原型.
  Do NOT use when the user already has a complete spec and just wants code written
  (that is prototype-workflow's job), or when modifying an existing prototype.
---

# prototype-design-interview

Turns a blank mind into a confirmed design. Runs **before** `prototype-workflow`, which
then builds the prototype from the artifacts this skill produces.

## Why this skill exists

The execution layer (`prototype-workflow`, `prototypes/CLAUDE.md`) assumes the user can
already state layout, content, and fields. For someone who has not thought it through,
that assumption is the whole problem: they cannot describe a layout they have never
imagined, so the prompt quality collapses and the prototype comes back wrong — expensive
to fix, because by then there is code to rework.

The fix is not a better prompt template. It is to **elicit the design through the question
form**, so the user never has to generate a description from nothing.

## Three mechanisms (the entire point of this skill)

Everything below is downstream of these. If you remember nothing else, remember these.

**1. 问现状，不问愿景。**
A blank-mind user cannot answer "你想要什么界面", but can answer "你现在这件事怎么做".
The current workaround — an Excel file, a ticket system, a whiteboard, three people
messaging each other — already encodes most design decisions: column names, field types,
process order, classification dimensions, exception cases. Those are free requirements.
Mine the status quo and transcribe it into the prototype; that is an order of magnitude
easier for the user than inventing a design. When you must ask about the future, anchor it
to the present first ("现在筛的时候是手填还是下拉？").

**2. 具象选择替代抽象描述。**
Nobody says "左右分栏，侧栏固定 280px，主区树表". Everybody can point at a sketch and say
"像这个". So every question about **shape** — layout, main-area form, density, style —
offers 2–4 concrete options, ideally with an ASCII sketch, plus room to say "都不是".
Reserve open questions for things the user genuinely owns: the business, the vocabulary,
the exceptions. Never ask an open question about a shape.

**3. 复述纠错替代从零生成。**
Editing is cheaper than authoring. So: infer before asking (if the answer follows from what
the user already said, state it as a default with a reason, and let them correct it); and
close every round with a 回声确认 — a short restatement of your understanding, in their
words, inviting correction. "我理解是……对吗？" costs the user three seconds and catches most
drift while it is still cheap.

Corollary: **"我不知道" is a valid and common answer.** When it comes, do not rephrase the
question. Pick the recommended default, say why in one line, mark it as an assumption, and
move on. Grinding on an unanswerable question is how these interviews die.

## Interview shape

Three rounds, 宏观 → 细节. Macro is one question at a time (the user is still locating the
problem); details come in batches (the problem is fixed, only facts remain).

Stop condition — do not run the rounds to completion for their own sake. You are done when
every layer of the spec has a concrete value and the user can point at the whole thing and
say "对，就是这样". Asking all the questions is not the same as having enough.

### Round 1 — 意图层（一次一问）

Goal: know what moment this tool lives in, and mine the status quo.

1. **使用时刻** — 什么时刻你会打开它？不是"它有什么功能"，是"上一秒发生了什么，让你想打开它"。
   时刻锚定用法，比功能清单好答得多。
2. **使用者** — 只有你自己，还是同事/团队/给领导看？多人会改变信息密度、术语水平、是否需要角色区分。
3. **现状** — 这件事现在怎么做的？把现有的表、流程、字段尽量问出来。**这一题的产出是后面所有细节题的主要来源。**
4. **痛点** — 现在最烦的是哪一步？决定主界面的重心，也决定什么可以先不做。
5. **成功标准** — 做完之后，哪一件事变快了/不用做了，你就觉得它有用？

若用户在第 3 题给出了丰富的现状材料，第 4、5 题常可合并为一句确认。

### Round 2 — 结构与任务层（一轮 3–4 问）

Goal: turn the status quo into a component layout.

1. **核心动作** — 进来第一件事做什么？最常做的三个动作是什么？（要动词，不要名词）
   每个动词大致对应一个 UI 入口，这是功能分区的依据。
2. **业务对象** — 别问"领域模型"。让他举一句话例子："比如：我一个项目下面有 5 个任务，每个任务派给一个人。"
   一句话例子直接给出实体、字段和基数关系。
3. **数据来源** — 真实系统导出？手填？从别处复制粘贴？决定要不要导入/粘贴/表单入口。
4. **布局方案** — 给整图草图选，见 `references/question-bank.md` 的布局草图区（用 AskUserQuestion 的 `preview` 字段并排展示效果最好）。

### Round 3 — 细节层（一轮 3–4 问）

Goal: fill in the main area, the branches, the look, and the boundary.

1. **主区内容** — 不要问"要哪些列"。问："把你现在那个表的表头念给我" 或 "你希望一行里看到哪几个信息"。
2. **状态与分支** — 空数据、加载中、失败、二次确认、批量操作、审批通过/驳回，哪些是真需要的。默认全给，让用户删，比让用户想更省力。
3. **视觉基调** — 2–3 个具象选项 + 类比，见 question-bank。
4. **明确不做** — 哪些这次不做？**必须问。** 原型膨胀是最大的成本来源，而用户几乎从不主动划边界。

### 收尾

生成 `DESIGN_SPEC.md` 和 `PROMPT.md`（模板见 `references/output-templates.md`），写入
`prototypes/<原型名>/`（目录不存在则新建，不要碰代码文件）。做最后一次整体回声确认，
然后调 `prototype-workflow` 交接。

## 自适应

引导的密度由用户的清晰度决定，不由流程决定。

- **用户其实很清楚** — 若开场就给出了明确的功能和形态，跳过对应轮次，直接进规格生成，只补真正的缺口。走完流程不是目的。
- **用户几乎全程"不知道"** — 不要硬问。改走「模板场景」路径：从 question-bank 的常见工具形态里挑 2–3 个最接近的，给用户选一个当骨架，再在骨架上改。空白用户改骨架比从零设计容易得多。
- **用户话很少、只答关键词** — 缩短问题，改成二选一或"是/不是 + 我猜是 X"。
- **用户话很多、跑题** — 先让他说完（材料宝贵），再收敛成一句回声确认。

## 常见失败模式

- **连问 8 个开放题** — 用户答不出来，访谈崩掉。形状题必须给选项。
- **问"你想要什么功能"** — 空白用户答不了。改问"你现在怎么做"。
- **把引导做成审查** — 用户说"不知道"时反复重问同一题。给默认值，往下走。
- **跳过"明确不做"** — 原型必然膨胀，返工成本最高的一项。
- **问完不停，把问题问满** — 问满 ≠ 问够。每轮末做回声确认，够了就收敛。
- **重复 prototype-workflow 的规则** — 那个 skill 管流程、规范、Git、文档同步。这里只管把设计问清楚到能开工。两套规则重复会互相打架。
- **直接开始写代码** — 本 skill 不产出代码。产物是规格 + 提示词，然后交接。

## 文件索引

- `references/question-bank.md` — 各层的问题、选项、ASCII 布局草图、常见工具形态。Round 2/3 提问前读。
- `references/output-templates.md` — `DESIGN_SPEC.md` 与 `PROMPT.md` 的模板。收尾时读。
