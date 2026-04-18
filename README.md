# Classical Grounded Theory AI-Assisted Research Toolkit

一个用于在 AI 协作下开展经典扎根理论（Classical / Glaserian Grounded Theory, CGT）研究的文档与脚本框架。本工具包不提供现成的可执行程序——它提供的是一整套**约束 AI 行为的规则**、**供 AI 检索的方法论知识**，以及**研究者可据以自行开发 Python 脚本的结构与算法示例**。

---

## 0. 这个工具包解决什么问题

扎根理论被 Suddaby（2006）和 Shah & Corley（2006）称为社会科学中使用最广、误解最深的方法论之一。当研究者借助 AI 协作开展扎根理论研究时，误用的风险进一步放大——AI 在默认状态下会倾向于"把事情做齐全、做整齐、做得像一份合格的定性报告"，这恰恰与 CGT"耐心等待浮现、只捕捉研究对象主要关切"的气质相反。本工具包的目的是在 AI 的默认倾向与 CGT 的方法论承诺之间建立一道硬约束，同时不剥夺 AI 在事务性工作上为研究者节约时间的能力。

工具包由三份 Markdown 文档（SKILL、REFERENCE、README）和一个由研究者根据需求自行编写的 Python 脚本集合组成。三份文档均不包含可执行代码；脚本层完全由研究者按本 README 的算法示例自行编写，脚本仅负责事务性工作，不介入任何分析判断。

---

## 1. 三份文档的分工

**`classical_grounded_theory_SKILL.md`** 是规范 AI 行为的主文档。它以"应该/禁止"清单的形式给出强约束，包括六大核心要素、六大研究程序、四大评判标准、中心范畴的十一项判据、九类 AI 协作中的红旗、以及当用户与 SKILL 冲突时的处理顺序。当您在 AI 对话中声明使用经典扎根理论，SKILL 即对 AI 生效。SKILL 的设计目标是：让 AI 在每一轮回应前都可以对照清单自检，避免"帮过头"。

**`classical_grounded_theory_REFERENCE.md`** 是 SKILL 的配套知识库。它收录 SKILL 中点名但未展开的外部信息——十八个理论性编码家族的完整清单、核心范畴十一判据的英文原文与中文要义、不断比较四层级的展开、数据切片、概念—指标模型、实质理论与形式理论的区分、理论性整理的十一项规则、备忘录可排序性的六条写作规则、采样术语对照、CGT 与程序化版本的差异对照、以及中英术语对照表。REFERENCE 的设计目标是：当 AI 或研究者需要展开、引证、解释时，有一份可检索、可追溯的原典性参考。

**本 `README.md`** 是整个工具包的入口与研究者的操作指南。它说明 SKILL 与 REFERENCE 如何被 AI 使用，给出项目目录结构，界定 Python 脚本可以做什么、不能做什么，并为八类可自动化任务提供核心算法示例。README 不提供完整脚本——每项脚本应由研究者根据自己的研究规模、资料类型、技术熟练度自行编写。

三份文档之间的关系是层级嵌套的：README 说明工具包如何被使用，指向 SKILL；SKILL 规定 AI 的行为边界，指向 REFERENCE；REFERENCE 是方法论知识的基础层。任何一份的更新都应同步核对另两份。

---

## 2. 如何向 AI 出示这些文件

推荐做法是在 Claude 的 Project（或同类工具的项目知识库）中把 SKILL、REFERENCE 以及研究者本人的一两份关键原著扫描件（例如 Glaser 1978、Glaser 1992）一并上传，并在 Project 的 Custom Instructions 中加入如下触发语："本项目采用经典扎根理论（Classical / Glaserian Grounded Theory）方法论。在协助任何研究相关任务前，请遵循 `classical_grounded_theory_SKILL.md` 中的全部约束；如需展开方法论细节或引证，请检索 `classical_grounded_theory_REFERENCE.md`。若用户的请求与 SKILL 中的禁止条款冲突，请按 SKILL §10 的顺序处理。"

对话级别的上传也可以，但需要每次对话都重新上传两份文档，会消耗 context window。若 AI 工具支持文件检索（retrieval），项目级上传是更稳健的做法。

研究者本人应在开展研究前通读 SKILL 与 REFERENCE 至少一次。SKILL 的 §5（应该）与 §6（禁止）建议打印出来贴在工作台前；REFERENCE 的 §1（十八个编码家族）与 §3（核心范畴判据）建议在编码阶段每日参照。

---

## 3. 建议的项目结构

以下目录结构遵循"原始资料只读、处理后资料分层、输出可重建、脚本各司其职"的原则，既符合质性研究的工作流，也便于版本控制与毕业答辩时的资料追溯。

```
my-cgt-project/
├── README.md                      # 本项目的研究者自述（不同于本工具包的 README）
├── classical_grounded_theory_SKILL.md
├── classical_grounded_theory_REFERENCE.md
├── config.yaml                    # 路径、命名规范、研究元信息
├── .env                           # API 密钥等敏感配置，不入库
├── requirements.txt               # Python 依赖锁定版本
│
├── data/
│   ├── raw/                       # 原始资料，只读
│   │   ├── interviews/            # 访谈原件（音频、纸笔笔记扫描、逐字稿）
│   │   ├── observations/          # 田野观察笔记
│   │   └── secondary/             # 二手资料（网络文本、文献引文）
│   └── processed/                 # 可重建的处理产物
│       ├── transcripts/           # 转录稿（Markdown + YAML front matter）
│       ├── codes.db               # 或 codes/ 目录下的结构化 Markdown
│       └── corpus_manifest.csv    # 脚本生成的资料清单
│
├── memos/                         # 备忘录（手工+脚本辅助）
│   ├── by-category/               # 按范畴标题组织
│   └── chronological/             # 按时间线组织（符号链接或独立拷贝）
│
├── logs/
│   ├── sampling_decisions.md      # 理论性采样决策日志
│   ├── code_evolution.md          # 编码演化轨迹
│   ├── literature_parking.md      # 文献停车场
│   └── methodological_memos.md    # 方法论反思备忘录
│
├── scripts/
│   ├── build_corpus_manifest.py   # 由研究者编写，见 §5-A
│   ├── init_memo.py               # 由研究者编写，见 §5-C
│   ├── export_comparison_set.py   # 由研究者编写，见 §5-F
│   └── ...                        # 其他按需脚本
│
└── output/                        # 可生成、可丢弃的展示物
    ├── category_index.md
    ├── memo_outline.md
    └── manuscript_skeleton.md
```

`/data/raw/` 一旦写入即视为只读。所有对资料的加工都产生 `/data/processed/` 下的衍生物。这一约束确保在任何阶段都可以追溯到原始资料。`/memos/` 目录采用"按范畴"与"按时间"两种视图是 Glaser（1978）备忘录整理规则的直接落地——按范畴视图用于整理阶段，按时间视图用于回溯思想演化。`/logs/` 目录的四份日志对应 CGT 研究中最容易被审评质疑的四个环节：采样合理性、编码的可追溯性、文献回顾的延迟、以及研究者的反身性。这四份日志应该由研究者手写，脚本的作用是维持其结构化格式与快速检索。

`config.yaml` 建议至少包含：研究者信息、研究实质领域、初始纳入与排除标准、受访者编号命名规范、访谈轮次的日期范围、当前研究阶段标记（开放编码/选择编码/饱和/外部比较/写作）。

---

## 4. Python 脚本的角色

经典扎根理论对软件辅助分析持保留态度。Glaser（1998）与曾晓晴（2025）引述的费小冬立场一致：研究者的分析和创造力是软件所无法取代的。因此本工具包中的 Python 脚本**不做分析判断**，只做三类工作。

第一类是**资料的组织与呈现**。包括文件的命名规范化、元数据提取、清单生成、交叉索引、按不同维度（时间、范畴、资料来源）重排资料。脚本把研究者的注意力聚焦到"该看什么"，但"看了之后得出什么"完全由研究者决定。

第二类是**研究过程的可审计记录**。包括编码每一次变更的时间戳、采样每一次决策的理由与预期、文献每一次被触发的停车场登记、备忘录每一次版本的留存。这些记录的价值不在于 AI 读它们，而在于当审评者问"你是怎么得到这个结论的"时，研究者有完整证据链。CGT 不追求可复制性，但追求可审计性——两者差一口气：前者要求别人按同样程序得到同样结果，后者只要求研究过程透明可查。

第三类是**事务性工作的模板化**。包括备忘录模板生成、参考文献格式转换（GB/T 7714-2015）、从备忘录目录生成写作章节骨架、转录文稿与笔记的对齐、外部比较阶段的文献检索 API 调用。这些工作本身不涉及任何 CGT 判断，只是常规文书工作。

**脚本绝不应做以下事**。第一，不做开放编码的自动化——任何"给一段文字、生成若干编码"的自动化（无论是基于关键词、词频、还是 LLM）都会把预设带入开放编码，破坏开放编码的根本承诺。第二，不做概念聚类或主题建模——LDA、BERTopic、句向量聚类等技术都违反概念—指标模型的两重比较原则，其产出看似像范畴，实则是预设语义空间的切分结果。第三，不做相似度计算驱动的"类似片段推荐"——CGT 的"不断比较"是意义比较，不是向量距离比较；脚本可以做关键词召回，但不应做相似度排序。第四，不做饱和判定——饱和是研究者对资料经验极限、理论密度、理论敏感性的综合判断，任何"N 轮无新代码即饱和"的自动判定都是对这一判断的替代。第五，不做中心范畴的推荐——Glaser 的十一项判据需由研究者综合运用，不是算法评分。第六，不做访谈逐字稿的自动润色——润色以 fit 为代价换取流畅性。

---

## 5. 可自动化任务的核心算法示例

以下八类任务是研究者最常遇到、可以安全自动化的工作。每项给出：任务目的、输入输出规格、关键算法的伪代码或函数签名、以及 CGT 视角下的注意事项。**所有示例均为骨架，需要研究者根据自己的实际数据格式与技术熟练度补全实现**。示例均遵循 PEP 8，使用标准库优先，必要时才引入第三方包。

### 任务 A — 资料清单生成

**目的**：扫描 `/data/raw/` 下所有资料文件，提取 YAML front matter 中的元数据，生成统一清单，便于随时回答"目前有哪些资料"。

**输入**：`/data/raw/` 目录下的 Markdown 文件（每份以 YAML front matter 开头）。
**输出**：`/data/processed/corpus_manifest.csv`，字段包括：文件路径、资料类型（interview/observation/secondary）、受访者或来源编号、日期、访谈轮次、字数、是否录音、命名规范性检查结果。

**核心算法骨架**：

```python
# scripts/build_corpus_manifest.py
# 目的：生成资料清单，用于研究者自查与答辩附录
# 输入：/data/raw/ 下的 .md 文件（含 YAML front matter）
# 输出：/data/processed/corpus_manifest.csv

from pathlib import Path
import csv
import re
import yaml


def extract_front_matter(file_path):
    """读取 Markdown 文件的 YAML front matter，返回 dict。"""
    text = file_path.read_text(encoding="utf-8")
    # front matter 以两行 --- 包围
    match = re.match(r"^---\n(.*?)\n---\n(.*)$", text, re.DOTALL)
    if not match:
        return {}, text
    front_matter = yaml.safe_load(match.group(1))
    body = match.group(2)
    return front_matter, body


def check_naming_convention(file_path, config):
    """按 config.yaml 中规定的命名规范核验文件名，返回 True/False + 问题描述。"""
    # TODO：由研究者填入自己的命名正则表达式
    pattern = config["naming_patterns"].get(file_path.parent.name)
    if pattern and not re.match(pattern, file_path.stem):
        return False, f"文件名不符合规范：{pattern}"
    return True, ""


def build_manifest(raw_dir, output_path, config):
    rows = []
    for md_file in Path(raw_dir).rglob("*.md"):
        front_matter, body = extract_front_matter(md_file)
        ok, note = check_naming_convention(md_file, config)
        rows.append({
            "path": str(md_file.relative_to(raw_dir)),
            "type": md_file.parent.name,
            "id": front_matter.get("id", ""),
            "date": front_matter.get("date", ""),
            "round": front_matter.get("round", ""),
            "word_count": len(body),
            "recorded": front_matter.get("recorded", ""),
            "naming_ok": ok,
            "note": note,
        })
    # 写出 CSV
    with open(output_path, "w", encoding="utf-8", newline="") as f:
        writer = csv.DictWriter(f, fieldnames=rows[0].keys())
        writer.writeheader()
        writer.writerows(rows)


if __name__ == "__main__":
    # TODO：从 config.yaml 加载路径与命名规范
    pass
```

**注意事项**：YAML front matter 的字段由研究者在研究开始前定义并写入 `config.yaml`。不同类型的资料字段可以不同（访谈有"受访者编号、访谈轮次"，二手资料有"来源、获取日期、引用段落"）。脚本不做自动修复命名——它只报告问题，修正由研究者手工完成。

### 任务 B — 访谈转录与对齐

**目的**：若采用录音+转录方案，调用语音识别服务生成初版转录稿，并与访谈当场的纸笔关键词笔记对齐。**注意**：Glaser（1998）不建议录音；曾晓晴（2025）采取的折中是"征得同意后录音"。本任务仅在您选择折中方案时有用。

**输入**：音频文件（`.wav`/`.m4a`）、访谈时段的纸笔关键词笔记（`.md`）。
**输出**：带时间戳的转录稿 Markdown，YAML front matter 包含访谈元信息。

**核心逻辑**：

```python
# scripts/transcribe_interview.py
# 目的：调用 Whisper 生成带时间戳的转录稿，并与现场笔记对齐
# 依赖：faster-whisper 或 openai-whisper

from faster_whisper import WhisperModel

def transcribe(audio_path, model_size="medium"):
    model = WhisperModel(model_size, device="cpu", compute_type="int8")
    segments, info = model.transcribe(audio_path, language="zh", beam_size=5)
    return [(s.start, s.end, s.text) for s in segments]


def render_transcript(segments, meta, field_notes):
    """把时间段+文本与现场关键词笔记对齐后渲染为 Markdown。"""
    # TODO：由研究者决定对齐策略
    # 例如：按时间窗口把现场笔记插入到最接近的转录段落后作为研究者备注
    pass
```

**注意事项**：转录稿一律视为草稿，必须经研究者逐段核对后才能进入编码。脚本绝不调用 LLM 做"摘要"或"润色"。若要做说话人分离（speaker diarization），可使用 `pyannote.audio`，但需注意该工具的模型使用条款。

### 任务 C — 备忘录模板与索引

**目的**：按 Glaser（1978）的可排序性规则（每份备忘录以一个范畴为标题、标记其他范畴、保持一份一文件）辅助生成与检索备忘录。

**输入**：研究者在命令行指定的范畴名；或扫描 `/memos/` 目录的现有备忘录。
**输出**：新备忘录的模板文件；或按范畴聚合的索引 `/output/category_index.md`。

**核心逻辑**：

```python
# scripts/init_memo.py
# 目的：为一个新的备忘录生成模板文件，符合 Glaser 1978 的可排序性规则
# 用法：python init_memo.py "范畴名"

from datetime import datetime
from pathlib import Path
import sys


MEMO_TEMPLATE = """---
title: {title}
related_categories: []  # 若本备忘录涉及其他范畴，请在此列出，便于后续检索
source_refs: []          # 关联的资料片段标识（如 P01-para-17）
stage: open_coding       # open_coding / selective_coding / sorting / writing
created: {timestamp}
---

# {title}

## 来自资料的观察


## 理论性想法


## 与其他范畴的关联


## 下一步追问
"""


def init_memo(title, memo_dir="memos/by-category"):
    timestamp = datetime.now().isoformat(timespec="seconds")
    filename = f"{title}_{timestamp[:10]}.md"
    path = Path(memo_dir) / filename
    path.parent.mkdir(parents=True, exist_ok=True)
    path.write_text(MEMO_TEMPLATE.format(title=title, timestamp=timestamp), encoding="utf-8")
    print(f"已创建备忘录：{path}")


if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("用法：python init_memo.py 范畴名")
        sys.exit(1)
    init_memo(sys.argv[1])
```

索引生成脚本的核心逻辑是遍历 `/memos/by-category/` 下所有文件，读取每份备忘录的 `title` 与 `related_categories`，建立"范畴 → 备忘录列表"与"范畴 → 其他涉及它的备忘录"两张反向索引表，输出为 Markdown。

**注意事项**：模板中的 `related_categories` 字段是手工填写的——脚本不自动从正文中识别范畴名，因为范畴名的边界判断是研究者的概念化工作。

### 任务 D — 编码登记与演化追踪

**目的**：每次研究者对一段资料给出开放性编码或修改编码时，自动记录操作、时间、资料位置、理由，形成可追溯的编码演化日志。

**输入**：研究者通过命令行或简单交互界面输入的编码操作。
**输出**：SQLite 数据库（或结构化 CSV）记录所有编码事件；按需生成"某个编码从首次出现到当前的演化时间线"。

**核心逻辑**：

```python
# scripts/log_coding_event.py
# 目的：记录一次编码事件（新建、合并、重命名、废弃）

import sqlite3
from datetime import datetime

SCHEMA = """
CREATE TABLE IF NOT EXISTS coding_events (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp TEXT NOT NULL,
    event_type TEXT NOT NULL,  -- create / merge / rename / deprecate / relate
    code_name TEXT NOT NULL,
    old_name TEXT,              -- 用于 rename、merge
    source_ref TEXT,            -- 资料片段标识（如 P01-para-17）
    rationale TEXT,             -- 研究者简述理由
    stage TEXT                  -- open_coding / selective_coding
);
"""


def init_db(db_path="data/processed/codes.db"):
    conn = sqlite3.connect(db_path)
    conn.executescript(SCHEMA)
    conn.commit()
    return conn


def log_event(conn, event_type, code_name, rationale, **kwargs):
    conn.execute(
        "INSERT INTO coding_events (timestamp, event_type, code_name, old_name, source_ref, rationale, stage) "
        "VALUES (?, ?, ?, ?, ?, ?, ?)",
        (
            datetime.now().isoformat(timespec="seconds"),
            event_type,
            code_name,
            kwargs.get("old_name"),
            kwargs.get("source_ref"),
            rationale,
            kwargs.get("stage", "open_coding"),
        ),
    )
    conn.commit()


def trace_code(conn, code_name):
    """返回某个编码的完整演化时间线。"""
    # TODO：由研究者补全——需同时追踪该编码作为 code_name 与作为 old_name 的所有事件
    pass
```

**注意事项**：编码一旦登记，原记录不删除。研究者废弃某个编码时应使用 `deprecate` 事件而非删除记录——这保留了研究过程的完整证据链。

### 任务 E — 理论性采样日志

**目的**：每次做出"下一步访谈谁、观察什么、找什么二手资料"的决定时，记录驱动这次决定的概念、预期比较维度、实际收获与修正。

**输入**：研究者填写的结构化决定。
**输出**：`/logs/sampling_decisions.md`，按时间顺序追加条目。

**核心逻辑**：

```python
# scripts/log_sampling_decision.py
# 目的：追加一条理论性采样决策记录

from datetime import datetime
from pathlib import Path

ENTRY_TEMPLATE = """
## {date} — 第 {round} 轮采样

**驱动概念**：{driving_concept}

**选择对象**：{target}

**选择依据**：{rationale}

**预期比较维度**：{expected_dimensions}

**实际资料是否印证预期**（本轮完成后回填）：

**对理论的影响**（本轮完成后回填）：

---
"""


def append_sampling_decision(round_num, driving_concept, target, rationale, expected_dimensions,
                              log_path="logs/sampling_decisions.md"):
    entry = ENTRY_TEMPLATE.format(
        date=datetime.now().date().isoformat(),
        round=round_num,
        driving_concept=driving_concept,
        target=target,
        rationale=rationale,
        expected_dimensions=expected_dimensions,
    )
    Path(log_path).parent.mkdir(parents=True, exist_ok=True)
    with open(log_path, "a", encoding="utf-8") as f:
        f.write(entry)
```

**注意事项**：前向字段（驱动概念、选择对象、依据、预期）在采样前填写；后向字段（是否印证、对理论的影响）在采样完成后回填。这种时序结构本身就是对理论性采样"由浮现驱动"的客观证据，在答辩时是最直接的辩护材料。

### 任务 F — 不断比较的素材提取

**目的**：当研究者浮现出一个概念，快速从所有已编码资料中召回相关片段，按事件—事件、概念—更多事件的格式并列呈现，供研究者做比较判断。

**输入**：研究者指定的概念名；编码数据库中已登记的资料片段引用。
**输出**：`/output/comparison_set_<concept>_<date>.md`，包含所有相关片段的并列展示。

**核心逻辑**：

```python
# scripts/export_comparison_set.py
# 目的：为某一概念导出可供不断比较的资料片段集

from pathlib import Path


def collect_segments_by_code(conn, code_name):
    """从编码数据库中检索所有标记为该编码的资料片段引用。"""
    cursor = conn.execute(
        "SELECT source_ref FROM coding_events WHERE code_name = ? AND event_type = 'create'",
        (code_name,),
    )
    return [row[0] for row in cursor.fetchall()]


def load_segment_text(source_ref, processed_dir="data/processed/transcripts"):
    """按 source_ref（如 P01-para-17）从转录稿中提取段落。"""
    # TODO：由研究者按自己的资料组织格式实现段落定位
    # 常见做法：source_ref 由"受访者编号-段落编号"组成，段落编号对应转录稿中的 Markdown 段落序号
    pass


def export_comparison_set(conn, code_name, output_dir="output"):
    refs = collect_segments_by_code(conn, code_name)
    segments = [(ref, load_segment_text(ref)) for ref in refs]
    lines = [f"# 不断比较素材集：{code_name}\n\n"]
    for i, (ref, text) in enumerate(segments, 1):
        lines.append(f"## 片段 {i}（来源：{ref}）\n\n{text}\n\n---\n\n")
    output_path = Path(output_dir) / f"comparison_set_{code_name}.md"
    output_path.write_text("".join(lines), encoding="utf-8")
```

**注意事项**：脚本**只做召回与并列展示**，不做相似度排序、不做聚类、不做自动归纳。比较的意义判断完全由研究者在阅读并列片段时做出。

### 任务 G — 文献停车场

**目的**：在 CGT 要求延迟文献回顾的前提下，研究者不可避免会读到或听到相关文献。停车场机制允许研究者登记这些"本应延迟的接触"，但推迟其对研究的影响，直到相关概念浮现后的外部比较阶段再召回。

**输入**：研究者登记的一条文献接触记录。
**输出**：`/logs/literature_parking.md`，按标签（tag）组织。

**核心逻辑**：

```python
# scripts/park_literature.py
# 目的：把不应立即影响研究的文献接触记录存入停车场，按标签组织以便日后召回

from datetime import datetime
from pathlib import Path
import yaml

ENTRY_TEMPLATE = """
### {date} — {short_title}

**完整引用**：{citation}

**接触方式**：{contact_type}   <!-- read / heard / recommended / skimmed -->

**一句话印象**：{impression}

**标签**：{tags}   <!-- 逗号分隔的候选概念名 -->

**状态**：parked   <!-- parked / released（当对应概念浮现后） -->

---
"""


def park_literature(citation, impression, tags, short_title, contact_type="read",
                     log_path="logs/literature_parking.md"):
    entry = ENTRY_TEMPLATE.format(
        date=datetime.now().date().isoformat(),
        short_title=short_title,
        citation=citation,
        contact_type=contact_type,
        impression=impression,
        tags=", ".join(tags),
    )
    Path(log_path).parent.mkdir(parents=True, exist_ok=True)
    with open(log_path, "a", encoding="utf-8") as f:
        f.write(entry)


def release_by_tag(tag, log_path="logs/literature_parking.md"):
    """当某个概念浮现时，召回所有带该标签的停车场条目。"""
    # TODO：研究者实现按标签检索并生成召回报告
    pass
```

**注意事项**：停车场不是"已采纳文献"，是"已接触但暂不影响研究的文献"。当中心范畴浮现、研究进入外部比较阶段时，按标签召回后由研究者重新审视哪些文献仍然相关、哪些已不必引用。

### 任务 H — 写作脚手架

**目的**：当手工整理（sorting）完成、研究者确定了论文大纲后，从备忘录目录自动组装成论文章节的初始骨架。

**输入**：研究者手写的大纲（YAML 格式，列出每一章节及其对应的范畴）；`/memos/by-category/` 下的备忘录。
**输出**：`/output/manuscript_skeleton.md`，每章节下列出该章节对应范畴的所有备忘录节选（按整合契合规则的携带向前注释合并）。

**核心逻辑**：

```python
# scripts/build_manuscript_skeleton.py
# 目的：从整理好的大纲与备忘录目录组装写作骨架

from pathlib import Path
import yaml


def load_outline(outline_path="output/outline.yaml"):
    """大纲格式示例：
    chapters:
      - title: 第四章 围透析期 CKD 患者心理调适的核心过程
        sections:
          - title: 4.1 感知评价阶段
            categories: [感知评价, 可控性, 自主性]
          - title: 4.2 学习阶段
            categories: [学习, 信息获取]
    """
    return yaml.safe_load(Path(outline_path).read_text(encoding="utf-8"))


def collect_memos_for_category(category, memo_dir="memos/by-category"):
    """收集某个范畴相关的全部备忘录。"""
    matched = []
    for memo_file in Path(memo_dir).glob("*.md"):
        # 简化起见：用文件名前缀匹配；复杂情况下读取 front matter 的 title 与 related_categories
        if memo_file.stem.startswith(category):
            matched.append(memo_file.read_text(encoding="utf-8"))
    return matched


def build_skeleton(outline, output_path="output/manuscript_skeleton.md"):
    lines = []
    for chapter in outline["chapters"]:
        lines.append(f"# {chapter['title']}\n\n")
        for section in chapter["sections"]:
            lines.append(f"## {section['title']}\n\n")
            for category in section["categories"]:
                memos = collect_memos_for_category(category)
                lines.append(f"### 范畴：{category}（{len(memos)} 份备忘录）\n\n")
                for memo in memos:
                    lines.append(memo + "\n\n---\n\n")
    Path(output_path).write_text("".join(lines), encoding="utf-8")
```

**注意事项**：脚本产出的是**备忘录的拼接**，不是"写好的章节"。研究者在骨架基础上重写为流畅的理论陈述，这一步是不可自动化的。Glaser（1978, Ch. 8）关于写作的专章强调，写作阶段仍在生成新想法，不是简单的"把整理好的内容抄成文章"。

---

## 6. 用户需要自行编写的部分

前述八项任务的代码骨架只覆盖核心逻辑，以下几类细节需要您根据自己的研究实际自行补全。

**资料格式的约定**。您需要在 `config.yaml` 中定义：资料文件的命名规范（用正则表达式表达）、YAML front matter 必需字段、资料片段引用格式（如 `P01-para-17` 指受访者 01 的第 17 段）。这些约定一旦定下，不要中途修改，否则脚本需要大量迁移。

**编码数据库的字段扩展**。任务 D 给出的 `coding_events` schema 是最小集。如果您的研究需要记录更多维度（例如编码的"抽象层级"、编码在某个访谈内的频次等），可在 schema 中增列。增列请用 SQLite 的 `ALTER TABLE ADD COLUMN`，不要重建表——重建表会丢失已有记录。

**资料片段的定位实现**。任务 F 的 `load_segment_text` 函数是留白最多的地方。常见的实现是：转录稿 Markdown 中每个段落用空行分隔，脚本按段落序号定位；或者使用 Markdown 的锚点标题（如 `## [P01-17]`）。具体实现取决于您的转录稿格式，请在动手编码前先选定一种段落标注方案并在所有转录稿中统一执行。

**命令行接口或简单图形界面**。上述脚本骨架都使用命令行参数。如果您不习惯命令行，可用 `typer` 或 `click` 简化命令行；或用 `streamlit` 做一个极简的网页界面（单文件、无前端构建、本地运行）。不建议投入过多时间在界面上——CGT 研究的大部分时间应该用于阅读资料与写备忘录，不是折腾工具。

**备份与版本控制策略**。建议在 `/memos/`、`/logs/`、`/data/processed/` 目录建立 Git 版本控制，每天或每个研究节点提交一次。原始资料 `/data/raw/` 因含个人身份信息，不应入库，应通过独立的加密备份（如本地加密磁盘+定期外部硬盘备份）处理。`.gitignore` 至少应包含 `.env`、`data/raw/`、`*.db-journal`、Python 相关的 `__pycache__/` 与 `.venv/`。

**Whisper 模型选择与本地部署**。若采用录音+转录路径，本地部署 faster-whisper 的 `medium` 或 `large-v3` 模型已可满足中文医疗访谈转录需求。不建议使用商业 API（如 OpenAI Whisper API），原因有二：访谈资料包含患者隐私，发往外部服务存在伦理风险；伦理审查通常不会通过把访谈音频发往境外服务器。

**对 LLM 的使用边界**。AI 协作工具（如 Claude、GPT）只在以下场景使用：检索 SKILL 与 REFERENCE 中的条款、就方法论问题提供解释与引证、对研究者草拟的备忘录或章节给出**语言层面**的修改建议。AI 不应被用于：给资料做编码、推荐中心范畴、判断饱和、做概念之间的关系判断。把这条约束写进您的 `config.yaml` 的 `ai_usage` 字段，并在每次开始工作前让自己重读一次。

---

## 7. 工作流示例

为了让上述组件的关系具体化，下面给出一个典型的两周工作流。假设您已完成方法论学习并进入实质研究阶段。

**第一周周一**。完成第一位受访者 P01 的深度访谈。回办公室后把纸笔笔记整理为 `data/raw/interviews/P01_2024-04-15.md`（带 YAML front matter）。如有录音且已获同意，运行 `transcribe_interview.py` 生成转录草稿，逐段核对后覆盖保存到同一文件。运行 `build_corpus_manifest.py` 更新清单。

**第一周周二至周四**。对 P01 的资料做开放编码。每发现一个概念运行 `init_memo.py 概念名` 创建备忘录，在备忘录中记录观察、理论想法、与其他范畴的可能关联、下一步追问。每次为资料段落标记编码时运行 `log_coding_event.py`。当脑中浮现"下一位受访者应该找谁"的想法时运行 `log_sampling_decision.py` 登记采样决定。

**第一周周五**。完成 P02、P03 的访谈与编码。此时编码数据库中已积累 30 余个开放编码，备忘录目录下约 10 份备忘录。做一次本周的 Git 提交。

**第二周周一**。注意到某位同事提到 Festinger 的社会比较理论与您看到的患者现象有关。这正是文献停车场的典型使用场景——运行 `park_literature.py` 登记该文献接触，标签为"社会比较"。**不读原文**——继续您的开放编码。

**第二周周三**。某个编码（假设是"向下比较"）在第 10 位受访者后仍在持续生成新属性，您判断它可能是中心范畴的候选之一。运行 `export_comparison_set.py 向下比较` 导出所有带此编码的资料片段并列展示，打印出来在桌上做手工比较。对照 REFERENCE §3 的十一项判据逐项检查。

**第二周周五**。您判断"重获控制"作为中心范畴候选更合适（因为它满足更多判据）。进入选择性编码。运行 `release_by_tag 社会比较` 从停车场召回 Festinger 的记录，重新评估是否纳入外部比较。更新 `methodological_memos.md` 记录这一判断的理由。

这个工作流的每一步都由研究者的判断驱动，脚本只负责把每一步的证据固化下来。一个月后如果您忘了为什么从"适应"改判为"重获控制"，日志与备忘录的组合可以让您在 10 分钟内重建当时的推理。这就是"可审计性"的含义。

---

## 8. 给非开发者的上手建议

如果您本人不是开发者，以下顺序是最稳妥的推进方式。第一步先读 SKILL 全文，不读代码，不考虑脚本。第二步读 REFERENCE 的 §1（十八个编码家族）与 §3（核心范畴判据），这是日常编码会反复用到的两节。第三步手工操作一遍——用纯 Markdown 文件和文件夹做资料管理、编码登记、备忘录写作，完成至少 5 位受访者的资料。这一步会让您体会到手工方式的痛点在哪里，然后才知道哪类脚本对自己最有价值。第四步，从最痛的一项开始，用 AI 协作（如 Claude）按本 README 的算法骨架生成脚本。**切忌一开始就试图搭建完整的脚本体系**——研究者最常见的失败模式是在方法论尚不熟练时就把精力投入工具建设，结果工具过度设计、研究反而停滞。

合格的工具标准是："让研究者能专注于阅读资料与写备忘录"。如果某个脚本让您花了更多时间在调试而不是研究本身，就停下来，退回手工。

依赖库的建议：Python 3.10+；`pyyaml`（读 front matter 与 config）；`pandas`（处理清单，非必需）；`faster-whisper` 或 `openai-whisper`（仅在需要转录时）；`typer`（命令行简化，非必需）。SQLite 是 Python 标准库自带，不需额外安装。全部锁定在 `requirements.txt` 中。

最后一条提醒：CGT 的核心承诺是相关性（relevance），即研究是否针对研究对象真正关切的问题。脚本、软件、工作流、文档体系都服务于这个承诺。当您感到工具在"推动研究往一个方向走"而不是"把研究者的判断固化下来"时，请立即停下来，重读 SKILL §8（AI 协作中的红旗），必要时推翻重来。Glaser（1978: 3）的话也适用于工具：

> "We offer this as one of the many actions in sociology, rather than as the only right one."

本工具包是众多可能方式之一。如果它不适合您的研究，丢掉它。

---

## 9. 依赖与环境

最小依赖（含义：仅任务 A、C、D、E、G、H 需要）：

```text
python >= 3.10
pyyaml >= 6.0
```

中等依赖（加上任务 B 的转录与 F 的资料召回）：

```text
pandas >= 2.0
faster-whisper >= 1.0    # 若使用语音转录
```

推荐的开发环境：VS Code + Python 扩展，或 PyCharm Community Edition。虚拟环境用 `venv`（标准库）或 `uv`（近年流行的替代品）。所有命令在项目根目录下运行。首次使用时：

```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

若您不熟悉命令行，直接让 AI 助手逐条解释并示范。这部分不属于方法论范畴，是纯粹的技术入门。

---

## 附：文档一致性检查清单

每次修订 SKILL、REFERENCE、或本 README 时，请对照以下清单确认三者一致。

SKILL 的 §5-第 10 条要求让理论性编码自然浮现。REFERENCE §1 给出十八个编码家族的具体内容。两者应一致。

SKILL 的 §7 给出核心范畴十一判据要义。REFERENCE §3 给出完整英文原文与中文要义。两者序号应一致。

SKILL 的 §3-程序二关于不断比较四层级。REFERENCE §5 给出每一层级的具体操作示例。两者层级划分应一致。

SKILL 的 §5-第 11 条关于备忘录手工整理。REFERENCE §9 给出整理的十一项规则，§10 给出备忘录书写的六项可排序性规则。两者对操作要求应一致。

README 的 §5 各类任务与 SKILL §6 的禁止清单应无冲突——任何一个任务都不得违反 SKILL 的禁止条款。

---

*本 README 与 `classical_grounded_theory_SKILL.md`、`classical_grounded_theory_REFERENCE.md` 共同构成经典扎根理论 AI 协作工具包。三者任一的修订应同步核对另两份。*
