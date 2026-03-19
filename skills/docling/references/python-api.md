# Python API 完整参考

本文档提供 Docling Python API 的完整参考。当主文件中的基础示例无法满足需求时，请查阅本文档。

## DocumentConverter

### 基本用法

```python
from docling.document_converter import DocumentConverter

converter = DocumentConverter()
result = converter.convert(source)
```

### convert() 方法参数

| 参数 | 类型 | 说明 |
|------|------|------|
| source | str/DocumentStream | 文件路径、URL 或 DocumentStream |
| max_num_pages | int | 最大处理页数 |
| max_file_size | int | 最大文件大小 (bytes) |

### 返回值

`result` 包含：
- `result.document`: DoclingDocument 对象
- `result.errors`: 错误列表

## DoclingDocument 导出方法

```python
doc = result.document

# 导出为各种格式
doc.export_to_markdown()        # Markdown
doc.export_to_markdown(mark_for_toc=False)  # 不生成目录
doc.export_to_json()            # JSON (无损)
doc.export_to_json(indent=2)   # 带缩进
doc.export_to_text()           # 纯文本
doc.export_to_html()           # HTML
doc.export_to_doctags()        # Doctags
doc.export_to_vtt()            # WebVTT
doc.export_to_dict()           # Python dict

# 保存到文件
doc.save_as_json("output.json")
doc.save_as_markdown("output.md")
doc.save_as_html("output.html")
doc.save_as_yaml("output.yaml")
doc.save_as_vtt("output.vtt")

# 从文件加载
doc = DoclingDocument.load_from_json("input.json")
doc = DoclingDocument.load_from_yaml("input.yaml")
doc = DoclingDocument.load_from_doctags("input.doctags")
```

## 输入格式

### 本地文件

```python
result = converter.convert("./document.pdf")
result = converter.convert("/absolute/path/to/doc.pdf")
```

### URL

```python
result = converter.convert("https://arxiv.org/pdf/2408.09869")
```

### 二进制流

```python
from io import BytesIO
from docling.datamodel.base_models import DocumentStream

buf = BytesIO(your_binary_stream)
source = DocumentStream(name="my_doc.pdf", stream=buf)
result = converter.convert(source)
```

### 批量处理

```python
from pathlib import Path
from docling.document_converter import DocumentConverter

files = list(Path("./docs").glob("**/*"))
converter = DocumentConverter()

for file in files:
    if file.is_file():
        result = converter.convert(str(file))
        print(result.document.export_to_markdown())
```

## 转换选项

### PdfPipelineOptions

```python
from docling.datamodel.base_models import InputFormat
from docling.datamodel.pipeline_options import PdfPipelineOptions
from docling.document_converter import DocumentConverter, PdfFormatOption

pipeline_options = PdfPipelineOptions()
pipeline_options.do_ocr = True                    # 启用 OCR
pipeline_options.do_table_structure = True       # 表格识别
pipeline_options.artifacts_path = "/path/to/models"  # 本地模型路径
pipeline_options.enable_remote_services = True   # 允许远程服务
```

### 表格选项

```python
from docling.datamodel.pipeline_options import TableFormerMode

pipeline_options = PdfPipelineOptions(do_table_structure=True)
pipeline_options.table_structure_options.mode = TableFormerMode.ACCURATE  # 默认: accurate
# 或
pipeline_options.table_structure_options.mode = TableFormerMode.FAST       # 快速模式

# 控制单元格匹配
pipeline_options.table_structure_options.do_cell_matching = True  # 默认
```

### OCR 选项

```python
from docling.datamodel.pipeline_options import (
    EasyOcrOptions,
    TesseractOcrOptions,
    RapidOcrOptions,
    OcrMacOptions,
)

# EasyOCR
pipeline_options = PdfPipelineOptions(do_ocr=True)
pipeline_options.ocr_options = EasyOcrOptions()

# Tesseract
pipeline_options.ocr_options = TesseractOcrOptions()

# RapidOCR
pipeline_options.ocr_options = RapidOcrOptions()

# macOS OCR
pipeline_options.ocr_options = OcrMacOptions()
```

### 完整配置示例

```python
from docling.document_converter import DocumentConverter
from docling.datamodel.base_models import InputFormat
from docling.datamodel.pipeline_options import PdfPipelineOptions, EasyOcrOptions, TableFormerMode
from docling.document_converter import PdfFormatOption

pipeline_options = PdfPipelineOptions(
    do_ocr=True,
    do_table_structure=True,
    artifacts_path="/local/models"
)
pipeline_options.ocr_options = EasyOcrOptions()
pipeline_options.table_structure_options.mode = TableFormerMode.ACCURATE

converter = DocumentConverter(
    format_options={
        InputFormat.PDF: PdfFormatOption(pipeline_options=pipeline_options)
    }
)

result = converter.convert(
    "document.pdf",
    max_num_pages=100,
    max_file_size=20971520
)
```

## 文档分块

### HybridChunker

```python
from docling.document_converter import DocumentConverter
from docling.chunking import HybridChunker

result = DocumentConverter().convert("document.pdf")
chunker = HybridChunker(
    max_tokens=512,        # 最大 token 数
    merge_short=True,      # 合并短块
    merge_long=True,       # 合并长块
)
chunks = list(chunker.chunk(result.document))

for chunk in chunks:
    print(f"Text: {chunk.text[:100]}...")
    print(f"Metadata: {chunk.metadata}")
```

### HierarchicalChunker

```python
from docling.chunking import HierarchicalChunker

hc = HierarchicalChunker()
hchunks = list(hc.chunk(result.document))
```

### 遍历文档内容

```python
doc = result.document

# 遍历所有元素
for item in doc.iterate_items():
    print(item.text)

# 遍历图片
for img in doc.pictures:
    print(f"Image: {img}, size: {img.image.size}")

# 遍历表格
for table in doc.tables:
    print(table.export_to_markdown())

# 获取文本
text = doc.export_to_markdown()
```

## 访问文档属性

```python
doc = result.document

# 获取文本
text = doc.export_to_markdown()

# 页数
page_count = doc.num_pages

# 元数据 (通过 origin 属性)
origin = doc.origin
filename = origin.filename
mimetype = origin.mimetype
uri = origin.uri

# 页面访问
for page in doc.pages:
    print(f"Page {page.page_no}: size={page.size}")
```

## 完整示例

```python
from pathlib import Path
from docling.document_converter import DocumentConverter
from docling.datamodel.base_models import InputFormat
from docling.datamodel.pipeline_options import PdfPipelineOptions, EasyOcrOptions
from docling.document_converter import PdfFormatOption
from docling.chunking import HybridChunker

# 配置转换器
pipeline_options = PdfPipelineOptions(
    do_ocr=True,
    do_table_structure=True
)
pipeline_options.ocr_options = EasyOcrOptions()

converter = DocumentConverter(
    format_options={
        InputFormat.PDF: PdfFormatOption(pipeline_options=pipeline_options)
    }
)

# 转换文档
result = converter.convert(
    "document.pdf",
    max_num_pages=100
)

# 导出
print(result.document.export_to_markdown())

# 分块
chunker = HybridChunker(max_tokens=512)
chunks = list(chunker.chunk(result.document))
print(f"生成了 {len(chunks)} 个块")
```
