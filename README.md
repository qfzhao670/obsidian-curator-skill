# Obsidian Curator Skill

一个给 **Codex** 和 **Claude** 共用的 agent skill，用来整理、重排、美化 Obsidian 笔记（markdown）。
它把你的「有内容但缺结构」的笔记，整理成简洁、有条理的成品，并加上 Obsidian 特有的知识关联能力：
内部双链 `[[wikilinks]]`、标签 + frontmatter、callout 摘要框、外部链接。
它也能把一段视频课程字幕（含时间戳的原始讲稿，如 NoteGPT 导出的 `.txt`）直接生成一篇中文 Obsidian 笔记。
它还能把你一次已解决的开发 / debug 对话，或一次讲清某个知识点的知识问答，总结抽象成 CSDN 风格的博客笔记，写到指定的 Obsidian 目录。

## 它能做什么

- 把长段落拆成短段落 + 无序列表，让结构一目了然。
- 用克制的中文标题层级（H2/H3）组织知识，不花哨。
- 保留图片嵌入 `![[...]]` 与 `==高亮==`。
- 添加 YAML frontmatter（tags / aliases / source）与正文标签。
- 为概念与其他笔记建立 `[[双链]]`，打通 Obsidian 的图视角与反向链接。
- 为工具/概念补充官方文档外链（不杜撰 URL）。
- 用 callout 框出关键结论（每篇最多 1–2 个）。
- 在你对笔记里某个概念不清楚时，于原句旁打一个「解释补丁」（折叠 callout），用简洁通俗又严谨的话讲清楚。
- 把一段视频课程字幕（含时间戳的原始讲稿）读入、切分、翻译，直接生成一篇结构清晰的中文 Obsidian 笔记。
- 在你一次开发 / debug 对话正确解决某个 bug / 需求，或一次知识问答讲清某个知识点后，把它总结抽象成一篇 CSDN 风格的博客笔记（去敏感信息、通用化、可复现），写到指定的 Obsidian 目录。

## 目录结构

```
obsidian-curator-skill/
├── SKILL.md                      # 主指令：先做功能分类，再分派到对应 reference（Codex 与 Claude 都读）
├── README.md                     # 本文件
└── references/
    ├── curate-note.md            # 功能① 整理/美化笔记：核心原则 + 工作流程
    ├── concept-patch.md          # 功能② 概念补丁：格式 + 解释写法（仅用户主动提问时用）
    ├── subtitle-to-note.md       # 功能③ 视频字幕 → 中文笔记的流程与示例
    ├── context-to-post.md        # 功能④ 开发/debug 对话或知识问答 → 博客笔记的流程与示例
    ├── formatting-guide.md       # 共享：Obsidian 语法速查 + 特性用法规则 + before/after 示例
    └── example-curated-note.md   # 功能① 的成品示例（可直接在 Obsidian 打开）
```

## 安装

两种工具的 skill 都用同一套约定：一个文件夹 + `SKILL.md`（frontmatter 只需 `name` 和 `description`）。
所以这一份 skill 两边通用，各自软链接过去即可。

### 安装到 Claude Code

用户级（所有项目可用）：

```bash
ln -s "$(pwd)/obsidian-curator-skill" ~/.claude/skills/obsidian-curator-skill
```

或项目级：把 `obsidian-curator-skill/` 文件夹放进目标项目的 `.claude/skills/` 下。

### 安装到 Codex

```bash
ln -s "$(pwd)/obsidian-curator-skill" ~/.codex/skills/obsidian-curator-skill
```

（Codex 也支持项目级 `.agents/skills/`，同理放入即可。）

> 注意：`ln -s` 的第二个参数要用**绝对路径**，否则软链接可能失效。

## 用法

装好后，直接对 agent 说（skill 会先自动判断你的请求属于四个功能中的哪一个，再执行对应功能；
四个功能互不混用，补丁只在你主动提问某概念时触发）：

```text
帮我整理 /Users/zhaoqifan/Code/obsidian notes/computer science/High Level Design 下的笔记
```

或指定单篇：

```text
整理这篇笔记，加上双链和 frontmatter：<路径/文件名.md>
```

也可以在请求里开关特性，例如「这次不要加外部链接」「只给 diff 不要改原文件」。

解释某句话里的概念（在原句旁打一个补丁）：

```text
帮我解释这句话里的「IP 层」和「应用层」是什么意思：<路径>/4.Networking.md
```

把视频字幕整理成中文笔记：

```text
把这段视频字幕整理成一篇中文 Obsidian 笔记：<路径>/NoteGPT_Introduction to Operating Systems.txt
```

把这次开发 / debug 对话或知识问答总结成博客笔记（写到指定目录）：

```text
当前的内容可以总结成一篇 obsidian 笔记，请总结，写到 <vault>/开发记录
```

## 风格说明

- **简洁、有条理、不花哨**是默认风格。
- 只重组/润色已有内容，**不编造**新知识、新事实。
- 层级克制（H2/H3 为主），排版克制（表格、callout 都少量使用）。
