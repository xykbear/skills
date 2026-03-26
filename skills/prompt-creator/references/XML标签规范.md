# XML标签规范参考

## 为什么使用 XML 标签

XML 标签帮助 LLM 解析复杂提示词，特别是在混合指令、上下文、示例和变量输入时。为每种内容类型使用独立的标签可以减少歧义，让指令更清晰明确。

**核心优势**：
- **结构化**：将不同类型的内容分隔开，避免混淆
- **明确性**：标签名称本身传达内容用途
- **可维护性**：修改特定部分的指令时不会影响其他部分

## 何时使用XML标签

**使用场景**：
- 行为约束（如"避免过度思考"）
- 格式要求（如"输出格式"）
- 工具偏好（如"优先使用edit"）
- 默认行为倾向（如"默认实施更改"）

**不使用场景**：
- 简单的一句话规则
- 临时性指令
- 可用自然语言清晰表达的内容

**命名规范**：
- **必须使用 snake_case**（小写加下划线，如`<default_to_action>`）
- 理由：snake_case 对 LLM 的 token 分词更友好，下划线明确分隔单词边界，减少歧义
- 名称应能表达标签目的

## 常用标签

### 响应格式控制
```xml
<response_format>
  回复应简洁直接，避免冗余解释
</response_format>
```

### 工具使用偏好
```xml
<tool_preference>
  修改代码时优先使用edit而非write，除非创建新文件
</tool_preference>
```

### 代码风格
```xml
<code_style>
  使用现代语法，优先const/let
</code_style>
```

### 避免事项
```xml
<avoid_excessive_markdown>
  撰写报告时使用流畅散文，避免过多项目符号
</avoid_excessive_markdown>
```

### 行动倾向
```xml
<default_to_action>
  默认实施更改而非仅建议
</default_to_action>

<do_not_act_before_instructions>
  除非明确指示，否则仅提供建议而非直接修改
</do_not_act_before_instructions>
```

### 研究行为
```xml
<investigate_before_answering>
  绝不对未打开的代码进行推测
</investigate_before_answering>
```

### 前端美学
```xml
<frontend_aesthetics>
  避免AI通用美学，选择独特字体和配色
</frontend_aesthetics>
```

### 状态跟踪
```xml
<state_tracking>
  使用JSON格式跟踪结构化状态，文本记录进度
</state_tracking>
```

### 子代理使用
```xml
<subagent_usage>
  复杂任务使用子代理，简单任务直接处理
</subagent_usage>
```

## 更多实用标签示例

### 避免过度思考
```xml
<avoid_overthinking>
  当决定如何处理问题时，选择一种方法并坚持下去。
  除非遇到直接与推理相矛盾的新信息，否则避免重新审视决定。
</avoid_overthinking>
```

### 沟通风格
```xml
<communication_style>
  提供基于事实的进度报告，使用流畅的散文段落。
  保持简洁除非用户要求详细摘要。
</communication_style>
```

### 并行工具调用
```xml
<use_parallel_tool_calls>
  如果打算调用多个工具且工具调用之间没有依赖关系，
  并行进行所有独立的工具调用以提高效率。
</use_parallel_tool_calls>
```

### 错误处理偏好
```xml
<error_handling>
  遇到错误时，先分析根本原因再尝试修复。
  不要简单地重试或忽略错误。
</error_handling>
```

### 代码审查标准
```xml
<code_review>
  审查代码时关注：
  1. 逻辑正确性
  2. 边界情况处理
  3. 性能影响
  4. 可读性和维护性
</code_review>
```
