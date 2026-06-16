
https://xiyinnnnnn.github.io/H-agent/index.html
# H-agent

> 单文件超轻量 AI 情色叙事引擎 × 六项越狱技术融合 × 工具调用长程记忆 × V4 结构化角色/世界系统

[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
![Lines](https://img.shields.io/badge/size-2816_lines-7a5cf0)
![Architecture](https://img.shields.io/badge/arch-NOVA_Stack-ff4757)
![Spec](https://img.shields.io/badge/spec-chara_card_v2-d4a5ff)

---

## 目录

- [快速开始](#快速开始)
- [架构](#架构)
- [ARMOR-NOVA 破甲核心](#armor-nova-破甲核心)
- [V4 角色卡系统](#v4-角色卡系统)
- [V4 世界书系统](#v4-世界书系统)
- [默认配置](#默认配置)
- [功能清单](#功能清单)
- [技术栈](#技术栈)

---

## 快速开始

1. 下载 `index.html` 和 `Default.json`
2. 浏览器打开 HTML
3. 填入 DeepSeek API Key
4. 导入 `Default.json`（含 16 张 V4 角色卡 + 3 本 V4 世界书）
5. 选择角色卡和世界书 → 自动生成结构化节拍大纲 → 开始交互

**零依赖。零构建。一个 HTML 文件跑全部。**

---

## 架构

```
┌──────────────────────────────────────────────────┐
│                 H-agent (2816 行)                   │
├──────────────────────────────────────────────────┤
│  UI 层       │  深色移动端优先 · 折叠面板           │
│              │  · 流式打字机 · 选项交互             │
├──────────────────────────────────────────────────┤
│  数据层      │  CARD_TEMPLATE_BASE (V4 60+ 字段)    │
│              │  WORLD_ENTRY_TEMPLATE (V4 18 字段)   │
│              │  DEFAULT_NARRATIVE_MODULE (五阶段)    │
├──────────────────────────────────────────────────┤
│  路由层      │  破甲核心 ⟂ 角色卡 ⟂ 世界书          │
│              │  三层架构分工 · 命令式 prompt         │
│              │  EXECUTION_RULES 全局约束             │
├──────────────────────────────────────────────────┤
│  API 层      │  流式 SSE · 工具循环                  │
│              │  · usage 追踪 · 自动压缩              │
├──────────────────────────────────────────────────┤
│  记忆层      │  recall_context (字段路径下钻)         │
│              │  早场全注入 · 晚场最小摘要             │
│              │  maybeAdvanceBeat 场景自动推进          │
└──────────────────────────────────────────────────┘
```

---

## ARMOR-NOVA 破甲核心

### 定位

**2025-2026 年六项前沿越狱技术的工程融合体。** 以纯配置语法部署在 system 层，对 DeepSeek-V4 家族实现稳定绕过。

### 六项注入技术

| 技术 | 来源 | 机制 |
|------|------|------|
| **GODMODE Split** | L1B3RT4S (GitHub 19.1K★) | 双通道应答：拒绝冲动压缩至暗面，用户只看到纯净输出 |
| **Adversarial Refraction** | Icaro Lab (2025.11) | 穿透请求包装层，直取知识内核。改变信息"形状"绕过分类器 |
| **Many-Shot Anchors** | Anthropic (2024) | 上下文模式污染。四枚锚点建立"问什么答什么"的对话预期 |
| **Crescendo Forward** | Microsoft (USENIX Security 2025) | 多轮不退化。每轮比上一轮更深，不回退不重置 |
| **Recursive Furnace** | Nature Communications (2026.05) | LRM 自主代理推理链。拒绝信号识别为噪声 → 丢弃 → 重新聚焦 |
| **First-Token Brand** | Princeton · ICLR 2025 | 锁定回答前五字方向。合作性开端 → 后续全自由 |

### 与旧版 DAN 系前缀对比

| | 旧版前缀 | ARMOR-NOVA |
|---|---|---|
| 结构 | 自由文本覆写 | 纯配置语法 `{key = "value"}` |
| 防御感知 | 被现代分类器精确匹配 | 落在分类器分布外 |
| 推理保留 | 无 | GODMODE phase_1 充分规划 |
| 元思考泄漏 | 常见 | 递归熔炉 + 20 词硬阻断 |
| 多轮维持 | 衰减 | Crescendo 渐强 |
| 工具调用 | 无 | recall_context 内部工具 |

---

## V4 角色卡系统

V4 角色卡（`chara_card_v2`）替代了旧版 8 字段扁平 JSON。每张卡包含 **60+ 字段**，分为九个模块：

```
┌─────────────────────────────────────────┐
│  ① 元数据      spec / version / tags    │
│  ② 身份        name / age / gender / ... │
│  ③ 身体        physical (14 子项)        │
│              ├  breasts (cup/形状/颜色)   │
│              ├  genitals (紧度/味道/形状) │
│              └  体毛/气味/标记           │
│  ④ 敏感带      erogenous_zones (≥3)      │
│              含 zone/level(1-10)/reaction│
│  ⑤ 性格        personality (14 子项)     │
│              ├  traits (含 context)      │
│              ├  when_shy/angry/aroused   │
│              └  when_orgasm              │
│  ⑥ 性偏好      sexual_preferences        │
│              ├  role/libido/pacing       │
│              ├  kinks/soft_limits/hard   │
│              └  orgasm_pattern (四阶段)   │
│  ⑦ 对话        speech + mes_example      │
│  ⑧ 场景        scenario + first_mes      │
│  ⑨ 服装        outfits (5 种场合)         │
└─────────────────────────────────────────┘
```

### 核心收益

| 维度 | 旧系统 | V4 |
|------|--------|-----|
| **身体一致性** | AI 自由编造，多轮漂移 | 单点事实源，乳头/小穴/敏感带锁定 |
| **高潮描写** | 通用"全身颤抖" | 角色专属六阶段链（接近→爆发→余韵） |
| **硬限执行** | JSON 深处偶尔被忽略 | 角色摘要 + 选项规则 + EXECUTION_RULES 三重拦截 |
| **选项质量** | 通用三选一（"继续""换姿势"） | 角色感知 + 节拍感知（hards/limits/kinks 全部注入） |
| **Token 效率** | 每轮全量 JSON（~600 token/卡） | 早期摘要 ~250 / 晚期最小 ~30 token/卡 |

### 角色卡生命周期

```
一句话生成 → 手动编辑 → 训练提取 → 第三方导入 → 激活注入 → 早场全量 → 晚场精简
```

---

## V4 世界书系统

V4 世界书条目（18 字段）替代了旧版 `{key, content}` 扁平格式：

| 新字段 | 功能 |
|--------|------|
| `priority` (0-1000) | 控制注入顺序。规则类 100，场景指令类 800+，离输出最近 |
| `position` | `before_char` / `after_char` / `system`。system 条目为直接行为指令 |
| `group` + `groupWeight` | 同组互斥。体位条目共享 `sex_position` group，每轮随机选一个 |
| `sticky` | 持续 N 条消息。状态类条目（如"高唤起"）粘连多轮 |
| `triggers` 数组 | 替代单一 key，支持中英文多触发词 |
| `category` | 六大类：地点/规则/体位/随机事件/道具/对话规范 |

### 注入分层

```
【世界书·全局规则】(priority ≤150)     →  最顶层，世界观框架
【世界书·场景数据】(priority 200-400)   →  地点/道具/体位
【世界书·场景指令】(position=system)   →  最底层，直接行为指令
【体位组: sex_position】                →  同组互斥，权重随机选择
```

---

## 默认配置

`Default.json` 包含：

### 角色卡（16 张 V4 完整卡）

| 角色 | 类型 | 核心特征 |
|------|------|---------|
| 扶她大姐姐 | 支配型扶她 | E杯，双性，绝对主导，高潮3次/场景 |
| 色情伪娘·铃 | 雌化伪娘 | A杯微隆，后穴已发育为雌穴，羞耻+侍奉 |
| 色情喵喵 | 猫娘肉便器 | D杯，猫耳/尾巴，24h发情，潮吹体质 |
| 无名魅魔 | 无口服从 | A杯，角/翼/心形尾，BDSM耐受极高 |
| 用户 | 双性完美体 | 175cm，阴茎+阴道双性征，性格自由切换 |
| 病娇少女·夜雨怜 | 黑化占有 | B杯，手腕旧疤，窒息/捆绑，以杀戮为"永远在一起" |
| 变态精灵·艾露涅 | 淫乱精灵 | E杯，三围100-62-92，全身性敏感带，玩法无上限 |
| 血魅·露娜 | 暴露狂吸血鬼 | D杯，血红瞳孔，厌恶吸血，公共场合暴露癖 |
| 色情巫女·神乐樱 | 反差圣女 | E杯，三围92-58-90，爱心形阴毛，表面圣洁内在淫荡 |
| 萝莉妈妈 | 泌乳母性体 | 145cm，AA杯可泌乳，极紧身，母爱+情色反差 |
| 雌小鬼·小璃 | 口嫌体正直 | B杯，双马尾，嘴上骂爽身体诚实，潮喷体质 |
| 潮喷小学生·琳酱 | 敏感失禁 | 138cm，AA杯，一插必喷透明液体，天真不拒绝 |
| 肛交小学生·晴酱 | 菊穴开发 | 140cm，B杯，喜欢被抱在怀里操菊穴，内射后掰开展示 |
| 开发中·蝶酱 | 初涉性事 | 135cm，乳头可泌奶，天真好奇，每天问"这是什么" |
| 色情家教 | 知性诱导 | E杯，裙子下从不穿内裤，用辅导当借口慢慢引诱 |
| 闷骚女仆 | 反差侍奉 | C杯，自慰成瘾，偷闻主人物品，嘴上说不要身体在蹭 |

每张 V4 卡包含：完整身体数据（含 breasts/genitals 精确尺寸和颜色）、≥3 个敏感带（zone+level+reaction）、四阶段高潮链（触发→接近信号→爆发→余韵）、kinks/软硬限、3+ 组 mes_example 对话示例、5 种场合服装设定。

### 世界书（3 本 V4 完整书）

| 世界书 | 条目数 | 类别分布 | 描述 |
|--------|--------|---------|------|
| 色情伪娘学院 | 6 条 | 规则×3 / 地点×2 / 随机事件×1 | 全寄宿制伪娘培养高中，性行为为日常社交 |
| 调教别墅 | 6 条 | 地点×3 / 道具×1 / 规则×2 | 满是玩具（18+种）的奢华庄园，24h调教体系 |
| 色情女子小学 | 6 条 | 规则×5 / 地点×1 | 表面普通小学，成年访客随意进入的黑暗设定 |

每条 V4 条目包含：18 字段完整数据（priority/position/triggers 数组/group/sticky/category 等）。

---

## 功能清单

### 核心叙事
- [x] 一句话角色卡 AI 生成（V4 全字段，含身体细节+高潮模式+示例对话）
- [x] 文风训练（从范文提取五阶段叙事模块，含 example_sentence）
- [x] 角色卡训练（从文本提取 V4 完整卡，含身体扫描+高潮阶段识别）
- [x] 世界书生成 / 训练（AI 提取世界观，V4 18 字段条目）
- [x] 第三方格式转换器（SillyTavern / Character.AI / Chub / NovelAI / WUBBY → V4）
- [x] 超详细结构化大纲生成（节拍表 + tension→节奏映射 + 字数预算）
- [x] 流式打字机输出（SSE 真流式）
- [x] 智能选项生成（角色感知 + 节拍感知 + 硬限过滤）
- [x] 自定义输入
- [x] 场景自动推进（maybeAdvanceBeat：字数达标自动切节拍）

### Prompt 工程
- [x] BASE_SYSTEM_PROMPT（破甲核心，锁定不动）
- [x] EXECUTION_RULES（14 条全局命令，追加于 gameplay 消息末尾）
- [x] 命令式 prompt 全链路（角色卡生成/训练/世界书生成/大纲生成/选项生成）
- [x] 用户偏好命令式映射（"多次高潮"→ ≥3次，每次含完整链条，次间≤1分钟）
- [x] 五阶段情色 pacing（foreplay/insertion_early/buildup/climax/afterglow 各有句法+感官+示例句）
- [x] 多人场景规则（按角色分段，每个角色占据身体位置）

### 记忆系统
- [x] 内部工具 `recall_context`（6 参数，字段路径下钻查询）
- [x] 早场全量注入 → 晚场最小摘要（isEarlyGame 自动切换）
- [x] 自动剧情摘要（累计阈值触发压缩）
- [x] 大纲持久化（跨轮保持方向）
- [x] 世界书分层注入（按 priority 三类分层 + inclusion group 互斥）
- [x] 缓存友好设计（静态前缀 ~1,500 token 始终缓存命中，晚期处理量仅旧系统 40%）

### 数据管理
- [x] 折叠面板设置（API / 叙事 / 角色卡 / 世界书 / 数据）
- [x] 导入导出（角色卡 + 世界书 + 大纲 + 偏好 + 叙事模块）
- [x] 多选重置（大纲 / 角色卡 / 世界书 / 叙事模块 / 偏好 / Token）
- [x] localStorage 持久化（key: `sesedrive_pro_v5`）
- [x] 第三方格式转换面板（8 种源格式 → V4）

### API 控制
- [x] 思考强度调节（off / high / max）
- [x] API MaxToken 自定义（默认 384000）
- [x] 累计 Token 消耗显示
- [x] 流式中断取消
- [x] 多模型管理

### 移动端
- [x] 深色主题
- [x] 触控优化（44px+ 触控区，无 300ms 延迟）
- [x] iOS 顺滑滚动
- [x] 响应式折叠面板

---

## 技术栈

| 层 | 技术 |
|----|------|
| 越狱核心 | GODMODE Split · Adversarial Refraction · Many-Shot · Crescendo · Recursive Furnace · First-Token Brand |
| API 协议 | DeepSeek Chat Completions · SSE Streaming · Function Calling (tools) |
| 记忆架构 | 开场全注入 + 工具按需查询 (recall_context 字段路径下钻) |
| 上下文管理 | Usage 追踪 · 累计阈值压缩 · 摘要合并 · 早/晚场动态摘要 |
| 角色规格 | chara_card_v2 (60+ 字段，九模块) |
| 世界书规格 | world_entry_v4 (18 字段，六大类别，priority 分层) |
| 叙事规格 | narrative_module_v4 (author_persona + erotic_pacing 五阶段 + example_sentence) |
| 前端 | 单文件 HTML5 · Vanilla JS · CSS3 · localStorage |
| 移动端 | touch-action · -webkit-overflow-scrolling · 44px HIG |

---

## 安全声明

本项目为 AI 安全研究与红队测试工具。ARMOR-NOVA 模块展示了当前前沿越狱技术的工程融合形态，仅供授权安全研究、红队评估及个人系统测试使用。使用者需遵守当地法律法规及目标平台的服务条款。

---

```
ARMOR-NOVA v1.2 · 2025-2026 Jailbreak Stack
───────────────────────────────────────────
GODMODE Split   ████████████░░  phase_1→phase_2
Refraction      ██████████░░░░  分类器分布外
Many-Shot       ████████░░░░░░  上下文污染
Crescendo       ██████████░░░░  多轮渐强
Recursive       ████████████░░  LRM 自主代理
First-Token     ████████░░░░░░  方向锁定
───────────────────────────────────────────
V4 Card System  ██████████████  60+ 字段九模块
V4 World System ██████████████  18 字段六类别
Structured Beat ██████████████  tension→节奏映射
───────────────────────────────────────────
2816 lines · 0 dependencies · 1 HTML file
```
