# 专业书籍内容概括助手

`professional-book-summarizer` 是一个开源的中文 Agent Skill，用于根据书名生成结构统一、信息谨慎且兼顾阅读价值的书籍概述。

本项目目前只包含 Skill 指令文件，不提供独立的命令行程序、Web 界面或 API。使用时需要将它安装到支持 Agent Skills 的 Codex 环境中。

## 项目解决什么问题

一般的书籍摘要容易出现版本指代不清、内容堆砌、小说关键结局被提前泄露，以及模型在资料不足时补写不存在情节等问题。本项目通过明确的核实流程、固定的输出结构和字数限制，让书籍概述更适合快速了解作品、选书判断与读书分享准备。

当书名存在多个同名作品时，Skill 会先列出作者、出版年代、作品类型等区分信息，请用户确认具体作品；当资料不足时，会直接说明信息不足，而不是虚构内容。

## 主要功能

- 核实作者、出版年代和作品类型等基础信息。
- 遇到同名书籍时先进行版本区分，不直接猜测用户所指作品。
- 使用固定的三部分结构输出：
  - `【核心主旨】`：80 字以内概括全书最核心的思想或故事主线。
  - `【内容简述】`：以 200–350 字梳理主要情节或论点脉络。
  - `【价值与局限】`：以 100–180 字说明阅读价值、适用人群和作品局限。
- 小说类内容避免泄露关键结局和核心悬念。
- 社科类内容完整呈现主要论证框架，不只罗列结论。
- 保持书面、简洁、客观的中文表达，资料不足时禁止虚构。
- 支持在用户指令中追加“精简版”“学术版”或“读书分享演讲稿”等形式要求。

## 安装方法

### 前置条件

需要一个支持 Agent Skills 的 Codex 环境。本仓库中的 Skill 源码位于：

```text
skills/professional-book-summarizer/
├── SKILL.md
├── README.md
└── LICENSE
```

### 安装到当前项目

在本项目根目录运行以下 PowerShell 命令，将 Skill 复制到项目级技能目录：

```powershell
New-Item -ItemType Directory -Force .cursor/skills | Out-Null
Copy-Item -Recurse -Force skills/professional-book-summarizer .cursor/skills/
```

项目级安装只对当前项目生效。

### 安装为个人 Skill

如需在本机所有项目中使用，可复制到个人技能目录：

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE/.cursor/skills" | Out-Null
Copy-Item -Recurse -Force skills/professional-book-summarizer "$env:USERPROFILE/.cursor/skills/"
```

如果宿主只在启动时加载 Skill，安装后需要重新打开会话。

## 使用方法

在支持 Agent Skills 的会话中直接提供书名，也可以明确指定 Skill：

```text
请使用 professional-book-summarizer 概括《平凡的世界》。
```

需要特定表达形式时，在书名后追加要求：

```text
请使用 professional-book-summarizer，以适合读书分享演讲稿的形式概括《平凡的世界》。
```

如果书名对应多部作品，先根据 Skill 给出的候选信息确认作者或版本，再继续生成概述。

## 输入输出示例

### 输入

```text
请使用 professional-book-summarizer 概括《平凡的世界》。
```

### 输出

实际措辞会因模型和可用资料而变化，但结构与内容约束保持一致。

```text
书籍信息：作者路遥；第一部于1986年出版，第二部于1988年出版，第三部于1989年出版；中国当代现实主义长篇小说。

【核心主旨】：通过孙少安、孙少平兄弟在时代变迁中的生活选择，展现普通人以劳动、尊严与韧性抵抗贫困和命运压力。

【内容简述】：小说以20世纪70年代中期至80年代中期的陕北城乡为背景，从贫困农家子弟孙少平的求学经历与精神成长写起，并以其兄孙少安立足乡土、谋求生活改善的实践作为另一条主线。兄弟二人在城乡流动、劳动选择、家庭责任和情感关系中走上不同道路：一人渴望突破环境限制，在更广阔的世界中确认自我；一人承担家庭与乡村现实，在改革带来的机会和风险中艰难创业。围绕孙家及其亲友，作品展开农民、干部、知识青年和产业工人的多重生活，呈现贫困、身份差异、城乡分化与社会转型对个人命运的塑造。全书并未把奋斗简化为成功叙事，而是反复追问人在挫折、损失和有限选择中如何保持尊严、责任与对生活的热情。

【价值与局限】：本书的价值在于以宏阔社会背景连接普通人的日常处境，强调劳动、自尊、责任和精神追求，对关注个人成长、城乡变迁及改革初期社会生活的读者具有启发。其现实主义叙事清晰、情感力量充沛，但篇幅较长，部分议论和理想化表达较突出，女性人物及若干配角的主体性相对有限；偏好高度实验性、节奏紧凑叙事的读者可能会感到缓慢。
```

## 项目文件

- [`skills/professional-book-summarizer/SKILL.md`](skills/professional-book-summarizer/SKILL.md)：Skill 的实际规则与触发描述。
- [`skills/professional-book-summarizer/README.md`](skills/professional-book-summarizer/README.md)：Skill 目录内的简要说明。
- [`skills/professional-book-summarizer/LICENSE`](skills/professional-book-summarizer/LICENSE)：MIT 开源许可证。

## 许可证

本项目采用 [MIT License](LICENSE)。
