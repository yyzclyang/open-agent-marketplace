---
description: 列出禅道 bug
---

# 禅道 Bug 列表

列出指定产品的 bug，方便快速查看待处理问题。

**用法：** `/bugs [limit]`

**前置配置：**
在环境变量或项目 `.env` 中设置：

- `ZENTAO_URL` - 禅道地址
- `ZENTAO_TOKEN` - API Token
- `ZENTAO_PRODUCT_ID` - 产品 ID（在禅道产品页面 URL 中可找到）

**步骤：**

1. 检查环境变量并获取 bug 列表（必须在同一个 bash 命令中执行，避免多次权限确认）：

```bash
[ -f .env ] && source .env
missing=""
[ -z "$ZENTAO_URL" ] && missing="$missing ZENTAO_URL"
[ -z "$ZENTAO_TOKEN" ] && missing="$missing ZENTAO_TOKEN"
[ -z "$ZENTAO_PRODUCT_ID" ] && missing="$missing ZENTAO_PRODUCT_ID"
if [ -n "$missing" ]; then
  echo "错误：缺少环境变量:$missing"
  echo "请在 .env 文件中配置："
  echo "  ZENTAO_URL=http://your-zentao/zentao"
  echo "  ZENTAO_TOKEN=your-token"
  echo "  ZENTAO_PRODUCT_ID=your-product-id"
  exit 1
fi
curl -s "${ZENTAO_URL}/api.php/v1/products/${ZENTAO_PRODUCT_ID}/bugs?status=assigntome&limit=${1:-20}" \
  -H "Token: ${ZENTAO_TOKEN}"
```

如果缺少环境变量，直接输出错误提示并停止。

3. 格式化输出 bug 列表，显示：
    - ID
    - 标题
    - 严重程度（severity: 1-致命 2-严重 3-主要 4-次要 5-建议）
    - 优先级（pri）
    - 状态（statusName）

**输出格式：**

## Bug 列表 (产品: {productName})

| ID   | 标题  | 严重程度 | 优先级 | 状态  |
|------|-----|------|-----|-----|
| #123 | xxx | 主要   | P3  | 激活  |
| #122 | xxx | 次要   | P4  | 已解决 |

共 {n} 条

使用 `/bug-analyze <id>` 查看详情和分析。

---

**参数：** $1 (可选，默认 20)
