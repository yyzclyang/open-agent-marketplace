---
description: 禅道 bug 辅助
---

# 禅道 Bug 辅助

当用户提到禅道相关问题时，提示可用的命令。

**触发条件：**
TRIGGER when: 用户提到"禅道 bug"、"查看 bug"、"bug 分析"、"禅道上的问题"、"指派给我的 bug"等关键词

DO NOT TRIGGER when: 用户明确在执行其他开发任务，未提及 bug 或禅道

**行为：**
当检测到用户需要查看或分析禅道 bug 时，提示：

> 你可以使用以下命令：
> - `/bugs` - 列出指派给你的 bug
> - `/bug-analyze <bug-id>` - 拉取 bug 详情并分析

**前置配置提醒：**
如果用户首次使用，提醒配置环境变量：

```env
ZENTAO_URL=http://your-zentao/zentao
ZENTAO_TOKEN=your-token
ZENTAO_PRODUCT_ID=your-product-id
```
