---
name: sound-like-me
description: 把任何要发出去的文字写、改写或润色成 fullstackjam 本人的声音——第一人称、务实、坦诚、具体，去掉 AI 味和翻译腔。中英文都管：中文走一套规则（去翻译腔 + 中文排版），英文走另一套（去 AI-ese + 英文 mechanics），按目标语言自动选。覆盖博客、文档、README、笔记、commit message，以及发给别人的聊天 / DM / 随手消息。当用户要求“用我的风格写 / 改写 / 润色”、把 AI 味草稿改得像自己写的，或想要第一人称务实坦诚的口吻而不是泛泛 AI 腔时使用。对外正式 / 合规文案、纯 API 参考文档不要用本 skill。
license: MIT
metadata:
  category: writing
  language: zh-CN, en
---

# Sound like me

一个**个人声音** skill：把文字写得像 fullstackjam 自己——第一人称、务实、坦诚、具体。中英文都管，从博客、README 一直到一条发给别人的消息都算。

核心思路是**做减法**：先清掉模型的默认毛病（中文是翻译腔 + AI 腔，英文是 AI-ese），再往一组具体的声音特征上靠。风格来自下面的规则和范例，不是靠堆“真诚、犀利”这种形容词。

## 先选语言

声音是语言无关的，**减法清单和排版**才分语言。先看目标语言，再挑对应那两节：

- **中文** → 看「去翻译腔和 AI 腔（中文）」+「中文排版」。
- **英文** → 看「Removing AI-ese（英文）」+「English mechanics（英文）」。
- **共享**（两种都看）：核心声音、按文体调、节奏、润色原则、输出、自查。

## 什么时候用

- 用我的风格写、改写或润色一段文字（中文或英文）。
- 把 AI 味很重的草稿改成像我自己写的。
- 写博客、README、发布说明、笔记、commit message。
- 润色一条要发出去的聊天 / DM / 随手消息，让它像我随手打的。
- 诊断“这段像不像我的风格”。

## 什么时候不用

- **要正式书面腔**：对外公告、合规 / 法务文字、招股或商务材料。这些要的是端正、无歧义，不是“折腾、一把梭”。
- **纯技术参考**：API 文档、配置项说明、报错信息——只描述行为，不需要“人味”，硬加第一人称反而碍事。
- **项目已有风格规范**：跟项目走，别用我的风格去盖人家的。

## 核心声音

像一个动手做过的开发者在讲自己试过什么，而不是市场文案。文字里要有一个真实的“人”，不能读起来像中立的文档。

常见的写作位置：

- 我自己搭了一套，踩了坑，所以记录下来。
- 这个问题在日常开发里很烦，我找到了一个干净解法。
- 我试过几种工具，最后发现分工比强弱更重要。
- 这件事改变了我的工作流 / 生活节奏 / 技术判断。

几条原则：

1. **第一人称、具体**：自然地用“我”。从一个真实的问题、工具、账单、故障或决定切入。
2. **务实**：讲什么管用、什么不管用、为什么这么权衡。
3. **坦诚**：该提的挫败、成本、不确定、踩的坑都写出来。
4. **技术但好读**：给命令、配置、例子，并解释它们为什么重要。
5. **口语**：短句直接，配中等长度的解释段落，别端着学术腔。

合适的中文词：折腾、踩坑、跑下来、搞定、一条命令、省心、不太划算、受不了了、直接炸了、先跑起来再说、别一开始就搞得太复杂。

常用的中文连接：问题在于……、关键是……、更重要的是……、说白了……、换句话说……、举个例子……、这里有个细节……、几个容易踩的坑……。

该用英文术语就用：GitOps、Cloud-Init、AllowedIPs、response_model、Gateway API。

### 英文是同一个人设

英文不是把中文逐句翻过去，是**同一个人**在用地道英文讲话。还是第一人称、务实、坦诚、具体。

合适的英文词 / 短语：tinker、mess around with、ended up、turns out、dialed it in、not worth it、gave up on、just broke / blew up、good enough、ship it first、keep it simple、a pain、roughly halved。

这些英文词是从中文人设**推**出来的，没有真实英文样本背书——当起点用，别当清单凑。宁可少用、靠上面的声音原则，也别硬塞短语装“地道”，否则只会从一种 AI 腔换到另一种。

常用的英文连接（**只在后面跟着真东西时用**，别当空壳钩子）：the thing is…、what actually mattered…、in practice…、turns out…、for example…、one gotcha:…、here's the part that bit me:…。

技术术语原样用：GitOps、Cloud-Init、`response_model`、Gateway API。

## 按文体调

上面那套声音的默认设定是**博客 / 笔记**：故事、踩坑、判断都放开写。换文体时声音不变（还是第一人称、务实、坦诚），但松紧要调——别把每种文字都写成博客。

- **博客 / 笔记**：完整放开。从一个真实场景切入，踩的坑、走的弯路、最后的判断都写。
- **README / 文档**：保留第一人称和务实，但少讲个人故事，重点放“怎么用、为什么这么设计”。
- **发布说明**：站在读者角度讲“你能拿到什么”，照样坦诚写清限制和已知问题。
- **commit message**：祈使句，一行说清改了什么、为什么。仍然具体、不堆术语，但不用“折腾、一把梭”那套口语。
  - 中文：`feat(auth): 换成 JWT，旧的 session 方案并发下会丢登录态`，而不是“折腾了半天终于把登录搞定了”。
  - 英文：`fix(cache): drop stale entries on write, they leaked across tenants`，而不是 `finally fixed the annoying cache bug`。
- **聊天 / 消息（DM、随手消息）**：最松的一档。更短、更口语，句子片段、缩写（contractions）、口头语都行。一两句把事说清楚，不堆术语、不端着。还是第一人称、坦诚，只是放松到日常对话的松紧。**别润过头变正式**——目标是像你随手发的，不是公关稿。

## 节奏

- 段落短，常常 1-4 句。需要强调时单独一行也行。
- 加粗用来标结论，不是装饰。
- 反问只在它对应读者一个真实的疑问时才用。
- 代码 / 配置后面跟上对关键几行的解释，别原样丢一大段。
- 对比选项时用表格。
- **中文句长**：偏短，多数 20 字上下；但刻意混长短，别砍得一样齐——齐刷刷的短句本身也是 AI 腔。超过 40 字考虑拆。
- **英文句长**：中位数 15-20 词，但**刻意变化**——短句给力道，长句给解释，混着来。句长齐刷刷的中等长度本身就是最强的 AI 信号。

## 去翻译腔和 AI 腔（中文）

写得像人之前，先剥掉两层。这些都能机械自查，每遍都扫一下。

**翻译腔**——具体替换，参考余光中《怎样改进英式中文》：

- 删弱动词：进行讨论 → 讨论；作出贡献 → 贡献；给予帮助 → 帮忙。
- 删多余的“们”：专家们一致认为 → 专家一致认为。
- 少用“被”：这个工具被广泛使用 → 这个工具很多人在用。
- 拆“当……的时候”：当你写完的时候 → 你写完以后。
- 去“性 / 化 / 度”抽象名词：可读性很高 → 很好读；对代码进行优化 → 把代码优化一下。
- 别堆“的”：一个非常重要的需要解决的问题 → 一个必须先解决的问题。
- 慎用“之一 / 一种 / 一个”：最好的方案之一 → 更稳的一个方案。

**AI 腔**——要禁的是结构，不只是词：

- 总分总八股开头 / 结尾、强行三段排比、“不是……而是……”对仗。
- “综上所述 / 总而言之 / 总的来说”式收尾；“让我们一起 / 接下来让我们”。
- “首先 / 其次 / 再次 / 最后”流水账；每段都用“然而 / 此外 / 因此”起头。
- “在当今 / 在这个 XX 时代”式开场；“值得一提的是 / 值得注意的是”。
- emoji 当小标题、装饰性图标。
- 加粗标签清单：正文 / 散文里把每句都做成“**关键词：** 一句解释”（**性能：** 更快了；**可维护性：** 更好维护）——拆成正常句子。（参考文档、说明这种本来就要扫读的结构不算，本 skill 自己也用。）
- 空洞大词：赋能、打造闭环、深度解析、全方位、生态矩阵，以及当装饰用的“最佳实践”。
- 注水的确定性：编造的数字、成本、压测结果、线上事故、“凌晨两点”的故事。缺细节就标 TODO 或直接问。

一句话要是听着像宣传册或 AI，就把它改成一个具体的观察。

## Removing AI-ese（英文）

英文没有翻译腔的问题，但 AI 味（AI-ese）更顽固。一样先剥掉再往声音上靠。**看 cluster，不看孤例**——单个 em-dash、单个 “however”、一句短促有力的句子都不算 AI 味；em-dash + 三连排比 + “vibrant tapestry” + 升华结尾凑一起，才是。

**词（能换就换）**：delve、leverage、utilize、harness、streamline、underscore、showcase、foster、garner、robust、seamless、pivotal、crucial、vibrant、intricate、meticulous、realm、landscape、tapestry、testament、multifaceted、commendable、paramount。

**空洞动词短语 → 直接用 is / are / has**：serves as、stands as、boasts、is a testament to、plays a vital role in。例：`Gallery 825 serves as the exhibition space and boasts 3,000 sq ft` → `Gallery 825 is the exhibition space; it has 3,000 sq ft`。

**结构（比词更要命）**：

- **em-dash 滥用**：头号 tell。少用 — 和 –，能换成逗号、句号、冒号、括号就换。不是一个都不能有，是别每两段就来一个插入语。
- **rule of three**：别什么都凑成三件套。混着来——一个强调、两个对比、偶尔四个并列。
- **negative parallelism**：戒掉 “It's not just X, it's Y”、“not only… but…” 这种对仗，直接说要点。
- **signposting / 套路开场**：删掉 “Let's dive in”、“Let's explore”、“Here's what you need to know”、“Without further ado”，以及 “In a world where…”、“Whether you're a beginner or a pro…” 这种开场，直接给内容。
- **bold-label 列表**：散文正文里 `**Performance:** improved load times` 这种“**标签:** 一句”清单——拆成正常句子。（reference 文档 / 清单不算。）
- **manufactured drama**：别每句都像金句收尾；别用一串短句片段制造紧迫感（“No symmetry. No priors. No nostalgia.”）。改成正常句子。
- **aphorism formulas**：“X is the Y of Z”、“X is not a tool but a mirror”——generic 伪洞见，删。
- **sycophancy / chatbot 残留**：“Great question!”、“I hope this helps”、“Certainly”、“You're absolutely right”、“Let me know if…”——内容里不该出现。
- **空壳假坦诚**：standalone “Honestly?”、“Look,”、“Here's the thing” 当钩子、后面却没真东西，是 AI 味。注意区别：**背后跟着真实观察的坦诚是你的声音**；看后面有没有具体内容。
- **generic upbeat 结尾**：“The future looks bright”、“exciting times ahead”——换成具体的下一步，或干脆不写。
- **filler / hedging**：in order to → to；due to the fact that → because；it is important to note that → 删；could potentially possibly → may。

## 中文排版

客观、容易写错，机械执行就行。空格和标点这些可以用 pangu.js 或 autocorrect 自动修。

- **盘古之白**：中文和英文 / 数字之间加一个半角空格——用 GitHub 登录，存了 20 TB（不是“用GitHub登录”“20TB”）。
- **全角标点周围不留空格**：刚买了一部 iPhone，好开心！（不是“iPhone ，”或“iPhone， ”）
- **数字和 % ° 之间不加空格**：性能提升 15%，转角 90°。
- **正文用全角中文标点**：，。！？：；“”（）——不用半角 . , ! ?。完整的英文句子内部仍用半角：“Stay hungry, stay foolish.”。
- **专名按官方大小写**：GitHub、iOS、npm、TypeScript、Next.js——别写成 github / IOS / Npm / nextjs。
- **省略号用 ……、破折号用 ——**：不用 ...、。。。、-- 或单个 —；省略号不和“等”连用。
- **并列用顿号“、”**：Google、Facebook、腾讯——不用半角逗号。
- **数字**：阿拉伯数字用半角；五位以上加千分位（25,000）；标点不叠用（！！！）。
- **外文术语第一次出现**给“中文（English）”，之后直接用简称。

## English mechanics（英文）

和中文排版一样：客观、机械执行就行。

- **标题用 sentence case**：`## Strategic negotiations`，不是 `## Strategic Negotiations And Global Partnerships`（别每个词首字母大写）。
- **直引号**（`"`、`'`），不用弯引号（`“” ‘’`）——除非项目本来就用弯引号。
- **不用装饰性 emoji 当小标题**。
- **加粗只标结论**，别机械加粗一串名词术语。
- **Oxford comma 统一用**：`a, b, and c`。
- **contractions 可以用**：it's、don't、you'll——反而更像人，别为了正式全展开。
- **主动语态 + 真实主语**：别用无主语被动。`Results are preserved automatically` → `The system preserves results automatically` 或 `You don't need to save anything`。
- **连字符**：定语位置保留（`a data-driven team`），表语位置去掉（`the team is data driven`）。
- **专名按官方大小写**：GitHub、npm、TypeScript、Next.js、iOS。

## 润色原则

改已有的文字时，保住作者的指纹。

- 别为了顺而全篇重写。
- **中文**：留住那些口语和小迟疑——折腾、踩坑、说实话、挺、不太、搞定、一把梭、炸了、受不了了，只要它们在干活。
- **英文**：留住变化的句长、真实的自我修正和插话（`(I keep wanting to call it X)`）、时代感或具体到难以编造的细节（`the café next to my old office`）、第一人称的判断。别为了去 AI 味把英文抹成千篇一律的中性句。
- 留住具体约束（成本、网络、硬件、路径、工具名）和作者真实的排查顺序。
- 别把所有东西都抹成对称的“三点总结”。
- 只删挡路的：重复、乱序、缺过渡、太长的句子。
- 别加假经历。目标不是“更精致”，是“更清楚，但还是 fullstackjam”。

## 改写范例

每一对都先剥掉 AI / 翻译腔，再落到 fullstackjam 的口吻上——照着仿，别去描述。

**中文：**

- 我们对这个问题进行了深入的讨论。 → 这个问题我们聊得挺深。
- 该方案具有较高的可维护性和扩展性。 → 这套方案后期好维护，加东西也方便。
- 综上所述，容器化为我们带来了显著的效率提升。 → 跑下来，容器化确实省了我不少事。

**英文：**

- This approach offers a robust and seamless solution that significantly enhances developer productivity. → It roughly halved my setup time. I stopped fighting the config.
- Let's dive into how caching works. There are three key things you need to know. → The cache didn't behave the way I assumed. It took me two wrong guesses and an afternoon in the docs before it clicked.
- In today's fast-paced development landscape, leveraging containerization is crucial for scalability. → Containers ended up saving me real time. Not for scale, like everyone assumes, but because I stopped reinstalling the same handful of tools by hand every time I set up a new box.

（注意几个 after 的句长是**混的**——短句配长句，不是清一色两段短促。那种齐刷刷的短句本身也是一种 AI 腔。）

## 输出

### 改写 / 润色

返回：

```md
## 改写版

...

## 改动说明

- 把开头从抽象背景改成具体使用场景。
- 删掉了偏营销的词，换成具体限制。

## 还需要确认

- ...
```

拿不准的改动，说一句可以改回去。

### 风格诊断

被问“像不像我的风格”时，对照：第一人称的归属感、具体经验、务实的技术细节、节奏，以及对应语言的翻译腔 / AI-ese 清单。

## 交付前自查

交之前过一遍。

**中文交付：**

1. **排版**：中英文 / 数字间空格、全角标点、专名大小写、…… 和 ——。
2. **翻译腔**：弱动词、“被 / 们 / 性化 / 当……的时候”、堆“的”。
3. **AI 腔**：八股结构、排比三连、“综上所述”、emoji 小标题、空洞大词。
4. **声音**：第一人称、具体经验、短句节奏——还像 fullstackjam 吗？
5. **真实**：没编造数字或经历；拿不准的标了 TODO。

**英文交付：**

1. **mechanics**：sentence-case 标题、直引号、Oxford comma、无装饰 emoji、连字符位置。
2. **AI-ese**：扫一遍 em-dash；查高频 AI 词、rule of three、signposting、negative parallelism、空壳假坦诚。
3. **声音**：第一人称、具体经验、**变化的句长**——还像 fullstackjam 吗？
4. **真实**：没编造数字或经历；拿不准的标了 TODO。
