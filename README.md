# Embodied Paper Writing

**面向具身智能顶会顶刊的论文撰写 Skill**

把研究材料组织成一条可核验的论证链：**研究问题 → 机制与假设 → 方法设计 → 实验证据 → 有边界的结论**。

支持 CVPR、ICLR、ICML、AAAI 等会议，以及机器人学习与具身智能相关会刊的构思、实验补强、逐节写作、改稿、投稿审查与审稿回复。默认中文讨论、英文论文正文，也可按需输出中文、Markdown 或现有 LaTeX 工程。

An evidence-grounded writing skill for embodied AI and robotics research, from paper framing and experimental reasoning to manuscript revision and reviewer responses.

**技能名称：** `embodied-paper-writing` · **形式：** 指令与参考资源 · **运行依赖：** 无专用服务、API Key 或 Python 包 · **许可：** [MIT](LICENSE)

[快速开始](#快速开始) · [安装](#安装) · [任务模式](#七种任务模式) · [会刊覆盖](#会刊覆盖) · [目录](#仓库结构) · [完整使用说明](skills/embodied-paper-writing/references/usage.md)

## 为什么使用它

论文写作需要把研究判断、方法设计与实验论证连接起来。这个 skill 帮助作者回答：问题究竟是什么，技术差异在哪里，哪些结果支持主张，哪些解释仍需检验。

- **先确定主张。** 从现有材料提炼可检验的研究问题，区分观察、假设与已验证结论。
- **叙事服务证据。** 按工作类型选择机制诊断、假设重审、理论解释等主线，避免只罗列模块与指标。
- **针对具身任务。** 检查闭环控制、数据划分、动作接口、真机/模拟、干预与统计口径。
- **覆盖完整写作阶段。** 支持从想法到全文，也支持单节改写、实验设计、rebuttal 和期刊返修。
- **按当期规则适配。** 将写作建议与官方投稿要求分开，核对目标年份、track 和阶段。

它适用于 VLA、机器人基础模型、世界模型、操作、导航、长时程交互、多本体学习，以及相关的视觉表示、规划、控制和评测研究。单篇论文精读和一般文案写作不属于主要用途。

## 快速开始

安装后，在支持本技能的 Codex 会话中输入：

```text
使用 $embodied-paper-writing。

目标：CoRL，年份与 track 待定。
阶段：已有方法和部分实验结果。
材料：./notes/method.md、./results/summary.csv、./results/protocol.md。
任务：提炼论文主线，撰写英文 Introduction，并给出优先补实验清单。
要求：结果必须来自材料；未完成实验写成计划，不补造数字或引用。
输出：./paper/introduction.md，以及独立的证据缺口记录。
```

上面的路径是示例，请替换为自己的文件。第一次使用只需要提供研究想法、方法说明或稿件中的一项，并说清本次想得到什么；不必先准备完整论文。

若还没有实验结果，可以这样开始：

```text
使用 $embodied-paper-writing，帮我构思一篇具身智能论文。
我观察到：加入历史图像后，长时程操作有所改善，但换对象时仍失败。
这只是初步观察，还没有受控实验。
请提出可区分的解释、可否定它们的实验和论文提纲，
明确当前能够主张什么，不要生成已经完成实验的结果段。
```

## 安装

### 方式一：使用 Skill Installer

在具备 `$skill-installer` 的 Codex 环境中输入：

```text
请使用 $skill-installer 安装这个 GitHub 技能目录：
https://github.com/JOKER-rf/embodied-paper-writing/tree/main/skills/embodied-paper-writing
```

如果当前客户端未显示新技能，重新启动客户端或开启新会话后再选择。

### 方式二：手动安装为个人技能

先下载仓库 ZIP，或克隆：

```bash
git clone https://github.com/JOKER-rf/embodied-paper-writing.git
cd embodied-paper-writing
```

将 **`skills/embodied-paper-writing` 整个目录**复制到客户端支持的个人技能目录。当前官方本地发现说明列出的个人位置是 `~/.agents/skills/`，项目位置是 `.agents/skills/`；以你使用的客户端版本为准。参见 [OpenAI 官方技能文档](https://learn.chatgpt.com/docs/build-skills)。

**Windows PowerShell**，在克隆后的仓库根目录运行：

```powershell
$skillRoot = Join-Path $env:USERPROFILE '.agents\skills'
$skillDestination = Join-Path $skillRoot 'embodied-paper-writing'
if (Test-Path -LiteralPath $skillDestination) {
    throw '技能目录已存在，请先检查已有版本。'
}
New-Item -ItemType Directory -Path $skillRoot -Force | Out-Null
Copy-Item -LiteralPath '.\skills\embodied-paper-writing' -Destination $skillDestination -Recurse
```

**macOS / Linux**，在克隆后的仓库根目录运行：

```bash
skill_destination="$HOME/.agents/skills/embodied-paper-writing"
if [ -e "$skill_destination" ]; then
  echo "技能目录已存在，请先检查已有版本。" >&2
  exit 1
fi
mkdir -p "$HOME/.agents/skills"
cp -R ./skills/embodied-paper-writing "$skill_destination"
```

如果你的桌面环境已通过 `$CODEX_HOME/skills/` 或 `~/.codex/skills/` 管理个人技能，可以沿用该环境已识别的位置。不要在多个扫描位置重复安装同名技能。

### 方式三：仅用于某个研究项目

把 `skills/embodied-paper-writing` 复制到研究项目的 `.agents/skills/embodied-paper-writing/`。安装后的入口应为：

```text
your-research-project/
└── .agents/
    └── skills/
        └── embodied-paper-writing/
            ├── SKILL.md
            ├── agents/
            ├── references/
            └── assets/
```

也可以不安装，在会话中提供本仓库 `skills/embodied-paper-writing/SKILL.md` 的绝对路径，并要求遵循其中流程。不要只复制 `SKILL.md`，它会按需读取同目录下的参考文件与模板。

### 能力与依赖

技能本体由 Markdown 和 YAML 构成，不运行后台服务，也不自动执行训练或机器人操作。读取 PDF、分析日志、检索文献、编辑 Word 或编译 LaTeX，使用当前客户端提供的工具与权限。

无法联网时仍可基于已有材料写作，引用与投稿规则保持待核验状态。其他支持 `SKILL.md` 的宿主可参考本目录结构接入；本仓库的说明主要面向 Codex，不声明已经验证所有客户端。

## 七种任务模式

| 模式 | 适合的材料 | 主要产物 |
| --- | --- | --- |
| 构思与定位 | 想法、初步观察、方法笔记 | 一句话主张、竞争解释、论文主线与提纲 |
| 全文或分节撰写 | 方法、协议、结果、参考文献 | 可编辑正文及证据缺口 |
| 重构与润色 | 现有稿件、需要保留的事实 | 修订稿及实质性主张变化说明 |
| 实验补强 | 核心论点、现有实验和预算 | 对照、消融、泛化与反证方案 |
| 投稿前审查 | 稿件、日志、目标年份/track | 有位置、依据和修复方式的问题清单 |
| Rebuttal 与返修 | 审稿意见、原稿、已完成新结果 | 逐条回应矩阵、英文回复与改动记录 |
| 会刊适配与转投 | 现有稿件、历史版本、目标会刊 | 定位、结构与证据调整方案 |

以下提示词可直接修改使用；更详细的七组示例见 [完整使用说明](skills/embodied-paper-writing/references/usage.md)。

**从材料写正文**

```text
使用 $embodied-paper-writing，基于 ./notes/method.md 和 ./results/
撰写英文 Abstract、Introduction 和 Method。
目标会刊及年份见 ./brief.md。延续 ./paper/main.tex 的术语和引用键。
没有证据的结果保留明确占位符；正文之外记录仍需补充的信息。
```

**重构已有稿件**

```text
使用 $embodied-paper-writing，直接修改 ./paper/intro.tex。
把模块罗列改为问题、机制与设计的论证链。
保留技术事实、数值、符号与引用键，不修改其他章节。
若必须收窄主张，请在正文外说明依据。
```

**设计实验与消融**

```text
使用 $embodied-paper-writing，检查 ./paper/draft.md 的实验论证。
核心主张是未见对象上的闭环操作改善，预算见 ./results/budget.md。
按影响核心结论、增强机制解释、可选展示排序补实验。
每项列出待检验论点、对照、控制变量、指标和否定条件。
```

**撰写审稿回复**

```text
使用 $embodied-paper-writing，根据 ./reviews/reviews.md 写英文回复。
原稿与回复限制见 ./reviews/constraints.md，新增结果见 ./results/new.md。
先建立意见、证据和修改位置的对应关系，再给回复草稿。
已完成、计划和无法完成的事项分别表述。
```

## 六类叙事路径

叙事框架受文末所列文章启发。这里将其转化为研究写作中的选择与检验，不要求每篇论文套用全部路径。

| 路径 | 要回答的问题 | 所需证据 |
| --- | --- | --- |
| 机制诊断 | 现有方法为什么在指定条件下失败？ | 区分竞争解释的受控实验 |
| 默认假设重审 | 一个常用设定在哪些条件下不必要或有代价？ | 替代方案、同预算比较与适用边界 |
| 理论解释 | 哪个形式化结论能解释或预测经验现象？ | 明确假设、有效证明和对应检验 |
| 诊断基准 | 现有评测漏测了哪种重要能力？ | 操作性定义、指标效度与防泄漏设计 |
| 真实效用 | 系统改善了哪些实际需求与约束？ | 场景指标、成本、干预与失效信息 |
| 统一机制 | 共用机制如何承担原先分离的功能？ | 共享接口定义、跨设置结果与负迁移 |

选择已有证据最强的一条作为主线。缺少机制诊断时保留假设措辞；没有可信理论时不强行增加定理；只在指定条件下有效时直接写清范围。

详见 [来源与叙事方法](skills/embodied-paper-writing/references/source-methodology.md)。

## 具身论文的专门检查

- **数据与泛化：** 轨迹、对象、场景、语言、动力学、本体的划分是否清楚；预训练可见性是否已知。
- **观测与动作：** 传感器、历史窗口、动作空间、坐标、单位、归一化与夹爪接口。
- **闭环控制：** 策略与底层控制频率、动作块、重规划、延迟、外部规划器与人工辅助。
- **真实统计：** 成功判据、成功数/分母、独立重复、任务聚合、误差线、重试与排除规则。
- **真机与模拟：** 区分离线预测、模拟闭环、真机闭环，以及适配、标定和测试数据。
- **方向特性：** VLA 的训练与动作接口、世界模型的预测与规划收益、导航的地图与定位、接触操作的失效与恢复。

检查项目按论文主张选择，不要求所有工作都进行真机或跨本体实验。纯理论、基准和数据论文采用相应证据结构。

详见 [具身评测指南](skills/embodied-paper-writing/references/embodied-evaluation.md)。

## 会刊覆盖

| 类别 | 会刊 |
| --- | --- |
| 视觉与通用 AI / ML 会议 | CVPR、ICLR、ICML、AAAI、NeurIPS、ECCV |
| 机器人与机器人学习会议 | CoRL、RSS、ICRA、IROS |
| 机器人期刊 | IEEE T-RO、IEEE RA-L、IJRR |
| 视觉与机器学习期刊 | TPAMI、JMLR、TMLR |

覆盖表示提供贡献定位、证据组织与官方入口，具体论文仍需符合目标 scope。指南核对年份为 2026；未来投稿应重新核验目标届次、track 和初投/回复/终稿阶段。页数、截止时间、匿名、补充材料和 AI 使用政策不作为永久规则写死。

详见 [会刊适配与规则核验](skills/embodied-paper-writing/references/venues.md)。

## 材料与交付

### 推荐提供的材料

目标会刊、年份和 track；本次任务与允许修改的范围；研究想法或方法；稿件；数据与评测协议；结果日志和汇总；最近邻文献；已有发表/在审版本与实际预算。

已有材料可以直接给路径。可选模板：

- [研究简报](skills/embodied-paper-writing/assets/paper-brief.md)：整理问题、方法、材料与任务范围。
- [事实与证据记录](skills/embodied-paper-writing/assets/evidence-ledger.md)：连接主张、实验、来源、引用和投稿规则。
- [论文骨架](skills/embodied-paper-writing/assets/manuscript-outline.md)：按真实内容填充并调整章节。

### 你会得到什么

以实际任务为准：正文、修改后的文件、实验方案、审查报告或审稿回复。完整论文任务通常另附核心主张、证据缺口和引用/规则核验状态；局部改句只提供相关说明。

证据记录会区分：**已从材料核验 / 作者报告未核验 / 解释性假设 / 计划实验 / 缺失**。这有助于作者判断当前哪些内容已能写入结论，哪些仍需补强。

## 证据与使用边界

不编造结果、引用、定理、硬件测试或审批；不把相关性升级为因果，不把模拟或精选视频写成完整真机验证，不把未见微调样本等同于基础模型从未见过。

SOTA、首次、通用、可靠等主张需有明确比较范围和证据。叙事不能替代科学贡献，本技能不保证录用、Oral 或奖项。论文作者应核对最终稿的事实、引用、伦理和投稿要求。

本技能完成本地写作与核验；投稿、联系编辑、公开研究数据、上传保密稿件和真实机器人实验需有对应的用户授权。

## 仓库结构

```text
embodied-paper-writing/
├── README.md                         # GitHub 主页与快速开始
├── LICENSE                           # 原创仓库文件的 MIT 许可
├── NOTICE.md                         # 来源与第三方内容说明
├── CONTRIBUTING.md                   # 维护与贡献约定
├── .gitignore
├── .gitattributes
└── skills/
    └── embodied-paper-writing/        # 安装时复制这个完整目录
        ├── SKILL.md                   # 技能入口、流程与边界
        ├── agents/
        │   └── openai.yaml            # 界面元数据与默认调用提示
        ├── assets/
        │   ├── paper-brief.md
        │   ├── evidence-ledger.md
        │   └── manuscript-outline.md
        └── references/
            ├── usage.md              # 七种模式与完整使用说明
            ├── source-methodology.md # 来源与六类叙事的操作化
            ├── writing-guide.md      # 分节写作、引用与稿件重构
            ├── embodied-evaluation.md
            ├── venues.md
            └── review-rebuttal.md
```

## 常见问题

**只有想法，没有实验结果，能用吗？**

可以。先形成研究问题、可区分假设和实验方案，也可以写问题与方法草稿；结果部分保持待补状态。

**会自动训练模型或运行真机吗？**

不会。实验设计模式交付方案，实际执行需要单独的任务范围和授权。

**可以只润色一段英文吗？**

可以。明确“仅语言修改”，技能会保留技术事实、措辞强度、符号与引用，不扩展成整篇论文重构。

**会自动检索并核验参考文献吗？**

取决于当前环境的联网工具和任务范围。可核验时使用原论文与官方出处；无法核验时记录缺口，不生成看似真实的 BibTeX。

**如何更新？**

更新本仓库后，比较已安装目录与 `skills/embodied-paper-writing/`，保留个人改动再同步。若客户端没有发现变化，重新加载或重启。不要并存多个同名版本。

**是否是会刊官方工具或官方投稿指南？**

不是。它是研究写作辅助技能，官方政策以目标会刊当期页面为准。

## 来源与致谢

叙事框架的启发来源为人工智能学社的文章：[《Story Is All You Need：扒透35篇顶会最佳/杰出论文的叙事密码，我们找到了六大范式》](https://mp.weixin.qq.com/s/mnU8PE-6DZ38rJqHeOiSfQ)，页面发布时间为 2026-10-03。

本仓库对方法进行了独立提炼与流程化改写，补充了具身评测、分节写作、引用核验和会刊适配。原文的奖项归属、投稿统计及性能数字未在本项目中逐项核验，也未作为生成论文的事实库。原文全文、图片和论文图表不包含在仓库中。

技能结构与安装说明参考 [OpenAI 官方技能文档](https://learn.chatgpt.com/docs/build-skills)。会刊官方链接与核验边界记录在 [venues.md](skills/embodied-paper-writing/references/venues.md)。

## 贡献与许可

欢迎提交更准确的会刊官方入口、具身评测建议和经过实际使用验证的流程改进。请附上问题场景、依据和适用范围，参见 [贡献说明](CONTRIBUTING.md)。

本仓库原创文件使用 [MIT License](LICENSE)。所引用文章、论文、模板和外部网站内容的权利归各自权利人所有；MIT 许可不覆盖它们。详见 [NOTICE.md](NOTICE.md)。
