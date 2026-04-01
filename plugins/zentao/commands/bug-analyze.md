---
description: 拉取禅道 bug 详情并分析问题
---

# 禅道 Bug 分析

拉取指定 bug 的详情，分析问题并提供解决建议。

**用法：** `/bug-analyze <bug-id>`

**前置配置：**
在环境变量或 `.env` 中设置：

- `ZENTAO_URL` - 禅道地址
- `ZENTAO_TOKEN` - API Token

**步骤：**

1. 检查环境变量并获取 bug 详情（必须在同一个 bash 命令中执行，避免多次权限确认）：

```bash
[ -f .env ] && source .env
missing=""
[ -z "$ZENTAO_URL" ] && missing="$missing ZENTAO_URL"
[ -z "$ZENTAO_TOKEN" ] && missing="$missing ZENTAO_TOKEN"
if [ -n "$missing" ]; then
  echo "错误：缺少环境变量:$missing"
  echo "请在 .env 文件中配置："
  echo "  ZENTAO_URL=http://your-zentao/zentao"
  echo "  ZENTAO_TOKEN=your-token"
  exit 1
fi
curl -s "${ZENTAO_URL}/api.php/v1/bugs/{bug-id}" \
  -H "Token: ${ZENTAO_TOKEN}"
```

如果缺少环境变量，直接输出错误提示并停止。

2. 如果 bug 的 steps 中包含图片（格式如 `file-read-{id}.png`），下载图片（与上一步合并到同一个 bash 命令中执行）：

```bash
curl -s "${ZENTAO_URL}/file-read-{file-id}.png" \
  -o /tmp/zentao_bug_{bug-id}_img_{file-id}.png
```

4. 使用 Read 工具读取下载的图片进行分析

5. 分析 bug 信息：
    - 读取 bug 标题、描述、严重程度、优先级
    - 查看附件截图（如有）
    - 结合截图和描述分析问题根因
    - 提供可能的修复方向

**输出格式：**

## Bug #{id} 分析报告

**基本信息**
| 字段 | 值 |
|------|-----|
| 标题 | {title} |
| 产品 | {productName} |
| 模块 | {moduleTitle} |
| 严重程度 | {severity} |
| 优先级 | P{pri} |
| 状态 | {statusName} |
| 指派给 | {assignedTo} |

**问题描述**
{steps 清理 HTML 标签后的纯文本}

**截图分析**（如有）
{对截图的分析}

**问题分析**
{根据描述和截图分析可能的原因}

**建议修复方向**

1. {建议1}
2. {建议2}

---

**参数：** $1
