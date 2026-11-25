---
name: paper-language
description: "Write, revise, or polish academic manuscript prose (English or Chinese) so it matches the author's own SCI / Chinese-core corpus instead of generic AI style. Use for abstracts, introductions, methods, results, discussions, conclusions, reviewer responses, cover letters, and for auditing existing drafts for AI-ish phrasing, over-claiming, and Chinglish."
---

# 论文语言契约（Paper Language Contract）

本 skill 与插件 `dsh-paper-language` 配套：插件负责把规则常驻注入系统提示，本文件负责在具体写作任务里给出可执行流程。

路径约定：下文 `<PLUGIN>` = `$DSH_HOME/plugins/dsh-paper-language`。
`DSH_HOME` 默认 `~/.dsh`；Windows 上通常是 `C:\Users\<用户名>\.dsh`。动手前先确认一次实际路径。

## 何时用

- 用户要写 / 改 / 润色论文正文（中英文均可），或要"去 AI 味"。
- 用户给了草稿、段落、审稿意见回复，要求"像我自己写的"。
- 用户问某句话该怎么写才符合本领域表达。

## 三个固定动作

1. **起草前读规则**：`<PLUGIN>/rules/paper-language.md`
   - 句式骨架：`<PLUGIN>/rules/frames.md`
   - 禁用/限用表（带语料次数）：`<PLUGIN>/rules/banned.md`
   - **木霉 / 生防方向先读**：`<PLUGIN>/rules/domain-biocontrol.md`（193 篇专题语料，11 930 句；筛选鉴定、拮抗、定殖、促生、防效、发酵优化、机理各有成型句式，同一功能下优先于通用骨架）
2. **拿不准就查语料**（语料由 `scripts/build_corpus.py` 从用户自己的 PDF 生成）：
   ```
   node "<PLUGIN>/scripts/corpus_search.mjs" "increased yield" --lang en --fn result
   node "<PLUGIN>/scripts/corpus_search.mjs" "微生物 群落" --lang zh
   ```
   功能标签：background / gap / objective / method / result / interpretation / comparison / limitation / conclusion
   若语料为空，先跑：`python "<PLUGIN>/scripts/build_corpus.py" --sci <英文PDF目录> --zh <中文PDF目录>`
3. **交付前跑 lint**：
   ```
   node "<PLUGIN>/scripts/lint.mjs" --file <草稿路径> --lang auto
   ```
   HIGH 清零；MEDIUM 逐条判断并在回复里说明保留理由。退出码 1 表示仍有 HIGH。

## 写作时的硬约束（摘要）

1. 一句一主张，主语是被测对象，不是"研究/本文/我们"。
2. 先数据后判断：对象 + 变化 + 幅度与单位 + 对照基准，再至多一句解释。
3. 图表引用只放括号里（…（图1）），紧跟被支撑的陈述；不写"见图1""如图1所示""shown in Fig. 1"。
4. 不用"就……而言／就……来说"；直接以该对象作主语。英文不用 as for / in terms of。
5. 正文不用冒号（题注与版面字段除外）。
6. 判断词按证据取最弱一档：demonstrate/show > indicate > suggest > imply > may be associated with；相关不写成因果。
7. 转折词闭合集：However / Moreover / In addition / In contrast / Conversely / Therefore / Thus / Collectively；中文：然而 / 此外 / 同时 / 因此 / 综上。
8. 每个 significant/显著 必须带检验与水平。
9. 句长守线：英文中位数 24 词、超 45 词必拆；中文中位数 59 字、超 120 字必拆。
10. 禁用"写作动作"与自我评价；不写防御性句子。
11. 不表演结构、不做对举：不写"沿两条线索展开/分为两支/成效一侧…分配一侧/共享一个前提/第一…第二"，不写"不是X而是Y/不以A而以B/以A衡量不以B衡量/与其说…不如说"。只写成立的那个判断。
12. 不改数字、单位、统计量、引用、图号、否定、因果强度、适用范围。

## 输出约定

- 直接给可用的正文，不给"改写建议清单"（除非用户只要建议）。
- 改写只做 CUT → PRUNE → RECAST → SPLIT，不做同义替换式的"润色"。
- 若某处必须超出规则（例如期刊要求被动语态、或作者指定的固定表述），在回复末尾用一行说明理由。
- 不得因为风格而削弱或加强科学主张；需要更强主张时，先要求补证据。
