# tech-report

一个给 Claude Code 用的技能，把**取证式技术报告**的写作纪律固定下来：每条断言绑定一手来源并标注可考证程度，不评优劣只描述决策，闭源对象一律写成时间戳化陈述。

它是从一个跨项目源码拆解项目里长出来的——那个项目要对比四个 Agent harness 的架构，其中三个开源、一个闭源，早期版本因为「只逐行读了一家、另外三家读的是文档」而得出过一条完全错误的核心论断。这个技能就是那次翻车之后总结的规矩。

## 它解决什么

用模型写技术调研报告时有三个反复出现的失败：

- **来源层级不对等**。读得最细的那个对象会被系统性地夸大差异，因为「我在别家文档里没看到」被当成了「别家没有」。
- **无证据的优劣评价**。「更优雅」「更好的设计」读起来像结论，实际上不可证伪，半年后自己都不敢引用。
- **范文腔**。「不是 A，而是 B」、排比、一段一个惊叹点——一眼就能看出是生成的文本。

技能对应给出三样东西：一张**来源层级表**规定每级材料允许下什么结论；一套**三问判据**（防什么失败 / 什么代价 / 什么前提）替代优劣评价；一组**文风约束**和润色优先级，明确规定顺口永远让位于事实准确。

## 装法

**项目级**（只对某个仓库生效）：

```bash
git clone https://github.com/hirovel/tech-report-skill .claude/skills/tech-report
```

**个人级**（对所有项目生效）：

```bash
git clone https://github.com/hirovel/tech-report-skill ~/.claude/skills/tech-report
```

装完重启 Claude Code。这个技能是模型自动触发的：起草或润色技术报告正文、写源码取证底稿、下「某个项目有/没有某设计」这类跨项目断言、引用闭源产品或二手拆解材料时会自己加载。也可以直接输入 `/tech-report` 手动调用。

## 文件

| 文件 | 内容 |
|---|---|
| `SKILL.md` | 三问判据、写一节的五步顺序、文风、命名、材料仓库维护 |
| `evidence.md` | 来源层级表、时间戳化陈述、声誉断言的四道门、标注粒度 |

`evidence.md` 是按需加载的：只在真正要给材料定级、或要下一条涉及项目声誉的断言时才会被读进上下文。

## 用之前要知道的

这套规矩是**为一类特定写作**定的：对多个真实系统做源码级拆解、并且作者半年后还要回来接着写。它对这类写作管用，代价是别的场合会显得过紧——

- **产出会变慢**。「先立预期再核对」和「存疑一节不许为空」都在强制多做一轮，赶稿场景下这是纯成本。
- **不评优劣会让一部分读者不满**。想看「所以到底哪个好」的读者拿不到答案，技能明确要求把判断留给读者。
- **它不替你查证**。规矩只约束怎么写，材料还得自己读；技能没法阻止一个编造出来的 `路径:行号`。

三条硬约束不建议改：不读源码不写「某家没有」、闭源对象带时间戳、涉及声誉的断言主线亲手核。前两条防的是把无知写成事实，第三条防的是署真名发布时的名誉风险。

## 改成自己的

`SKILL.md` 的三问判据、五步顺序、材料仓库维护是通用的。要换领域，通常只改两处：`evidence.md` 的来源层级表（换成你这个领域的材料类型和可考证程度），以及命名那张对照表（换成你自己的口味示例）。

frontmatter 的 `description` 决定它什么时候被自动触发，改动时把触发场景写全，一个场景写一条，别写同义词。

## License

MIT

---

**English**: A Claude Code skill enforcing forensic discipline for technical research reports — every claim bound to a primary source with an explicit evidence tier, no unfalsifiable "better design" judgments (replaced by three checkable questions: what failure does it prevent / what does it cost / what does it presuppose), and timestamped statements for closed-source subjects whose current implementation cannot be verified. Extracted from a four-way source-level teardown of Agent harnesses after an early draft's central claim turned out to be wrong — the result of reading one subject's source line by line and the other three's docs only.
