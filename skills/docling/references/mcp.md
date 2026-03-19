# MCP Server 配置

本文档详细介绍 Docling MCP Server 的配置和使用方法。

## 什么是 MCP

MCP (Model Context Protocol) 是一个开放标准协议，用于将 AI 应用连接到外部工具和服务。它让 AI 代理能够调用外部工具，就像调用内置函数一样。

## 安装 MCP Server

```bash
# 使用 uvx 运行 (推荐)
uvx --from=docling-mcp docling-mcp-server

# 或安装到本地
pip install docling-mcp

# 本地运行
python -m docling_mcp.server
```

## Claude Desktop 配置

### 配置文件位置

- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`
- Linux: `~/.config/Claude/claude_desktop_config.json`

### 基本配置

```json
{
  "mcpServers": {
    "docling": {
      "command": "uvx",
      "args": [
        "--from=docling-mcp",
        "docling-mcp-server"
      ]
    }
  }
}
```

### 高级配置

```json
{
  "mcpServers": {
    "docling": {
      "command": "uvx",
      "args": [
        "--from=docling-mcp",
        "docling-mcp-server",
        "--port",
        "8080"
      ],
      "env": {
        "DOCLING_ARTIFACTS_PATH": "/path/to/models"
      }
    }
  }
}
```

### 配置参数

| 参数 | 说明 |
|------|------|
| `--port` | MCP Server 端口 (默认: 8765) |
| `DOCLING_ARTIFACTS_PATH` | 本地模型路径 |
| `--max-file-size` | 最大文件大小 (MB) |
| `--max-pages` | 最大页数 |

## LM Studio 配置

### 方式一：自动安装

1. 打开 LM Studio
2. 进入 Settings > MCP Servers
3. 点击 "Add MCP Server"
4. 搜索 "docling" 或 "document conversion"
5. 点击安装

### 方式二：手动配置

```json
{
  "mcpServers": {
    "docling": {
      "command": "uvx",
      "args": ["--from=docling-mcp", "docling-mcp-server"]
    }
  }
}
```

## Cursor 配置

### macOS

编辑 `~/Library/Application Support/Cursor/User/globalStorage/mcp.json`:

```json
{
  "mcpServers": {
    "docling": {
      "command": "uvx",
      "args": ["--from=docling-mcp", "docling-mcp-server"]
    }
  }
}
```

### Windows

编辑 `%APPDATA%\Cursor\User\globalStorage\mcp.json`:

```json
{
  "mcpServers": {
    "docling": {
      "command": "uvx",
      "args": ["--from=docling-mcp", "docling-mcp-server"]
    }
  }
}
```

## 其他 MCP Client

### Claude Code

```json
{
  "mcpServers": {
    "docling": {
      "command": "uvx",
      "args": ["--from=docling-mcp", "docling-mcp-server"]
    }
  }
}
```

### 自定义 MCP Client

```python
from mcp import ClientSession, StdioServer

async def main():
    server = StdioServer(
        command="uvx",
        args=["--from=docling-mcp", "docling-mcp-server"]
    )
    async with ClientSession(server) as session:
        # 调用工具
        result = await session.call_tool(
            "convert_to_markdown",
            {"source": "./document.pdf"}
        )
```

## 可用工具

Docling MCP Server 提供以下工具：

### convert_to_markdown

将文档转换为 Markdown 格式。

```python
# 参数
{
    "source": "document.pdf",  # 文件路径或 URL
    "options": {               # 可选
        "ocr": True,
        "tables": True,
        "device": "auto"
    }
}
```

### convert_to_json

将文档转换为 JSON 格式 (无损)。

```python
{
    "source": "document.pdf",
    "options": {
        "ocr": True,
        "preserve_layout": True
    }
}
```

### convert_to_html

将文档转换为 HTML 格式。

```python
{
    "source": "document.pdf",
    "options": {
        "image_mode": "embedded"  # embedded, referenced, placeholder
    }
}
```

### extract_text

仅提取文本内容。

```python
{
    "source": "document.pdf",
    "options": {
        "include_metadata": True
    }
}
```

### get_document_info

获取文档基本信息。

```python
{
    "source": "document.pdf"
}
# 返回: 页数、大小、格式等
```

## 使用示例

### Claude Desktop 中使用

> "把这个 PDF 文件转换成 Markdown"

> "提取文档中的表格数据"

> "将这个 URL 的 PDF 转为 JSON"

### Cursor 中使用

在 Cursor 中直接使用自然语言：
- "Convert this PDF to markdown"
- "Extract all tables from the document"
- "Summarize this paper"

### 自动化脚本

```python
import asyncio
from mcp import ClientSession, StdioServer

async def convert_documents():
    server = StdioServer(
        command="uvx",
        args=["--from=docling-mcp", "docling-mcp-server"]
    )
    
    async with ClientSession(server) as session:
        # 初始化
        await session.initialize()
        
        # 转换文档
        result = await session.call_tool(
            "convert_to_markdown",
            {"source": "./document.pdf"}
        )
        
        print(result.content[0].text)

asyncio.run(convert_documents())
```

## 故障排除

### 连接问题

```bash
# 测试 MCP Server 是否运行
uvx --from=docling-mcp docling-mcp-server --help

# 检查日志
uvx --from=docling-mcp docling-mcp-server -v
```

### 模型下载问题

```bash
# 预下载模型
docling-tools models download

# 设置本地模型路径
export DOCLING_ARTIFACTS_PATH="/path/to/models"
```

### 性能问题

```bash
# 限制资源使用
uvx --from=docling-mcp docling-mcp-server \
    --max-file-size 50 \
    --max-pages 100
```

## 资源

- [Docling MCP GitHub](https://github.com/docling-project/docling-mcp)
- [MCP 官方文档](https://modelcontextprotocol.io)
- [Claude Desktop 配置示例](https://github.com/docling-project/docling-mcp/blob/main/docs/integrations/claude_desktop_config.json)
