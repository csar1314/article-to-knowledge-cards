# make-knowledge-cards

将文章提炼成便于阅读和复习的 Markdown 知识卡片。这是一个由 Codex 加载并执行的指令型 Skill，核心规则位于 [SKILL.md](skills/make-knowledge-cards/SKILL.md)。

## 1. 项目解决什么问题

文章阅读后常需要再次整理重点、删除重复表述，并准备复习问题。本项目提供统一的提炼规则，让 Codex 从用户提供的文章中选出重要知识，按“一张卡片一个知识点”组织输出，减少手工整理工作。

它要求输出忠于原文，保留适用条件和不确定性，不添加材料没有的事实。卡片由模型生成，措辞可能变化，使用者仍应对照原文核查。

## 2. 主要功能

- 接受直接粘贴的文章正文，或本地 `.md`、`.markdown`、`.txt` 文件。
- 通常生成 5～8 张卡片；材料不足时只生成有依据的数量并说明原因，不强行凑数。
- 每张包含标题、核心知识点、通俗解释，以及原文例子或附答案的自测问题。
- 合并同义和重复内容，跳过广告、寒暄及无关细节。
- 保留否定、数字单位、适用条件、作者观点归属及相关与因果的区别。
- 默认用中文 Markdown 回复；可遵循用户指定的语言，要求保存时写入指定文件。
- 空白或没有可提炼知识的材料会得到原因说明，不生成空卡片。

当前不支持网页抓取、PDF、Anki 导出或图形界面。项目没有独立命令行转换程序、服务端或自动转换脚本。

## 3. 安装方法

### 环境要求

需要可加载本地 Skill 的 Codex 环境。处理本地文件时，Codex 需要能读取输入文件；保存结果时需要输出位置的写入权限。本 Skill 本身无需安装 Python、Node.js 或业务依赖。

### 安装到当前项目

在本项目根目录打开 PowerShell，执行下面的命令，将完整 Skill 文件夹复制到项目的技能发现目录：

```powershell
New-Item -ItemType Directory -Path '.agents/skills' -Force | Out-Null
Copy-Item -LiteralPath './skills/make-knowledge-cards' -Destination './.agents/skills/' -Recurse
```

安装后应存在：

```text
.agents/skills/make-knowledge-cards/
├── SKILL.md
└── agents/
    └── openai.yaml
```

以上命令用于首次安装。若目标中已有同名 Skill，请先检查现有版本，再决定如何更新。源目录 `skills/` 用于维护；复制安装后，源文件的后续修改需要同步到安装目录。

### 安装到个人目录（可选）

如需跨项目使用，在本项目根目录执行以下命令，替代上面的项目安装方式：

```powershell
New-Item -ItemType Directory -Path "$HOME/.agents/skills" -Force | Out-Null
Copy-Item -LiteralPath './skills/make-knowledge-cards' -Destination "$HOME/.agents/skills/" -Recurse
```

技能发现目录与加载行为依据 [OpenAI 官方技能文档](https://learn.chatgpt.com/docs/build-skills)：项目目录为 `.agents/skills`，个人目录为 `$HOME/.agents/skills`。安装后若技能未出现，重启 Codex 并确认目录结构。本文提供安装步骤，不表示已替用户完成安装。

## 4. 使用方法与输入输出示例

### 粘贴文章

安装并加载 Skill 后，在 Codex 中输入：

```text
请使用 $make-knowledge-cards，将下面的文章整理成知识卡片：

每次会议结束后记录待办事项。每项待办注明负责人。会议结束后要记录待办。
```

以下是项目中 [short-cards.md](examples/short-cards.md) 的完整示例输出：

```markdown
材料仅支持两个独立知识点，以下生成 2 张卡片。

### 卡片 1：会后记录待办
- **核心知识点**：每次会议结束后记录待办事项。
- **通俗解释**：会议结束时把需要做的事情记下来。
- **例子或自测问题**：自测：何时记录待办？答案：每次会议结束后。

### 卡片 2：标明负责人
- **核心知识点**：每项待办注明负责人。
- **通俗解释**：每件待办都明确由谁负责。
- **例子或自测问题**：自测：每项待办注明什么？答案：负责人。
```

这段输入有重复表述，仅支持两个独立知识点，因此示例输出为 2 张，而非强行生成 5 张。

### 读取本地文件并保存

在本项目作为工作目录时输入：

```text
请使用 $make-knowledge-cards，读取 examples/science.md，
将知识卡片保存到项目根目录的 science-cards.md。
```

输入文件是现有的 [science.md](examples/science.md)。输出应为 Markdown 卡片；项目保存的参考输出 [science-cards.md](examples/science-cards.md) 包含 7 张卡片。项目根目录的 `science-cards.md` 由这次使用请求生成，示例目录中的文件用于对照，实际生成措辞可能不同。

省略“保存到……”时，默认直接在聊天回复中返回卡片。处理其他位置的文件时，可提供绝对路径。

### 更多现有示例

| 类型 | 输入 | 参考输出 | 卡片数 |
| --- | --- | --- | --- |
| 科普文章 | [science.md](examples/science.md) | [science-cards.md](examples/science-cards.md) | 7 |
| 操作流程 | [procedure.txt](examples/procedure.txt) | [procedure-cards.md](examples/procedure-cards.md) | 6 |
| 观点与观察 | [opinion.md](examples/opinion.md) | [opinion-cards.md](examples/opinion-cards.md) | 6 |
| 短文本与重复句 | [short.txt](examples/short.txt) | [short-cards.md](examples/short-cards.md) | 2 |

## 项目结构与验证

- `skills/make-knowledge-cards/SKILL.md`：输入范围、提炼规则和输出格式。
- `skills/make-knowledge-cards/agents/openai.yaml`：显示名称、简介与默认提示词。
- `examples/`：四组输入、参考输出和 [测试记录](examples/TEST-RESULTS.md)。

目前已有四组人工示例检查和官方 `quick_validate.py` 结构验证通过记录，没有自动语义测试套件。

如需复验结构，需另备官方 skill-creator 工具、Python 和 PyYAML；它们用于开发验证，不是本 Skill 的运行依赖：

```text
python <skill-creator目录>/scripts/quick_validate.py skills/make-knowledge-cards
```

结构验证检查 Skill 格式，不能保证生成内容质量。效果复测应对照示例原文，检查事实依据、条件保留、去重与数量处理。
