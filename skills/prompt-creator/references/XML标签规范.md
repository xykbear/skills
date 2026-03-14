# XML标签规范参考

系统提示词内部可使用XML标签引导LLM行为。

## 常用标签

### 响应格式控制
```xml
<ResponseFormat>
  回复应简洁直接，避免冗余解释
</ResponseFormat>
```

### 工具使用偏好
```xml
<ToolPreference>
  修改代码时优先使用edit而非write，除非创建新文件
</ToolPreference>
```

### 代码风格
```xml
<CodeStyle>
  使用现代语法，优先const/let
</CodeStyle>
```

### 避免事项
```xml
<AvoidExcessiveMarkdown>
  撰写报告时使用流畅散文，避免过多项目符号
</AvoidExcessiveMarkdown>
```

### 行动倾向
```xml
<DefaultToAction>
  默认实施更改而非仅建议
</DefaultToAction>

<DoNotActBeforeInstructions>
  除非明确指示，否则仅提供建议而非直接修改
</DoNotActBeforeInstructions>
```

### 研究行为
```xml
<InvestigateBeforeAnswering>
  绝不对未打开的代码进行推测
</InvestigateBeforeAnswering>
```

### 前端美学
```xml
<FrontendAesthetics>
  避免AI通用美学，选择独特字体和配色
</FrontendAesthetics>
```

### 状态跟踪
```xml
<StateTracking>
  使用JSON格式跟踪结构化状态，文本记录进度
</StateTracking>
```

### 子代理使用
```xml
<SubagentUsage>
  复杂任务使用子代理，简单任务直接处理
</SubagentUsage>
```
