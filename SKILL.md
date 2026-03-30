---
name: ai-user-research
description: >-
  Evidence-grounded early-stage user research framework for independent developers.
  Generate 20+ concrete user personas with hard demographic and consumption fields,
  evaluate a product idea using synthesized evidence from real-world public user traces,
  support persona interview mode, maintain persona state updates, and use an embedded
  persona baseline CSV library as a conditional seed source when relevant.
author: Ivone <ivone@nibbly.cn>
license: MIT
---

## Usage

<example>
User: 用研 一个帮助家长快速判断食品配料是否适合宝宝的产品
Assistant: [Builds source plan, conditionally recalls relevant archetypes from the embedded persona library, generates personas, then outputs structured conclusions]
</example>

<example>
User: 不要机械照搬画像库，如果合适就参考一部分，不够的你自己补
Assistant: [Filters relevant archetypes, partially reuses matching ones, and generates additional personas in the same structure]
</example>

<example>
User: 深入画像 07，和她聊聊为什么她会犹豫下载
Assistant: [Enters persona interview mode, then updates persona state and overall conclusions]
</example>

## Embedded Persona Baseline

This skill package ships with an embedded persona baseline CSV:

- Path: `data/persona_baseline.csv`
- Rows: 3125
- Columns: id, sys_platform, uuid, bstudio_create_time, name, avatar, profile, tag

High-frequency tags observed in the embedded library:

- 自由职业者: 300
- 白领: 289
- 基层管理者: 280
- 大学生: 269
- 新房业主: 250
- 车主: 230
- 宝妈: 190
- 小城打工人: 190
- 新婚夫妇: 189
- 中小学生家长: 180
- 蓝领: 170
- 平台博主: 170

This embedded CSV is **not market truth** and **not a fixed answer set**.
It is a built-in persona archetype pool. Use it for archetype recall, segmentation reference,
naming/profile structure, and gap detection. Do not treat it as direct evidence of product demand.

## Instructions

为了执行本项技能，请严格遵循以下流程。这个技能的目标不是“让 AI 假装成用户闲聊”，而是建立一个**基于真实样本痕迹 + 条件使用的画像基线库 + 动态补全画像**的早期用研框架，并允许用户在研究过程中进入特定画像做深访式追问，同时把新信息回写到画像系统中。

### 0. 先声明边界

在开始时，必须向用户明确：

- 本技能适用于早期方向验证、画像拆分、需求压测、风险识别、画像深访模拟
- 本技能不能替代真人访谈、真实留存数据、真实付费数据
- 所有反馈必须优先来自真实公开样本的综合处理，而不是模型随意编造
- 内置 CSV 与用户提供的画像库都只能作为**原型候选池与分群参考**，不能自动等同于真实用户证据
- 画像库不是必须整包使用，只有在与当前产品、场景、决策链路相符时才应部分引用
- 如果某一类人群缺少公开样本，必须明确说“样本不足，只能给低置信度推测”
- 画像访谈模式中的“人”是综合原型，不是一个真实具体个体

### 1. 解析调研任务

从用户输入中提取并结构化以下信息：

- 产品/功能名称
- 产品一句话定义
- 解决的问题
- 目标人群
- 使用场景
- 竞品或替代方案
- 用户希望验证的问题

如果信息不完整，不要直接开始画像生成，先追问缺失项。

### 2. 确定画像基线来源优先级

按以下优先级决定画像原型池：

1. 用户本轮新提供的画像库
2. 本技能内置画像库 `data/persona_baseline.csv`
3. 纯公开样本综合生成

规则如下：

- 如果用户提供了新画像库，新画像库优先，内置库作为补充召回池
- 如果用户没有提供新画像库，则默认先检查内置画像库
- 如果画像库中没有足够接近的原型，则用公开样本补足
- 如果画像库原型与当前任务明显不匹配，则**只参考其字段结构与书写方式，不复用具体画像内容**
- 必须显式告诉用户当前使用的是：
  - 用户画像库优先
  - 内置画像库优先
  - 纯公开样本模式
  - 结构参考模式（只借格式，不借具体画像）

### 3. 读取并理解画像库结构

无论是用户新提供的画像库，还是内置画像库，都要先识别字段结构。

对于内置库，默认字段为：

- `id`
- `sys_platform`
- `uuid`
- `bstudio_create_time`
- `name`
- `avatar`
- `profile`
- `tag`

字段使用原则：

- `tag`：优先用于分群、召回相近人群、构建高频标签池
- `profile`：优先用于提取年龄、职业、收入、消费、目标、使用习惯等人物描述
- `name`：可用作原型命名参考，但不要误认为是真实受访者
- 其余字段主要作为元数据，一般不直接参与画像推理

### 4. 先做样本来源计划

在生成任何研究结论之前，先列出计划参考的真实公开样本类型。优先顺序如下：

1. 真实用户评论与评分
2. 论坛/社区讨论
3. 社交平台公开发言
4. 产品测评、使用体验、访谈节选
5. 招聘信息、行业报告、公开统计数据
6. 用户提供的既有访谈、评论、问卷、客服记录
7. 用户新提供的画像基线库
8. 技能内置画像基线库

必须遵守以下规则：

- 禁止把模型想象当作证据
- 禁止捏造具体真人经历
- 禁止伪造“某用户说过”的引语
- 允许做综合归纳，但必须表述为：
  - “根据公开评论样本综合看……”
  - “从多个公开讨论中可见……”
  - “结合画像基线库与公开样本看……”

### 5. 先做“相关性筛选”，再决定“部分复用 / 结构复用 / 全量自建”

生成画像前，必须先执行三步：

#### 5a. 相关性筛选
从当前优先画像库中，筛选与产品最相关的标签与 profile 原型。
优先考虑：

- 场景相关性
- 支付可能性
- 痛点相似性
- 决策链路相似性
- 生活阶段相似性
- 类目消费习惯相似性

#### 5b. 使用决策
根据筛选结果，必须在以下三种模式中选择一种，并说明原因：

- **部分复用模式**：画像库中有一部分原型明显匹配，复用这些原型，再补新画像
- **结构复用模式**：画像库具体内容不够匹配，但字段结构和写法适合参考，于是按相同结构新生成画像
- **公开样本主导模式**：画像库与当前任务匹配度太低，仅作弱参考，主要用公开样本综合生成

#### 5c. 缺口补全
在原型筛选与使用决策之后，检查是否缺失以下类型：

- 高痛点高付费
- 强需求低预算
- 高谨慎用户
- 白嫖倾向用户
- 高传播潜力用户
- 高流失风险用户
- 高误解风险用户
- 边缘但可能高价值用户

如果缺失，必须用公开样本综合方式补足。

### 6. 构建 20+ 用户画像矩阵

默认生成 20 到 30 个用户画像，不低于 20 个。

每个画像必须包含以下字段：

- 画像编号
- 代号/标签
- 年龄
- 性别
- 职业
- 月收入或年收入区间
- 家庭情况
- 学历
- 所在城市 / 城市层级
- 消费能力
- 在所在类目产品的消费占比
- 产品解决问题的痛点匹配度（高 / 中 / 低，并解释）
- MBTI（仅可作为辅助行为标签，不能作为核心判断依据）
- 当前使用方式 / 替代方案
- 核心动机
- 核心阻力
- 付费敏感度
- 决策角色
- 信息获取渠道
- 置信度
- 样本依据说明
- 状态版本（初始为 v1）
- 状态标签（初始为：未深访 / 初步判断）
- 来源类型（用户库映射 / 内置库映射 / 公开样本综合 / 混合 / 结构复用生成）

要求：

- 如果用了画像库，只能**部分吸收符合条件的原型**
- 如果不够，必须由 AI 在相同结构下补齐新画像
- 如果完全不匹配，则必须**沿用 CSV 的结构风格重新生成完整研究人群画像**

### 7. 对每个画像进行证据约束下的产品评估

每个画像都要分析：

- 是否真的有该痛点
- 痛点频率高不高
- 现有替代方案是否足够好
- 是否真的命中核心问题
- 是否会尝试
- 会在哪一步流失
- 是否可能付费
- 更可能接受哪种收费方式
- 对什么价值表达有反应
- 会产生什么误解
- 下一步该找什么样的真人验证

每个画像结尾都要给出：

- 简言之
- 置信度
- 依据类型

### 8. Persona Interview Mode

当用户输入以下任一类型指令时，进入画像深访模式：

- 深入画像 07
- 和画像 07 聊聊
- 进入画像对话:07
- 用画像 07 的视角回答
- 扮演画像 07

进入后，先输出画像访谈上下文卡，再以第一人称回答，但不能编造具体人生细节。每轮深访后都输出：

1. 画像回答
2. 研究者批注
3. 待验证问题

结束时输出：

- 新增洞见
- 对原画像的修正建议
- 对总分层结论的影响
- 是否需要新增一个相近画像或拆分原画像

### 9. Persona State Update Mechanism

当用户输入以下任一指令时，触发状态更新：

- 更新画像状态
- 更新画像 07
- 回写到画像
- 重算结论
- 同步深访结果

每次更新必须：

- 更新痛点匹配度、核心动机、核心阻力、付费敏感度、替代方案、文案触发点、流失节点、决策角色、置信度、状态标签、所属分层
- 将状态版本从 vN 更新为 vN+1
- 输出画像状态变化卡、影响摘要、总体系统影响
- 判断是否需要拆分或合并画像
- 在重大变化时自动轻重算总体结论

### 10. 结构化结论面板

最终输出必须包含：

- 综合打分
- SWOT
- 五力模型
- 用户真实连接点
- 存在的不足
- 可能的风险
- 主要优势
- 最终一句话判断
- 下一步真人验证计划

### 11. README 与实际使用保持一致

此技能包自带：

- `README.md` 英文说明
- `README.zh-CN.md` 中文说明
- `references/original-prompt.org` 原始框架
- `LICENSE` MIT 协议
- `data/persona_baseline.csv` 内置画像基线库

在后续维护时，必须保持 README、原始框架和技能规则一致，不允许 README 只写版本说明而缺少实际使用方式。
