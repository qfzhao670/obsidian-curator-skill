# Obsidian Curator

面向 Codex 与 Claude Code 的 Obsidian 笔记整理技能。它可以重排已有 Markdown 笔记、解释笔记中的概念、把视频字幕转换为中文笔记，以及把已解决的技术对话整理成可复用的博客文章。

这个技能只处理 Markdown 内容，不依赖脚本或第三方运行时。输出会保留 Obsidian 常用语法，包括 `[[wikilinks]]`、YAML frontmatter、标签、callout、图片嵌入和高亮。

> [!NOTE]
> 默认原则是忠实于输入：可以重组、压缩和润色，但不会凭空补充事实、数据、例子或结论。

## 功能

Obsidian Curator 会先判断请求属于哪一种模式，再只加载对应的规则：

| 模式 | 适用场景 | 默认结果 |
| --- | --- | --- |
| 整理已有笔记 | 重排、润色 Markdown，添加双链、标签、frontmatter 或 callout | 就地修改原文件 |
| 概念补丁 | 解释笔记中指定的概念、术语或句子 | 在原句下插入折叠 callout |
| 字幕生成笔记 | 将 NoteGPT、SRT 或 VTT 字幕整理成中文课程笔记 | 创建新的 Markdown 文件 |
| 对话总结成文 | 将已解决的开发/debug 对话或知识问答整理成博客笔记 | 在指定目录创建新文件 |

四种模式彼此独立。整理笔记时不会擅自添加“概念补丁”；只有用户明确询问某个概念时，才会插入解释。

## 安装

### Codex

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/qfzhao670/obsidian-curator-skill.git ~/.codex/skills/obsidian-curator
```

项目级安装时，将仓库放到目标项目的 `.agents/skills/obsidian-curator/`：

```bash
mkdir -p .agents/skills
git clone https://github.com/qfzhao670/obsidian-curator-skill.git .agents/skills/obsidian-curator
```

### Claude Code

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/qfzhao670/obsidian-curator-skill.git ~/.claude/skills/obsidian-curator
```

项目级安装时，将仓库放到目标项目的 `.claude/skills/obsidian-curator/`。

### Codex 与 Claude Code 共用一份

如果两个工具都要使用，可以只克隆一次，再创建软链接：

```bash
mkdir -p ~/.local/share
git clone https://github.com/qfzhao670/obsidian-curator-skill.git ~/.local/share/obsidian-curator-skill
mkdir -p ~/.codex/skills ~/.claude/skills
ln -s "$HOME/.local/share/obsidian-curator-skill" "$HOME/.codex/skills/obsidian-curator"
ln -s "$HOME/.local/share/obsidian-curator-skill" "$HOME/.claude/skills/obsidian-curator"
```

> [!IMPORTANT]
> 复制或链接时必须保留整个仓库，不能只复制 `SKILL.md`；具体工作流和格式规则位于 `references/` 中。

## 使用

安装后直接描述任务即可。也可以明确指定使用 `obsidian-curator` 技能。

### 整理已有笔记

```text
使用 obsidian-curator 整理这篇笔记，补充 frontmatter 和必要的双链：
/path/to/vault/计算机网络.md
```

也可以一次处理一个目录：

```text
整理 /path/to/vault/System Design 下的 Markdown 笔记。这次保留原有标题，不添加外部链接。
```

处理过程中会：

- 将长段落拆成短段落和列表
- 使用克制的 `##`、`###` 标题层级
- 原样保留 `![[图片]]` 与已有高亮
- 仅在确有关系时添加 `[[双链]]`
- 仅为公认的官方文档添加外链
- 将 callout 控制在每篇一至两个

### 添加概念补丁

```text
帮我解释这篇笔记中“IP 层”和“应用层”的区别：
/path/to/vault/Networking.md
```

解释会紧跟在相关原句下方，并使用默认折叠的 callout：

```markdown
> [!question]- 补丁：IP 层与应用层有什么区别？
> 简洁、通俗且严谨的解释。
```

### 将视频字幕转换为中文笔记

```text
把这份视频字幕整理成一篇中文 Obsidian 笔记：
/path/to/subtitles/operating-systems.srt
```

技能会清理时间戳、序号、口头禅和重复表达，再按内容脉络重组，而不是按照时间码机械分段。专业术语首次出现时会使用“中文（English, 缩写）”形式。

支持的输入包括：

- NoteGPT 导出的带时间戳文本
- SRT 字幕
- VTT 字幕

原字幕不会被修改，整理结果会写入新文件。

### 将技术对话总结成博客笔记

在问题已经解决或知识点已经讲清之后，可以说：

```text
把当前对话总结成一篇 Obsidian 博客笔记，写到：
/path/to/vault/开发记录
```

输出会面向不熟悉原项目的读者，并执行以下处理：

- 将具体问题抽象为可复用的排查或学习经验
- 移除项目名、人名、内网地址、密钥和真实路径等敏感信息
- 保留关键报错、根因、验证过程与最终采用的解决方案
- 在目标目录中发现高度相似的笔记时，先询问是补充原文还是新建文章
- 对话尚未得出确定结论时，不强行生成完整文章

## 输出风格

- 简洁、有条理，不堆叠装饰性元素
- 以中文为主，保留必要的英文术语和缩写
- 表格只用于真正的多项对比
- 双链嵌入正文语境，不在文末堆砌“相关笔记”
- 默认写回文件；可以在请求中指定“创建副本”或“只给 diff”

## 项目结构

```text
obsidian-curator-skill/
├── SKILL.md
├── README.md
└── references/
    ├── curate-note.md
    ├── concept-patch.md
    ├── subtitle-to-note.md
    ├── context-to-post.md
    ├── formatting-guide.md
    └── example-curated-note.md
```

- `SKILL.md`：识别用户意图，并将任务路由到对应工作流
- `references/curate-note.md`：已有笔记的整理原则与步骤
- `references/concept-patch.md`：概念解释的触发条件和格式
- `references/subtitle-to-note.md`：字幕清洗、翻译与成文流程
- `references/context-to-post.md`：技术对话的判断、脱敏与博客化规则
- `references/formatting-guide.md`：共享的 Obsidian 语法与排版约束
- `references/example-curated-note.md`：整理后的完整示例
