---
name: cose-html-theme-replica
description: 用于生成或改写高贴合度的 HTML 汇报、提案、PPT deck、单页 dashboard、信息卡页面，基于内部提炼出的三类视觉语言：白底商务汇报、深色科技 IP、浅色玻璃拟态信息卡。适用于用户要求“参考既有内部案例的风格”“按现有风格高复刻”“保持相近配色、布局、结构、节奏”“区分 PPT 16:9 分页与单页模块自适应布局，并且后续方便扩展新的风格类型”的任务。
---

# Cose HTML Theme Replica

这个 skill 用来把内部参考案例提炼为可复用的“风格原型 + 布局骨架 + 输出约束”体系。

## 来源边界

- 内部参考案例只用于前期提炼风格规律。
- 最终对外产物不要提到 `case`、`cose`、源文件名或内部来源路径。
- 这个 skill 的长期依赖对象是当前 `references/` 中沉淀出的抽象规则，而不是原始案例文件。
- 如果原始参考文件后续被移除，这个 skill 仍应继续可用。

## 适用范围

当用户有下面这些需求时使用：

- 基于既有内部风格模板继续写新页面
- 想高还原现有配色、排版、卡片、章节结构
- 想生成 HTML 版汇报页、滚动式 PPT、信息卡页面
- 想在保持同一设计语言的前提下扩展新主题

## 先做什么

1. 先判断输出模式：`deck` 或 `single-page`。
2. 再判断用户要对齐哪一种风格原型。
3. 读取对应 mode reference 和 variant reference，不要一次性加载全部细节。
4. 输出时优先复用该组合的：
   - 页面宽高关系
   - 背景构成
   - 标题层级
   - 色彩 token
   - 卡片/分栏/章节骨架
   - 信息密度和留白节奏

模式规则见 [references/mode-deck.md](references/mode-deck.md) 和 [references/mode-single-page.md](references/mode-single-page.md)。原型索引见 [references/archetypes-index.md](references/archetypes-index.md)。

## 工作流

### 1. 选输出模式

- `deck`
  适合 PPT、slides、deck、分页汇报、一页一屏、正式汇报、阶段复盘、提案路演。
- `single-page`
  适合单页概览、dashboard、信息卡页面、资源盘点、渠道对比、长页面内容组织。

如果用户没有明确指定：

- 出现“PPT / slide / deck / 分页 / 一页一屏 / 汇报 / 复盘 / 提案”时，优先 `deck`。
- 出现“单页 / dashboard / 总览 / 信息卡 / 资源盘点 / 渠道对比 / 长页面”时，优先 `single-page`。
- 如果同时出现“汇报”和“单页”，以用户明确的“单页”优先。

模式硬约束：

- `deck` 模式必须遵守 [references/mode-deck.md](references/mode-deck.md)。
- `single-page` 模式必须遵守 [references/mode-single-page.md](references/mode-single-page.md)。

### 2. 选风格原型

- `light-report`
  适合正式汇报、规划方案、SOP、排期、老板形象方案这类内容。
- `dark-ip`
  适合个人 IP、品牌策略、创意方向、视觉体系、形象升级方案。
- `light-glass`
  适合双通道对比、招商信息、展会沟通、资源盘点、报价解读。

如果用户没有明确指定，就按内容判断：

- “方案汇报 / SOP / 排期 / 规划” 优先 `light-report`
- “IP / 品牌 / 视觉 / 科技感 / 高级感” 优先 `dark-ip`
- “对比 / 资源 / 渠道 / 展会 / 报价 / 卡片信息” 优先 `light-glass`

推荐组合：

- `light-report + deck`：正式 PPT 汇报、经营复盘、项目提案。
- `dark-ip + deck`：品牌策略、个人 IP、视觉策略 deck。
- `light-glass + single-page`：资源盘点、商务沟通、渠道对比、报价解读。
- `light-report + single-page`：单页商务概览、数据 dashboard、管理层简报。

### 3. 定义输出骨架

先决定页面是：

- 多页滚动式 16:9 deck
- 单页长内容 dashboard
- 头图 + 分段卡片式方案页

除非用户明确要求，否则遵循 mode 优先、variant 辅助：

- `deck` 默认使用分页容器，每页是独立 16:9 画布。
- `single-page` 默认使用自然高度页面容器，section 和卡片按内容展开。
- 风格原型只决定视觉语言，不单独决定是否 16:9。

### 4. 抽取内容并重写为该风格

把用户内容映射成该风格常见结构：

- 封面
- 目录或章节导览
- 核心判断 / 核心机会
- 分模块说明
- SOP / 时间轴 / 矩阵 / 对比表 / 卡片组
- 收尾总结页

不要只改配色。真正的高贴合还原，必须同时对齐：

- 信息编排顺序
- 标题气质
- 模块密度
- 区块边界形式
- 装饰元素强弱

## 输出要求

- 优先直接交付可运行 HTML。
- 默认保留移动端基本兼容，但主体仍以桌面汇报视图优先。
- 允许使用 Tailwind CDN，如果原型本身就是 Tailwind 路线。
- 如果选择 `dark-ip`，优先原生 CSS 变量，不要硬套 Tailwind 设计语言。
- 不要混杂三种原型的核心识别元素，除非用户明确要求混搭。
- 先满足 mode 版式约束，再满足 variant 风格约束。
- `deck` 模式不得依赖内部滚动、缓冲区、溢出裁切来解决内容装不下的问题。
- `single-page` 模式允许页面和卡片随内容自然增高，但必须保持 section、网格、卡片节奏清晰。
- 默认不要添加“演示说明壳”。
  这包括：
  - 顶部总说明栏
  - 测试页标题横幅
  - 为了展示风格而额外添加的导航按钮组
  - “这是第几种风格”的外层介绍区
- 除非用户明确要求做“风格总览页”或“导航页”，否则页面应直接进入内容本体。

详细约束见 [references/output-contract.md](references/output-contract.md)。

## 对外交付约束

- 对外文案中不要出现“参考 `case`”“参考 `cose`”“源自某某案例文件”等说法。
- 如果用户要求“像现有案例”，默认只复用抽象后的风格特征，不暴露内部模板来源。
- 新产出的 HTML、文档、提示词模板应优先使用风格名 `light-report`、`dark-ip`、`light-glass`，而不是历史文件名。

## 风格扩展规则

当后续需要新增第 4 种或更多类型时：

1. 在 `references/archetypes-index.md` 新增一个原型条目。
2. 新建一个 `references/variant-*.md` 文件，描述：
   - 适用场景
   - 色彩 token
   - 背景规则
   - 字体与标题规则
   - 卡片和模块规则
   - 推荐布局骨架
   - 禁止事项
3. 如果新原型有强约束结构，再补充到 `references/output-contract.md`。

扩展时尽量遵守“主流程稳定，变体外置”的原则，不要把所有变体细节堆进这个 `SKILL.md`。

## Reference 导航

- 原型总览： [references/archetypes-index.md](references/archetypes-index.md)
- PPT/deck 模式： [references/mode-deck.md](references/mode-deck.md)
- 单页模块模式： [references/mode-single-page.md](references/mode-single-page.md)
- 白底商务汇报： [references/variant-light-report.md](references/variant-light-report.md)
- 深色科技 IP： [references/variant-dark-ip.md](references/variant-dark-ip.md)
- 浅色玻璃拟态卡片： [references/variant-light-glass.md](references/variant-light-glass.md)
- 输出约束： [references/output-contract.md](references/output-contract.md)
- 调用模板： [references/prompt-recipes.md](references/prompt-recipes.md)
