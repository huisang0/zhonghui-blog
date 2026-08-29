---
title: Markdown扩展功能
published: 2024-05-01
updated: 2024-11-29
description: '了解Fuwari博客支持的Markdown扩展语法'
image: ''
tags: [演示, 示例, Markdown, Fuwari]
category: '示例'
draft: false 
---

## GitHub仓库卡片
你可以添加动态GitHub仓库卡片，页面加载时会从GitHub API拉取仓库信息。

::github{repo="Fabrizz/MMM-OnSpotify"}

使用语法`::github{repo="<owner>/<repo>"}`即可生成GitHub仓库卡片。

```markdown
::github{repo="saicaca/fuwari"}
```

## 提示块

支持这几种提示块类型：`note`（注释）、`tip`（提示）、`important`（重要）、`warning`（警告）、`caution`（注意）

:::note
用于强调需要留意的信息，即使快速浏览也应当注意。
:::

:::tip
提供辅助性的可选小提示，帮助你更好完成操作。
:::

:::important
成功完成操作所必须阅读的关键信息。
:::

:::warning
存在潜在风险，需要立刻引起重视的重要内容。
:::

:::caution
执行操作后可能带来的负面后果。
:::

### 基础语法

```markdown
:::note
用于强调需要留意的信息，即使快速浏览也应当注意。
:::

:::tip
提供辅助性的可选小提示，帮助你更好完成操作。
:::
```

### 自定义标题

提示块的标题可以自定义。

:::note[自定义标题]
这是一个带有自定义标题的注释块。
:::

```markdown
:::note[自定义标题]
这是一个带有自定义标题的注释块。
:::
```

### GitHub兼容写法

> [!TIP]
> 同时也支持[The GitHub syntax](https://github.com/orgs/community/discussions/16925)的提示块语法。

```
> [!NOTE]
> 这里使用GitHub风格的注释语法。

> [!TIP]
> 这里使用GitHub风格的注释语法。
```

### 文字遮罩

可以添加隐藏遮罩文字，遮罩内部同样支持**Markdown**格式。

这部分内容:spoiler[被**隐藏**起来了]！

```markdown
这部分内容:spoiler[被**隐藏**起来了]！

```