# 框架集成完整参考

本文档详细介绍 Docling 与各 AI 框架的集成方法。当需要构建 RAG pipeline 或使用特定框架时，请查阅本文档。

## LlamaIndex

### 安装

```bash
pip install llama-index-readers-docling llama-index-node-parser-docling
```

### DoclingReader

将文档加载为 LlamaIndex Document：

```python
from llama_index.readers.docling import DoclingReader

reader = DoclingReader()
documents = reader.load_data(path="./document.pdf")

# 批量加载
documents = reader.load_data(path=["./doc1.pdf", "./doc2.docx"])
```

#### 参数

| 参数 | 类型 | 说明 |
|------|------|------|
| path | str/list | 文件路径或路径列表 |
| converter | DocumentConverter | 自定义转换器 |
| export_format | str | 导出格式: "markdown" (默认), "json", "text" |

#### 导出格式

```python
# Markdown (默认)
reader = DoclingReader(export_format="markdown")
docs = reader.load_data(path="./document.pdf")

# JSON (无损)
reader = DoclingReader(export_format="json")
docs = reader.load_data(path="./document.pdf")

# 纯文本
reader = DoclingReader(export_format="text")
docs = reader.load_data(path="./document.pdf")
```

### DoclingNodeParser

将 Docling 文档解析为节点：

```python
from llama_index.node_parser.docling import DoclingNodeParser

parser = DoclingNodeParser()
nodes = parser.get_nodes_from_documents(llama_index_documents)
```

### 完整 RAG 示例

```python
from llama_index.readers.docling import DoclingReader
from llama_index.node_parser.docling import DoclingNodeParser
from llama_index.core import VectorStoreIndex
from llama_index.embeddings.openai import OpenAIEmbedding

# 加载文档
reader = DoclingReader()
documents = reader.load_data(path="./document.pdf")

# 解析为节点
parser = DoclingNodeParser()
nodes = parser.get_nodes_from_documents(documents)

# 创建索引
embeddings = OpenAIEmbedding()
index = VectorStoreIndex(nodes, embed_model=embeddings)

# 查询
query_engine = index.as_query_engine()
response = query_engine.query("文档的主要内容是什么？")
print(response)
```

### 自定义转换器

```python
from docling.document_converter import DocumentConverter
from llama_index.readers.docling import DoclingReader
from docling.datamodel.base_models import InputFormat
from docling.datamodel.pipeline_options import PdfPipelineOptions
from docling.document_converter import PdfFormatOption

# 创建自定义转换器
pipeline_options = PdfPipelineOptions(do_ocr=True)
converter = DocumentConverter(
    format_options={
        InputFormat.PDF: PdfFormatOption(pipeline_options=pipeline_options)
    }
)

# 使用自定义转换器
reader = DoclingReader(converter=converter)
docs = reader.load_data(path="./document.pdf")
```

## LangChain

### 安装

```bash
pip install langchain-docling
```

### DoclingLoader

```python
from langchain_docling import DoclingLoader

loader = DoclingLoader(path="./document.pdf")
documents = loader.load()
```

#### 参数

| 参数 | 类型 | 说明 |
|------|------|------|
| path | str/list | 文件路径或 URL |
| converter | DocumentConverter | 自定义转换器 |
| export_format | str | 导出格式: "markdown", "json", "text" |

#### 导出格式

```python
# Markdown (默认)
loader = DoclingLoader(path="./document.pdf", export_format="markdown")

# JSON (无损)
loader = DoclingLoader(path="./document.pdf", export_format="json")

# 纯文本
loader = DoclingLoader(path="./document.pdf", export_format="text")
```

### 完整 RAG 示例

```python
from langchain_docling import DoclingLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.vectorstores import Chroma
from langchain_openai import OpenAIEmbeddings
from langchain.chains import RetrievalQA
from langchain_openai import OpenAI

# 加载文档
loader = DoclingLoader(path="./document.pdf")
docs = loader.load()

# 分块
splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
chunks = splitter.split_documents(docs)

# 向量化存储
embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(chunks, embeddings)

# 创建 QA chain
llm = OpenAI()
qa = RetrievalQA.from_chain_type(llm=llm, chain_type="stuff", retriever=vectorstore.as_retriever())

# 查询
result = qa({"query": "文档的主要内容是什么？"})
print(result["result"])
```

### 批量加载

```python
from langchain_docling import DoclingLoader

loader = DoclingLoader()
documents = loader.load([
    "./doc1.pdf",
    "./doc2.docx",
    "https://example.com/doc.pdf"
])
```

### 自定义转换器

```python
from docling.document_converter import DocumentConverter
from langchain_docling import DoclingLoader
from docling.datamodel.base_models import InputFormat
from docling.datamodel.pipeline_options import PdfPipelineOptions
from docling.document_converter import PdfFormatOption

pipeline_options = PdfPipelineOptions(do_ocr=True)
converter = DocumentConverter(
    format_options={
        InputFormat.PDF: PdfFormatOption(pipeline_options=pipeline_options)
    }
)

loader = DoclingLoader(path="./document.pdf", converter=converter)
docs = loader.load()
```

## Haystack

### 安装

```bash
pip install docling-haystack
```

### 使用

```python
from haystack import Document
from docling_haystack import DoclingConverter

converter = DoclingConverter()
documents = converter.convert("document.pdf")

# 或批量转换
docs = converter.convert(["doc1.pdf", "doc2.pdf"])
```

## Crew AI

Docling 可作为 Crew AI 的文档加载器：

```python
from crewai import Agent, Task, Crew
from langchain_docling import DoclingLoader

# 加载文档
loader = DoclingLoader(path="./document.pdf")
docs = loader.load()

# 创建任务
task = Task(
    description=f"分析以下文档内容: {docs[0].page_content}",
    agent=agent
)
```

## 其他框架

### 与 Data Prep Kit 集成

```python
# 用于大规模文档处理和数据准备
from docling.document_converter import DocumentConverter
from datprepkit import TextChunker
```

### 与 spaCy 集成

```python
import spacy
from docling.document_converter import DocumentConverter

converter = DocumentConverter()
result = converter.convert("document.pdf")
nlp = spacy.load("en_core_web_sm")

doc = nlp(result.document.text)
```

### 向量数据库集成

#### Milvus

```python
from langchain_docling import DoclingLoader
from langchain.vectorstores import Milvus
from langchain.embeddings import OpenAIEmbeddings

loader = DoclingLoader(path="./document.pdf")
docs = loader.load()

embeddings = OpenAIEmbeddings()
vectorstore = Milvus.from_documents(docs, embeddings)
```

#### Weaviate

```python
from langchain_docling import DoclingLoader
from langchain.vectorstores import Weaviate
from langchain.embeddings import OpenAIEmbeddings

loader = DoclingLoader(path="./document.pdf")
docs = loader.load()

embeddings = OpenAIEmbeddings()
vectorstore = Weaviate.from_documents(docs, embeddings, client=weaviate_client)
```

#### Qdrant

```python
from langchain_docling import DoclingLoader
from langchain.vectorstores import Qdrant
from langchain.embeddings import OpenAIEmbeddings

loader = DoclingLoader(path="./document.pdf")
docs = loader.load()

embeddings = OpenAIEmbeddings()
vectorstore = Qdrant.from_documents(docs, embeddings, host="localhost", collection_name="docs")
```

## 批量处理模式

### 多文档处理

```python
from pathlib import Path
from docling.document_converter import DocumentConverter
from llama_index.readers.docling import DoclingReader
from llama_index.core import VectorStoreIndex
from llama_index.node_parser.docling import DoclingNodeParser

# 方式 1: 使用 DocumentConverter
converter = DocumentConverter()
files = list(Path("./docs").glob("**/*.*"))

all_docs = []
for f in files:
    if f.suffix.lower() in ['.pdf', '.docx', '.md']:
        result = converter.convert(str(f))
        all_docs.append(result.document)

# 方式 2: 使用 LlamaIndex Reader
reader = DoclingReader()
documents = []
for f in files:
    if f.suffix.lower() in ['.pdf', '.docx', '.md']:
        docs = reader.load_data(path=str(f))
        documents.extend(docs)

# 创建索引
parser = DoclingNodeParser()
nodes = parser.get_nodes_from_documents(documents)
index = VectorStoreIndex(nodes)
```

## 性能优化

### 并行处理

```python
from concurrent.futures import ThreadPoolExecutor
from docling.document_converter import DocumentConverter

def process_file(filepath):
    converter = DocumentConverter()
    return converter.convert(filepath)

files = ["doc1.pdf", "doc2.pdf", "doc3.pdf"]

with ThreadPoolExecutor(max_workers=4) as executor:
    results = list(executor.map(process_file, files))
```

### 内存优化

```python
# 分批处理大文档
from docling.document_converter import DocumentConverter

converter = DocumentConverter()

# 处理大文档时限制页数
result = converter.convert(
    "large_document.pdf",
    max_num_pages=50  # 每批处理 50 页
)

# 继续处理剩余页
# (需要自行管理页面偏移)
```

## 最佳实践

1. **选择合适的分块大小**: 根据下游任务调整 (512-1024 tokens)
2. **使用元数据**: 在 RAG 中保留文档来源、页码等信息
3. **批量处理**: 大量文档时使用并行处理
4. **缓存转换结果**: 避免重复转换
5. **选择导出格式**: 
   - 简单 RAG: Markdown
   - 需要完整布局信息: JSON
   - 只需文本: Text
