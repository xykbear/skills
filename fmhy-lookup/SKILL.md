---
name: fmhy-lookup
description: Search the FMHY (Free Media Heck Yeah) wiki for free tools, software, and site recommendations. Make sure to use this skill whenever the user mentions looking for tools, software, apps, sites, resources, or recommendations (video downloaders, VPNs, ad blockers, password managers, proxy tools, download utilities, etc.). Trigger this skill for ANY tool-finding request, even if the user doesn't explicitly mention FMHY.
license: MIT
---

## 数据文件

缓存文件位于：`references/fmhy-cache.md`

## 工作流程

### 步骤 1：定位缓存文件

使用 **Glob 工具** 搜索：
1. 优先搜索：`**/fmhy-lookup/references/fmhy-cache.md`
2. 如无结果（首次运行）：搜索 `**/fmhy-lookup/SKILL.md`

从搜索结果中提取 skill 根目录，定义变量：
```
$CACHE_FILE = <skill 目录>/references/fmhy-cache.md
```

### 步骤 2：检查是否需要更新

以下情况需要更新缓存：
- 缓存文件不存在（首次运行）
- 缓存超过 30 天未更新
- 用户请求中包含"最新"关键词（如"根据最新数据找 XXX"）

检查命令：
```bash
if [ ! -f "$CACHE_FILE" ] || [ $(find "$CACHE_FILE" -mtime +30) ]; then
    echo "NEED_UPDATE"
fi
```

### 步骤 3：执行更新（如需要）

```bash
mkdir -p "$(dirname "$CACHE_FILE")"
curl -s "https://api.fmhy.net/single-page" -o "$CACHE_FILE"
```

### 步骤 4：搜索

使用 **Grep 工具** 在 `$CACHE_FILE` 中搜索用户需求的关键词。

### 步骤 5：读取内容

使用 **Read 工具** 读取匹配的章节内容。

### 步骤 6：输出

按以下模板格式化结果：

```markdown
### [工具名称](链接)
- **功能**: 功能说明
- **平台**: Windows / Mac / Linux / Android / iOS
- **许可**: 免费 / 付费 / 开源
- **备注**: 其他信息

---
💡 缓存更新时间：YYYY-MM-DD
```
