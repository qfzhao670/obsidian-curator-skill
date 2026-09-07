# 功能④：把开发 / debug 对话总结成博客笔记

当用户**明确要求**把「本次开发 / 调试对话」总结抽象成一篇博客风格的 Obsidian 笔记时
（例如说「当前的内容可以总结成一篇 obsidian 笔记，请总结」），遵循本文件。
输入是**当前对话的上下文**；输出是一篇写给陌生读者的 CSDN 风格技术博客，落到用户指定的 Obsidian 目录。

## 适用前提（先判断）

- 本次对话是**一个完整、已解决的问题**：一个具体的 bug 被修好，或一个具体需求被实现。
- 若问题还没解决、结论还不确定，**不要**强行成文，先向用户说明「这个对话还不能总结成一篇完整的帖子」。

## 核心原则（优先级最高）

1. **忠实于对话**：只写这次对话里真实发生的问题、排查过程与最终方案，不编造、不夸大、不添油加醋。
2. **抽象通用化**（本功能的核心价值）：把「我这个项目、这台机器上的具体 bug」抽象成「这类问题的通用解法」。
   - 去掉：真实项目名、公司名、人名、内网 IP / 域名、真实路径、账号 / 密钥 / token 等敏感信息。
   - 保留：技术要点、报错关键信息、可复现的排查与解决步骤。
   - 代码 / 命令示例里的具体值用占位名代替（如 `your-project`、`your-service`、`your-branch`）。
3. **博客视角**：读者是**遇到同类问题的陌生人**，不是项目协作者。必须交代背景、环境、报错、完整步骤，
   让他们照着就能复现、就能解决；项目内部才知道的上下文要补成读者能看懂的说法。

## 输出结构（CSDN 风格）

| 小节 | 内容 |
| --- | --- |
| frontmatter | `tags`、`aliases`、`source`（可写「开发记录」）、`date` |
| 标题 `#` | 一句话概括问题，如「解决 xxx 报错」或「xxx 踩坑记录」 |
| 前言 | 一两句背景：在做什么事、遇到了什么问题 |
| 问题描述 | 现象、报错信息、触发条件（贴关键报错原文） |
| 排查过程 | 怎么定位的：先尝试了哪些、排除了哪些、最终如何锁定 |
| 根因分析 | 为什么会出现这个问题（讲清机理，而不是只给结论） |
| 解决方案 | 可复现的步骤 / 命令 / 代码块 |
| 总结 / 踩坑点 | 提炼通用经验（callout，≤2 个）；可选「参考链接」（只放官方文档） |

## 格式规则

- 复用 `references/formatting-guide.md`：frontmatter、tags、callout、外链、表格的写法与克制原则。
- **代码块**：贴完整的报错与修复代码，用 ``` 围栏并标注语言；报错原文不要截断关键信息。
- 标题层级克制（`#` + `##`，最多 `###`）。
- 外链只加公认官方文档，拿不准的不加。

## 写回

1. 目标目录由用户指定（如「写到 xxx 目录」）；**用户未给路径时，先向用户确认 vault 路径与目标目录**，不要猜。
2. 生成新文件，文件名用文章标题（如 `解决 MySQL 连接超时.md`），不改动任何已有文件。

## 示例

### 对话上下文要点（输入）

```
用户在 macOS 上给一个 Django 项目跑 `python manage.py migrate`，报
`django.db.utils.OperationalError: (2002, "Can't connect to local MySQL server through socket ...")`。
排查：先怀疑 MySQL 没启动 → `brew services list` 确认在跑；再怀疑连接配置 → settings.py 里
HOST 写的是 `localhost`，而本机 MySQL 监听的是 socket 不是 TCP。定位：把 HOST 改成 `127.0.0.1`
强制走 TCP。验证通过，migrate 成功。
```

### 生成的博客笔记（输出，节选）

````markdown
---
tags: [dev/debug, dev/django]
aliases: [Django 连不上 MySQL 报 2002]
source: 开发记录
date: 2026-09-07
---

# 解决 Django 连接 MySQL 报 2002 错误

## 前言

本地 Django 项目执行数据库迁移时报连不上 MySQL，记录一下排查与解决过程。

## 问题描述

```text
django.db.utils.OperationalError: (2002, "Can't connect to local MySQL server through socket ...")
```

## 排查过程

- 先确认 MySQL 是否启动：`brew services list`，正常在跑。
- 再怀疑连接配置：`settings.py` 里 `HOST = 'localhost'`。
- 定位：本机 MySQL 监听的是 Unix socket，而 `localhost` 会优先走 socket，
  与实际监听方式不匹配。

## 根因分析

`localhost` 会让客户端优先通过 Unix socket 连接；当 MySQL 未在预期 socket 路径监听时连接失败。

## 解决方案

把 `HOST` 改成 `127.0.0.1`，强制走 TCP：

```python
DATABASES = {
    'default': {
        'HOST': '127.0.0.1',
    }
}
```

重新 `python manage.py migrate`，成功。

> [!note] 踩坑点
> `localhost` 与 `127.0.0.1` 在 MySQL 连接里不是一回事：前者可能走 socket，后者走 TCP。
````

**要点**：把「某台 Mac 上的某个项目」抽象成通用的「Django + MySQL 连接」问题；保留了报错原文与可复现步骤；
去掉了具体路径、项目名；结尾一个 callout 提炼通用经验。
