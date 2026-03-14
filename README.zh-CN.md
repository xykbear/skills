[English](./README.md) | [中文](./README.zh-CN.md)

---

# xykbear-skills

个人 AI Agent Skills 集合，为 AI 编码助手扩展能力。

## 安装

```bash
npx skills add xykbear/skills
```

或手动安装：

```bash
git clone https://github.com/xykbear/skills.git
cp -r skills/{skill-name} ~/.agents/skills/
```

## 可用 Skills

### fmhy-lookup

搜索 FMHY (Free Media Heck Yeah) wiki 获取免费工具、软件和网站推荐。

**使用场景：**
- 查找免费工具、软件、app、网站或资源推荐
- 寻找视频下载器、VPN、广告拦截器、密码管理器、代理工具、下载工具等

**使用示例：**
- "帮我找一个免费的视频下载器"
- "有什么好的密码管理器推荐？"
- "推荐一些免费的代理工具"

### prompt-creator

帮助用户创建、优化和沉淀系统提示词 (AGENTS.md)。

**使用场景：**
- 用户想要把对话经验沉淀为规则
- 询问"是否有可提炼的系统提示词"
- 想要优化/审查现有 agents.md 配置

**使用示例：**
- "是否有可提炼的系统提示词"
- "把这次的经验沉淀下去"
- "看看当前提示词是否有问题"

## 使用方式

Skills 安装后自动可用。当检测到相关任务时，Agent 会自动调用对应的 skill。

只需自然地描述你的需求 - 无需特殊命令。

## 许可证

MIT License - 见 [LICENSE.txt](./LICENSE.txt)
