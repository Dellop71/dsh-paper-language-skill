# paper-language · DSH 论文写作 skill

给 [DeepSeek Harness](https://github.com/Dellop71)（DSH）用的论文写作 skill：把"你自己的语料怎么说"变成模型写论文时的可执行流程。

配套插件：[dsh-paper-language](https://github.com/Dellop71/dsh-paper-language) —— 插件负责把规则**常驻注入系统提示**（每一轮都在），本 skill 负责长文写作任务里的**流程**（读规则 → 查语料 → 写 → lint）。

## 它解决什么

DSH 默认的学术写作会滑向通用 AI 腔：`It is worth noting that…`、`plays a crucial role`、`值得注意的是`、`具有重要的理论意义和实践价值`。这些在你的目标期刊语料里是 **0 次**出现的。本 skill 用统计基线把表达拉回本领域作者的实际写法：

| 指标 | 英文 | 中文 |
|---|---|---|
| 句长中位数 / p90 | 24 / 38 词 | 59 / 100 字 |
| 高频判断词 | indicate, suggest, demonstrate | 表明(425), 说明(109), 结果表明(98) |
| 高频转折 | In contrast, However, Compared with | 然而, 此外, 因此, 与…相比 |

## 安装

```bash
# 1) skill
cp -r paper-language ~/.dsh/skills/          # Windows: %USERPROFILE%\.dsh\skills\

# 2) 配套插件（否则 skill 里的脚本路径不存在）
git clone https://github.com/Dellop71/dsh-paper-language.git
cd dsh-paper-language
python scripts/build_corpus.py --sci "<英文PDF目录>" --zh "<中文PDF目录>"
node scripts/install.mjs --check             # 体检
```

## 内容

- `SKILL.md` —— 触发条件、三个固定动作、写作硬约束、输出约定
- `manifest.yaml` —— DSH skill 清单

规则全文、句式骨架、禁用表在插件的 `rules/` 下；语料检索与 lint 是插件的两个 CLI。

## 用法

装上之后不需要记命令，直接说"帮我写/改这段论文"，skill 会自己走流程。也可以显式：

```
/paper-rules 结果          # 只看"结果"一节的句式骨架
/paper-corpus increased yield --fn result
/paper-lint @draft.md
```

## License

MIT
