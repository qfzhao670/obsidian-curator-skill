# 视频字幕 → 中文笔记：流程与示例

把一段视频课程字幕（含时间戳的原始讲稿）生成一篇中文 Obsidian 笔记时，参考本文件。
它与「整理已有笔记」最大的不同：输入没有现成结构，需要先**理解、切分、翻译**，再套用格式化规则。

## 输入格式

### NoteGPT（导出 .txt）
```
00:00:00
Hello everyone, welcome to the first lecture in the course Operating System...

00:00:26
We will see what the subject means and what we can learn from the subject...
```
- 每段：一行时间戳，下面跟一段话，段与段之间空行。

### SRT / VTT
```
1
00:00:00,000 --> 00:00:26,000
Hello everyone, welcome to...

2
00:00:26,000 --> 00:00:48,000
We will see what...
```
- 带序号与起止时间码。

**处理方式**：不管哪种，先去掉时间戳/序号/空行，拼成连续讲稿。时间戳只用于大致判断节奏，
**不要**按它机械切段——真正的段落边界来自内容脉络，不来自时间码。

## 完整流程

1. **通览定题**：读完整段，判断「这一节在讲什么」。
   讲课通常有固定脉络，可作为标题骨架：
   开场白 → 核心定义 → 图解/例子 → 反面推演（没有它会怎样）→ 正面结论 → 类型/功能/目标列举 → 小结。
2. **清洗口语**：剔除
   - 填充词：Alright、So、you know、well、actually、let me just…
   - 重复句、口误、改口（说一半换说法）。
   - 与内容无关的寒暄/套话（"I hope this was clear"、"Thank you for watching"）。
   - 「稍后会在另一节讲 XX」这类预告 → 浓缩成一句（如「（后续章节再展开）」，不展开）。
3. **切分结构**：按脉络定 `##`/`###`，把并列点拆成 `- ` 列表；一句话讲一件事。
4. **翻译**：见下「术语处理」。
5. **套用 Obsidian 特性**：frontmatter（`source` 填视频/课程名）、`==高亮==` 关键术语、
   callout 小结（≤2 个）、按需 `[[双链]]` 与外链（规则同 `formatting-guide.md`）。
6. **写回**：生成新文件，文件名用主题（如 `操作系统导论.md`），不改动字幕原文件。

## 术语处理

- 专业术语**首次出现**写成「中文（English, 缩写）」：`==操作系统（Operating System, OS）==`，
  之后用中文或缩写即可。
- 译名要**准确、通用**，常用对照：

| English | 中文 |
| --- | --- |
| Operating System (OS) | 操作系统 |
| CPU (Central Processing Unit) | 中央处理器 |
| RAM (Random Access Memory) | 随机存取存储器（内存） |
| ROM (Read-Only Memory) | 只读存储器 |
| I/O (Input/Output) devices | 输入输出设备 |
| primary / secondary memory | 主存 / 辅存 |
| intermediary | 中间人 / 中介 |

- 讲课者举的**具体例子**（Word、Excel、Chrome、Windows、Linux…）原样保留，不翻译、不替换。
- 拿不准的译法用「中文（English）」并存，不硬造。

## 边界（重要）

- **浓缩允许**：删废话、合并重复、理顺口语语序，都属于「重组」。
- **编造禁止**：不得新增讲稿里没有的事实、数据、例子或结论，也不要随意删除讲稿中的细节知识。
- **翻译忠实**：不添油加醋、不夹带自己的观点。
- 语气是「干净的听课笔记」，不是逐字稿，不保留时间戳。

## Before / After 示例

（取材自 "Introduction to Operating Systems" 视频第一节，NoteGPT 导出字幕。）

### Before（原始字幕节选）

```
00:00:48
Alright, so let's get started. So, here it says that an Operating System also abbreviated as OS is a program that manages the computer hardware. It also provides a basis for application programs and acts as an intermediary between computer user and computer hardware. So, before we understand these two lines, I will be explaining it to you with a diagram.

00:02:26
So first of all at the lowermost level we have the computer hardware. What are computer hardwares? Computer hardware consists of resources like CPU which is the Central Processing Unit, and memory, and I/O devices, which means Input-Output devices. ...

00:06:22
So, let's see how he does it. Let's imagine, there is no operating system over here. We just have this user, the application program, and the computer hardware. So, if he wants to write a document... he has to tell explicitly to the computer hardware, what he wants to do in the form of code. ...

00:08:14
So, in order to overcome this problem, we have the Operating system. The Operating System resides between the computer hardware and the users. So, it is an intermediary, that acts between the computer hardware and the user. ...
```

### After（生成的中文笔记节选）

```markdown
---
tags: [cs/os]
aliases: [操作系统导论]
source: Introduction to Operating Systems（视频课程）
---

# 操作系统导论

## 什么是操作系统

==操作系统（Operating System, OS）== 是管理计算机硬件的程序：
- 为应用程序提供运行基础
- 在用户与硬件之间充当中间人

## 计算机系统的基本结构

- 最底层：**计算机硬件** —— CPU（中央处理器）、内存、I/O 设备（输入输出设备）
  - 内存分主存（RAM）与辅存（ROM）
  - 输入设备：键盘、鼠标、麦克风；输出设备：显示器、音箱
- 中间层：**操作系统**
- 最上层：**应用程序**

## 两类软件

- **系统软件**：直接控制/修改硬件（操作系统也是系统软件）
- **应用软件**：完成特定任务、供用户直接使用
  - 文字处理：Microsoft Word；电子表格：Excel
  - 编译器：写 C/C++/Java；文本编辑器：Notepad、Wordpad
  - 浏览器：Chrome、Firefox、IE

## 没有操作系统会怎样

用户每件小事都要**自己写代码**告诉硬件，极其繁琐：
- 打开 Word → 要请求硬件把程序加载进内存
- 打字 → 要请求硬件显示到屏幕
- 保存 → 要请求硬件存到硬盘
→ 普通人根本无法使用计算机

## 操作系统的价值

OS 位于硬件与用户之间，作为中间人接管这些琐事：
- 双击 Word，OS 负责打开、准备输入界面
- 打字，OS 告诉硬件如何显示
- 保存，OS 决定存哪、怎么存、分配多少内存

> [!note] 小结
> OS 管理硬件、为程序提供基础、在用户与硬件之间做中间人，
> 从而让使用计算机变得便捷（convenience）且高效（efficiency）。
```

**要点**：去掉时间戳与填充词（Alright/So）；把「定义 + 结构 + 反面推演 + 正面结论」映射为
`##` 标题；并列的软件例子、没 OS 的繁琐步骤都拆成列表；术语首次出现标「中文（English, 缩写）」；
结尾一个 callout 小结；没有硬凑 `[[双链]]`。
