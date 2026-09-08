---
name: obsidian-curator
description: 整理、重排并美化 Obsidian 笔记（markdown），为笔记中不清楚的概念/术语打「解释补丁」，将视频课程字幕直接生成中文笔记，或把一次已解决的开发/debug 对话或知识问答总结抽象成 CSDN 风格的博客笔记。当用户要求整理/美化/重排 markdown、添加双链 wikilinks、标签 tags、callout、frontmatter、外部链接、解释概念术语、把视频字幕整理成笔记，或「把这次对话总结成一篇 obsidian 笔记」时使用。
---

# Obsidian Curator

把「有内容但缺结构」的 Obsidian 笔记整理成简洁、有条理的成品；或把一段视频字幕直接生成中文笔记；
或在你对笔记里某个概念不清楚时，于原句旁打一个「解释补丁」；或在一次开发 / debug 对话正确解决、
或一次知识问答讲清某个知识点后，把它总结抽象成 CSDN 风格的博客笔记，落到指定目录。四个功能彼此独立，**先分类、再执行**。

## 第一步：功能分类（最先做，且只执行命中的一项）

先判断用户请求属于下面哪一种，然后**只读取并执行**对应功能的参考文件，不要混用：

| 用户意图 | 功能 | 读取 |
| --- | --- | --- |
| 已有 markdown 笔记要整理 / 美化 / 重排，加双链、tags、callout、frontmatter | ① 整理美化笔记 | `references/curate-note.md` + `references/formatting-guide.md` |
| 对笔记里某概念/术语提问「XX 是什么意思 / 有什么区别 / 为什么需要」 | ② 概念补丁 | `references/concept-patch.md` |
| 提供视频字幕（NoteGPT / SRT / VTT）要直接生成中文笔记 | ③ 字幕生成笔记 | `references/subtitle-to-note.md` + `references/formatting-guide.md` |
| 明确要求把本次开发 / debug 对话或知识问答总结成博客笔记（如「当前的内容可以总结成一篇 obsidian 笔记，请总结」） | ④ 对话总结成文 | `references/context-to-post.md` + `references/formatting-guide.md` |

分类硬规则：

- **补丁只在用户主动提问某概念时使用**；整理笔记或生成笔记时**不得主动打补丁**。
- 意图含糊时向用户确认，不要猜测。
- 一次请求命中一个功能；若用户既要求整理、又顺带问某个概念，则**先整理、后打补丁**，二者是独立步骤。
- 功能④仅在问题已解决（或知识点已讲清）、结论确定时使用；且用户未指定目标目录时先向用户确认，不猜路径。

## 共享约定

- **写回**：整理（①）默认就地编辑原文件；字幕（③）默认生成新文件（不改字幕原文件）；对话总结（④）默认生成新文件到用户指定目录；用户要求时可输出新文件或只给 diff、不落盘。
- 四个功能都会用到的 Obsidian 语法与特性细则（wikilinks、tags、callout、frontmatter、外链）统一见 `references/formatting-guide.md`。
