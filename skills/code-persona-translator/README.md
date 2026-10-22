# 代码人格翻译器

`code-persona-translator` 是一个开源的中文 Codex Agent Skill。它会在不修改代码、不虚构逻辑的前提下，把同一段源码分别解释成五岁小孩版、武侠版、老程序员版、面试回答版和严肃技术文档版。

## 特性

- 固定输出五种独立、完整的解释，顺序始终一致。
- 保持源码语义和实际行为，不为风格效果编造逻辑。
- 如实说明明显 bug、边界条件和潜在风险。
- 只解释代码，不自动重写或优化代码。
- 用户可以直接粘贴代码，无需回答额外问题。

## 安装

将整个 `code-persona-translator` 目录复制到支持 Agent Skills 的项目技能目录：

```powershell
New-Item -ItemType Directory -Force .cursor/skills | Out-Null
Copy-Item -Recurse -Force skills/code-persona-translator .cursor/skills/
```

如需作为个人 Skill 使用，可复制到个人技能目录：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE/.cursor/skills" | Out-Null
Copy-Item -Recurse -Force skills/code-persona-translator "$env:USERPROFILE/.cursor/skills/"
```

如果宿主只在启动时加载 Skill，安装后请重新打开会话。

## 使用

安装后直接粘贴代码并要求解释，或显式指定 Skill：

````text
请使用 code-persona-translator 解释下面的代码：

```python
def greet(name):
    return f"Hello, {name}!"
```
````

输出标题固定为：

```text
🔹 五岁小孩版
🔹 武侠版
🔹 老程序员版
🔹 面试回答版
🔹 严肃技术文档版
```

## 许可证

本 Skill 采用 [MIT License](LICENSE)。
