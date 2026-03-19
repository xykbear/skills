---
name: docling
description: 使用 Docling 进行文档转换。将 PDF、DOCX、PPTX、图片、音频、视频等文档转换为 Markdown、JSON、HTML 等格式。适用于文档解析、OCR 处理、RAG 文档准备、批量转换等场景。支持 MCP 协议，可集成到 LangChain、LlamaIndex 等 AI 框架。当用户提到文档转换、PDF 转 Markdown、提取文档内容、使用 docling 时触发。
---

# Docling 文档转换工具

Docling 是 IBM 开源的文档解析和转换工具，支持将多种格式文档转换为统一的 Docling Document，并导出为 Markdown、JSON、HTML 等格式。

## 安装

```bash
pip install docling
```

### 常用 extras

```bash
# 音频/视频处理 (ASR pipeline)
pip install "docling[asr]"

# VLM pipeline (视觉语言模型)
pip install "docling[vlm]"

# OCR 引擎支持
pip install "docling[easyocr]"     # 跨平台 OCR
pip install "docling[tesserocr]"  # Tesseract OCR

# 组合安装
pip install "docling[asr,vlm,easyocr]"
```

### 常见安装问题

- **CPU Only**: `pip install docling torch --index-url https://download.pytorch.org/whl/cpu`
- **macOS Intel**: `pip install "docling[mac_intel]"`
- **离线使用**: 首次自动下载模型，或使用 `docling-tools models download`

## Python API

### 最简用法

```python
from docling.document_converter import DocumentConverter

converter = DocumentConverter()
result = converter.convert("document.pdf")

# 导出格式
print(result.document.export_to_markdown())  # Markdown
print(result.document.export_to_json())       # JSON (无损)
print(result.document.export_to_text())      # 纯文本
```

### 支持的输入格式

| 格式 | 说明 |
|------|------|
| PDF | PDF 文档 |
| DOCX, XLSX, PPTX | Microsoft Office |
| Markdown | .md 文件 |
| 图片 | PNG, JPEG, TIFF, WEBP |
| 音频 | MP3, WAV, M4A (需 asr) |
| 视频 | MP4, AVI, MOV (需 asr) |
| XML | USPTO, JATS, XBRL |

> 更多格式和选项请参考 `references/python-api.md`

### 常用选项

```python
from docling.document_converter import DocumentConverter
from docling.datamodel.base_models import InputFormat
from docling.datamodel.pipeline_options import PdfPipelineOptions
from docling.document_converter import PdfFormatOption

# 自定义 PDF 处理
pipeline_options = PdfPipelineOptions()
pipeline_options.do_ocr = True           # 启用 OCR
pipeline_options.do_table_structure = True  # 表格识别

converter = DocumentConverter(
    format_options={
        InputFormat.PDF: PdfFormatOption(pipeline_options=pipeline_options)
    }
)
result = converter.convert("document.pdf")
```

> 更多转换选项请参考 `references/python-api.md`（当需要限制文档大小、处理二进制流、调整表格模式时）

### 文档分块

```python
from docling.document_converter import DocumentConverter
from docling.chunking import HybridChunker

result = DocumentConverter().convert("document.pdf")
chunker = HybridChunker()
chunks = list(chunker.chunk(result.document))

for chunk in chunks[:3]:
    print(chunk.text[:200])
```

> 高级分块选项请参考 `references/python-api.md`

### 访问文档元素

```python
doc = result.document
text = doc.export_to_markdown()   # 获取文本
page_count = doc.num_pages         # 页数
```

## CLI 用法

### 基础命令

```bash
# 基本转换
docling ./document.pdf                    # PDF 转 Markdown
docling --to json ./document.pdf         # 转 JSON
docling --to html ./document.pdf         # 转 HTML

# 输出目录
docling --output ./output/ ./document.pdf
```

### 常用选项

```bash
# OCR
docling --ocr ./document.pdf              # 启用 OCR
docling --force-ocr ./document.pdf        # 强制全页 OCR
docling --ocr-lang eng,chi_sim ./doc.pdf # 指定语言

# 表格
docling --tables ./document.pdf           # 启用表格识别
docling --table-mode fast ./doc.pdf      # 快速模式
docling --table-mode accurate ./doc.pdf  # 精确模式

# 设备
docling --device cpu ./document.pdf       # CPU
docling --device mps ./document.pdf      # Apple Silicon
docling --device cuda ./document.pdf      # CUDA
docling --device auto ./document.pdf      # 自动选择
docling --device xpu ./document.pdf       # XPU
```

> 完整 CLI 选项请参考 `references/cli.md`（当需要 VLM pipeline、ASR、enrichments 等高级选项时）

### VLM Pipeline

```bash
# 使用视觉语言模型
docling --pipeline vlm --vlm-model granite_docling ./document.pdf
```

> VLM 模型选择和配置请参考 `references/cli.md`

## 框架集成

### LlamaIndex

```python
# 安装
pip install llama-index-readers-docling

# 加载文档
from llama_index.readers.docling import DoclingReader

reader = DoclingReader()
documents = reader.load_data(path="./document.pdf")
```

> 完整集成请参考 `references/integrations.md`（DoclingNodeParser、RAG 示例）

### LangChain

```python
# 安装
pip install langchain-docling

# 加载文档
from langchain_docling import DoclingLoader

loader = DoclingLoader(path="./document.pdf")
docs = loader.load()
```

> 完整集成、批量加载、RAG pipeline 请参考 `references/integrations.md`

## MCP Server

### Claude Desktop 配置

编辑 `~/Library/Application Support/Claude/claude_desktop_config.json`:

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

> 其他 MCP Client 配置请参考 `references/mcp.md`

## 离线/本地模型

```bash
# 下载模型
docling-tools models download

# 指定模型路径
export DOCLING_ARTIFACTS_PATH="/local/path/to/models"
```

> 离线环境配置详细说明请参考 `references/python-api.md`

## 常见问题

1. **首次使用下载模型慢**: 使用 `docling-tools models download` 预先下载
2. **OCR 不工作**: 安装对应引擎 `pip install "docling[easyocr]"` 或 `pip install "docling[tesserocr]"`
3. **内存不足**: 使用 `--device cpu` 并减少 `--num-threads`
4. **转换超时**: 使用 `max_num_pages` 限制页数

## 参考文档

| 文档 | 何时阅读 |
|------|----------|
| `references/python-api.md` | 需要完整 API 参考、限制文档大小、处理二进制流、高级转换选项 |
| `references/cli.md` | 需要 VLM/ASR pipeline、enrichments、完整 CLI 选项 |
| `references/integrations.md` | 需要 LlamaIndex/LangChain 完整集成、RAG pipeline、批量处理 |
| `references/mcp.md` | 需要配置其他 MCP Client、自定义 MCP 选项 |

## 官方资源

- 文档: https://docling-project.github.io/docling/
- GitHub: https://github.com/docling-project/docling
- Discord: https://docling.ai/discord
