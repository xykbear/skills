# CLI 完整参考

本文档提供 Docling 命令行工具的完整选项参考。当基础命令无法满足需求时，请查阅本文档。

## 基本用法

```bash
docling <source> [options]
```

## 源 (source)

- 本地文件: `./document.pdf`
- URL: `https://example.com/doc.pdf`
- 目录: `./docs/`

## Pipeline 选项

| 选项 | 说明 | 使用场景 |
|------|------|----------|
| `--pipeline` | 处理 pipeline | 需要使用 VLM 或 ASR 时 |
| `--vlm-model` | VLM 模型 | 需要视觉语言模型理解时 |
| `--asr-model` | ASR 模型 | 处理音频/视频时 |

### Pipeline 类型

```bash
# 标准 pipeline (默认)
docling --pipeline standard document.pdf

# VLM pipeline - 使用视觉语言模型
docling --pipeline vlm --vlm-model granite_docling document.pdf

# ASR pipeline - 音频/视频转写
docling --pipeline asr --asr-model whisper_small audio.mp3

# Legacy pipeline
docling --pipeline legacy document.pdf
```

### VLM 模型

| 模型 | 说明 |
|------|------|
| `granite_docling` | IBM Granite Docling (默认) |
| `smoldocling` | 小型 Docling 模型 |
| `deepseek_ocr` | DeepSeek OCR |
| `granite_vision` | Granite Vision |
| `pixtral` | Pixtral |
| `got_ocr` | GOT OCR |
| `phi4` | Phi-4 |
| `qwen` | Qwen |
| `gemma_12b` | Gemma 12B |
| `gemma_27b` | Gemma 27B |
| `dolphin` | Dolphin |

### ASR 模型

| 模型 | 说明 |
|------|------|
| `whisper_tiny` | Whisper Tiny (默认) |
| `whisper_small` | Whisper Small |
| `whisper_medium` | Whisper Medium |
| `whisper_base` | Whisper Base |
| `whisper_large` | Whisper Large |
| `whisper_turbo` | Whisper Turbo |
| `whisper_tiny_mlx` | Whisper Tiny (MLX 加速) |
| `whisper_small_mlx` | Whisper Small (MLX 加速) |
| `whisper_medium_mlx` | Whisper Medium (MLX 加速) |
| `whisper_base_mlx` | Whisper Base (MLX 加速) |
| `whisper_large_mlx` | Whisper Large (MLX 加速) |
| `whisper_turbo_mlx` | Whisper Turbo (MLX 加速) |
| `whisper_tiny_native` | Whisper Tiny Native |
| `whisper_small_native` | Whisper Small Native |
| `whisper_medium_native` | Whisper Medium Native |
| `whisper_base_native` | Whisper Base Native |
| `whisper_large_native` | Whisper Large Native |
| `whisper_turbo_native` | Whisper Turbo Native |

> MLX 加速仅在 Apple Silicon 可用

## OCR 选项

| 选项 | 说明 | 使用场景 |
|------|------|----------|
| `--ocr` / `--no-ocr` | 启用/禁用 OCR | 处理扫描版 PDF 或图片时 |
| `--force-ocr` | 强制全页 OCR | 需要对所有页面进行 OCR 时 |
| `--ocr-engine` | OCR 引擎 | 需要指定特定 OCR 引擎时 |
| `--ocr-lang` | OCR 语言 | 处理非英文文档时 |
| `--psm` | 页面分割模式 | 需要精细控制 OCR 时 |

### OCR 引擎

```bash
# 自动选择 (默认)
docling --ocr-engine auto document.pdf

# 指定引擎
docling --ocr-engine easyocr document.pdf
docling --ocr-engine tesserocr document.pdf
docling --ocr-engine rapidocr document.pdf
docling --ocr-engine ocrmac document.pdf
```

### OCR 语言

```bash
# 英文
docling --ocr-lang eng document.pdf

# 多语言
docling --ocr-lang eng,chi_sim,chi_tra,jpn,kor document.pdf

# 各种语言代码
# eng, chi_sim (简体中文), chi_tra (繁体中文), jpn, kor, fra, deu, spa, etc.
```

## 表格与布局

| 选项 | 说明 | 使用场景 |
|------|------|----------|
| `--tables` / `--no-tables` | 表格识别 | 提取表格数据时 |
| `--table-mode` | 表格模式 | 需要平衡速度/精度时 |
| `--show-layout` | 显示布局框 | 调试/可视化时 |

```bash
# 启用表格识别 (默认)
docling --tables document.pdf

# 快速模式 (更快)
docling --table-mode fast document.pdf

# 精确模式 (默认)
docling --table-mode accurate document.pdf

# 显示布局
docling --show-layout document.pdf
```

## Enrichments (丰富功能)

| 选项 | 说明 | 使用场景 |
|------|------|----------|
| `--enrich-code` | 代码识别 | 提取代码块时 |
| `--enrich-formula` | 公式识别 | 提取数学公式时 |
| `--enrich-picture-classes` | 图片分类 | 识别图片类型时 |
| `--enrich-picture-description` | 图片描述 | 生成图片说明时 |
| `--enrich-chart-extraction` | 图表提取 | 将图表转为表格时 |

```bash
# 启用多项 enrichment
docling --enrich-code --enrich-formula --enrich-picture-classes document.pdf
```

## 设备与性能

| 选项 | 说明 | 使用场景 |
|------|------|----------|
| `--device` | 计算设备 | 需要指定 CPU/GPU 时 |
| `--num-threads` | 线程数 | 调整并行度时 |
| `--page-batch-size` | 页面批大小 | 调整内存使用时 |

```bash
# 指定设备
docling --device cpu document.pdf       # CPU
docling --device cuda document.pdf       # NVIDIA GPU
docling --device mps document.pdf       # Apple Silicon
docling --device auto document.pdf      # 自动选择 (默认)

# 调整性能
docling --num-threads 8 document.pdf
docling --page-batch-size 8 document.pdf
```

## PDF 选项

| 选项 | 说明 | 使用场景 |
|------|------|----------|
| `--pdf-backend` | PDF 后端 | 特定解析问题时 |
| `--pdf-password` | PDF 密码 | 处理加密 PDF 时 |

```bash
# PDF 后端
docling --pdf-backend pypdfium2 document.pdf
docling --pdf-backend docling_parse document.pdf

# 加密 PDF
docling --pdf-password yourpassword document.pdf
```

## 输入/输出选项

| 选项 | 说明 |
|------|------|
| `--from FORMAT` | 指定输入格式 |
| `--to FORMAT` | 指定输出格式 |
| `--output PATH` | 输出目录 |
| `--image-export-mode` | 图片导出模式 |

### 输出格式

```bash
docling --to md document.pdf        # Markdown
docling --to json document.pdf      # JSON (无损)
docling --to html document.pdf      # HTML
docling --to text document.pdf     # 纯文本
docling --to yaml document.pdf     # YAML
docling --to html_split_page document.pdf  # HTML 每页单独文件
docling --to doctags document.pdf  # Doctags
docling --to vtt document.pdf      # WebVTT
```

### 图片导出模式

```bash
docling --image-export-mode embedded document.pdf  # Base64 内嵌 (默认)
docling --image-export-mode referenced document.pdf # 引用外部文件
docling --image-export-mode placeholder document.pdf # 仅占位符
```

## 其他选项

| 选项 | 说明 |
|------|------|
| `--artifacts-path` | 本地模型路径 |
| `--enable-remote-services` | 允许远程服务 |
| `--abort-on-error` | 遇错停止 |
| `-v, --verbose` | 详细输出 |
| `--version` | 显示版本 |
| `--profiling` | 性能分析 |

```bash
# 本地模型
docling --artifacts-path /local/models document.pdf

# 允许远程服务
docling --enable-remote-services document.pdf

# 调试
docling -v document.pdf          # Info 日志
docling -vv document.pdf         # Debug 日志
docling --debug-visualize-layout document.pdf  # 可视化布局

# 性能分析
docling --profiling document.pdf
docling --save-profiling document.pdf
```

## 使用示例

### 处理扫描版 PDF

```bash
docling --ocr --force-ocr --ocr-lang eng,chi_sim scan.pdf
```

### 批量处理

```bash
docling --output ./output/ ./docs/
docling --from pdf --from docx --to json ./documents/
```

### 使用 VLM

```bash
# 基本 VLM
docling --pipeline vlm --vlm-model granite_docling document.pdf
```

### 处理音频/视频

```bash
# 音频转写
docling --pipeline asr --asr-model whisper_small audio.mp3

# 视频转写
docling --pipeline asr video.mp4
```

### 完整配置

```bash
docling \
  --pipeline standard \
  --ocr \
  --tables \
  --table-mode accurate \
  --enrich-code \
  --enrich-formula \
  --device auto \
  --num-threads 4 \
  --to json \
  --output ./output/ \
  document.pdf
```
