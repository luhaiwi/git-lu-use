# 中文 Agent Skills

这是一个采用 MIT License 的开源中文 Agent Skills 项目。目前仓库提供两个可独立安装的 Skill：代码人格翻译器和专业书籍内容概括助手。

本项目只提供 Agent Skill 指令文件，不包含独立的命令行程序、Web 页面或 API。使用前需要将所需 Skill 安装到支持 Agent Skills 的 Codex 环境。

## 1. 项目解决什么问题

通用模型在解释代码或概括书籍时，输出格式和表达风格容易不稳定，也可能出现脱离原内容、遗漏关键信息或虚构细节的问题。本项目通过可复用的 Skill 指令，为两类常见任务提供明确的处理规则和固定输出结构：

- `code-persona-translator`：解决同一段代码难以面向不同受众进行一致、准确讲解的问题。它要求五种解释都忠于源码行为，不修改功能，也不为追求风格而编造逻辑。
- `professional-book-summarizer`：解决书籍概述中版本指代不清、内容堆砌、小说剧透以及资料不足时虚构内容的问题。它要求先核实书籍信息，再按固定结构进行概括。

## 2. 主要功能

### 代码人格翻译器

源码位于 [`skills/code-persona-translator/`](skills/code-persona-translator/)。主要功能包括：

- 接收用户提供的一段代码，并针对同一段代码生成五个独立版本的解释。
- 固定按照“五岁小孩版 → 武侠版 → 老程序员版 → 面试回答版 → 严肃技术文档版”的顺序输出。
- 保持解释与源码语义和实际行为一致，不添加源码中不存在的业务逻辑。
- 如实说明代码中明显的 bug、边界条件和潜在隐患。
- 只解释代码，不自动修改、重构或优化代码。
- 即使代码很短，也完整输出五个版本。

### 专业书籍内容概括助手

源码位于 [`skills/professional-book-summarizer/`](skills/professional-book-summarizer/)。主要功能包括：

- 根据书名核实作者、出版年代和作品类型。
- 遇到同名作品时列出区分信息，并等待用户确认具体版本。
- 固定输出 `【核心主旨】`、`【内容简述】` 和 `【价值与局限】` 三部分。
- 小说类内容避免泄露关键结局和核心悬念。
- 社科类内容重点呈现主要论证框架，而不是只罗列结论。
- 信息不足时明确说明，不虚构情节、观点或出版信息。
- 支持精简版、学术版和读书分享演讲稿等追加要求。

## 3. 安装方法

### 前置条件

需要一个支持 Agent Skills 的 Codex 环境。仓库结构如下：

```text
skills/
├── code-persona-translator/
│   ├── SKILL.md
│   ├── README.md
│   └── LICENSE
└── professional-book-summarizer/
    ├── SKILL.md
    ├── README.md
    └── LICENSE
```

### 安装到当前项目

在本仓库根目录运行以下 PowerShell 命令。可以安装其中一个 Skill，也可以同时安装两个：

```powershell
New-Item -ItemType Directory -Force .cursor/skills | Out-Null

# 安装代码人格翻译器
Copy-Item -Recurse -Force skills/code-persona-translator .cursor/skills/

# 安装专业书籍内容概括助手
Copy-Item -Recurse -Force skills/professional-book-summarizer .cursor/skills/
```

项目级安装只对当前项目生效。

### 安装为个人 Skill

如需在本机所有项目中使用，可复制到个人技能目录：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE/.cursor/skills" | Out-Null

Copy-Item -Recurse -Force skills/code-persona-translator "$env:USERPROFILE/.cursor/skills/"
Copy-Item -Recurse -Force skills/professional-book-summarizer "$env:USERPROFILE/.cursor/skills/"
```

如果宿主只在启动时加载 Skill，安装完成后需要重新打开会话。

## 4. 使用方法

### 使用代码人格翻译器

直接粘贴需要解释的代码，也可以显式指定 Skill：

````text
请使用 code-persona-translator 解释下面的代码：

```c
for (int i = 0; i < 3; i++) {
    printf("%d\n", i);
}
```
````

Skill 会依次生成以下五个部分：

```text
🔹 五岁小孩版
🔹 武侠版
🔹 老程序员版
🔹 面试回答版
🔹 严肃技术文档版
```

### 使用专业书籍内容概括助手

提供书名即可，也可以追加希望采用的表达形式：

```text
请使用 professional-book-summarizer 概括《平凡的世界》。
```

```text
请使用 professional-book-summarizer，以适合读书分享演讲稿的形式概括《平凡的世界》。
```

如果存在多部同名作品，应先根据 Skill 给出的作者、年代和作品类型确认具体版本。

## 5. 输入输出示例

以下示例用于展示固定结构，实际措辞会根据输入内容变化。

### 示例一：解释代码

输入：

```c
int add(int a, int b) {
    return a + b;
}
```

输出节选：

```text
🔹 五岁小孩版
把两个数字放在一起算一算，再把得到的答案交出来。

🔹 武侠版
两枚数字信物被送入“add”这招功法，合力相加后，所得结果被交还给来者。

🔹 老程序员版
这是个最基础的整数加法函数，接收两个 int，相加后直接返回。写法简单，但仍受 int 取值范围限制，溢出时不能假定结果仍是数学意义上的和。

🔹 面试回答版
该函数接收两个整数参数并返回两者之和。核心逻辑是执行一次整数加法，时间复杂度为 O(1)。需要注意有符号整数溢出的边界问题。

🔹 严肃技术文档版
【功能概述】接收两个 int 类型参数并返回其算术和。
【输入约束】输入值应保证相加结果可由 int 类型表示。
【输出行为】返回 a + b 的计算结果。
```

### 示例二：概括书籍

输入：

```text
请概括《平凡的世界》。
```

输出结构：

```text
书籍信息：作者、出版年代、作品类型。

【核心主旨】：一句话概括作品的核心思想或故事主线。

【内容简述】：在不泄露关键结局的前提下，梳理主要人物、情节或论点脉络。

【价值与局限】：说明作品的阅读价值、适用人群以及客观局限。
```

## 项目文件

- [`skills/code-persona-translator/SKILL.md`](skills/code-persona-translator/SKILL.md)：代码人格翻译器的规则与触发描述。
- [`skills/professional-book-summarizer/SKILL.md`](skills/professional-book-summarizer/SKILL.md)：专业书籍内容概括助手的规则与触发描述。
- [`LICENSE`](LICENSE)：项目的 MIT 开源许可证。

## 许可证

本项目采用 [MIT License](LICENSE)。各 Skill 目录同时包含许可证副本，便于单独分发。
