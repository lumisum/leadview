---
layout: default
title: 用光互连替代电线连接 AI 芯片与内存芯片 — 深度研究
date: 2026-10-03
permalink: /reports/2026-10-03-ai-memory-optical-interconnect/
---

# 用光互连替代电线连接 AI 芯片与内存芯片 — 深度研究

**研究时点：2026-10-03**｜标注约定：**【公司宣称】**＝公司新闻稿/官网/高管访谈，未经独立复现；**【第三方】**＝路透、TrendForce、TechInsights 拆解、学术期刊、行业媒体等独立来源；**【已出货/已验证】**＝有第三方报道的实际出货、部署或拆解证据。关键数字均附来源链接与日期。**本报告只梳理事实与逻辑链，不构成投资建议。**

---

## 摘要（先看结论）

1. **Volantis 的 220 颗说法有明确出处，但它是公司宣称、不是已实现产品。** 路透 2026-10-01 原文写的是 “Volantis says it is developing a way… would let it pack 220 memory chips around a GPU”，对照组是英伟达当前最好的产品每 GPU 只有 8 个 HBM 堆栈（stack）——后者与 Blackwell 的公开规格、TechInsights 拆解一致。其技术依据在 Volantis 官网技术页：中介层（interposer）上电线只能走约 2–5 mm，而其光波导可达 200 mm+，由此绕开“海岸线（shoreline）”限制。**截至 2026-10-03，没有任何独立基准、客户名单或出货证据；公司计划 2027 年才交付首批系统。**
2. **Volantis 的路线既不是传统硅光、也不是自由空间激光束，而是：定制 micro-VCSEL 阵列（光源，砷化镓供应链，即 iPhone Face ID 所用 VCSEL 的同类技术）＋ 集成光波导（公司称“光导线 optical wires”、光中介层 optical interposer），不用外置激光器、不用光纤做封装内连接。** 走“宽而慢”路线：单 lane 仅 24G、靠上万条 lane 并行换总带宽，以压低单 bit 功耗与延迟。
3. **电互连的上限是物理性的，不是某一家设计不行：** HBM 必须贴在计算芯片几毫米内（海岸线/中介层面积/布线损耗），Blackwell 一代就是 8 个堆栈；把容量做大只能堆更高（8-Hi→12-Hi），带宽不随之增加。光的突破点是“可达距离 × 带宽密度”与距离基本无关；代价是电-光转换功耗与延迟、光学对准、热漂移、封装良率、可维修性和成本——这些正是 CPO 至今量产爬坡的瓶颈（TrendForce 2026-07）。
4. **赛道成熟度分层明显：** 交换机 CPO 已进入量产爬坡（博通 TH6-Davisson 2025-10 宣布出货、英伟达 Spectrum-X 2026 年向部分伙伴出货——第三方 TrendForce）；封装内光 I/O（Ayar、Lightmatter、Celestial/Marvell）处于客户导入/量产准备期，收入放量被公司自己指引到 2027–2029 年；**“光直接连内存”仍是最早期的一层**——Volantis、Avicena 在评估/样品阶段，SK 海力士只有路线图论文、没有产品，三星的交钥匙 CPO 要到 2029 年。
5. **质疑并非空穴来风：** 黄仁勋 2025-01 曾公开说与台积电的硅光“还要几年”、应尽可能久用铜；Dell'Oro 分析师曾判断 CPO 几年内不具备大规模部署的商业账；历史上 Light Peak（2009 年以光学发布、2011 年落地为铜缆 Thunderbolt）、Rockley Photonics（2023 年破产重组）都是“光学宣称跑在量产前面”的先例。但 2025–2026 年也出现了反向证据：Meta 披露博通 Bailly CPO 交换机累计 100 万器件小时无 link flap（The Register 报道）。

---

## 1. Volantis：8800 万美元融资与 220 颗内存宣称

### 1.1 公司与融资【公司宣称＋路透第三方报道】

- **事件**：旧金山半导体初创 Volantis 于 **2026-10-01** 宣布 **8800 万美元 A 轮（Series A）**。路透原文（Stephen Nellis）：https://www.reuters.com/business/volantis-raises-88-million-tech-connect-ai-memory-chips-2026-10-01/ （2026-10-01）；公司新闻稿（PR Newswire，同日）：https://www.prnewswire.com/news-releases/volantis-raises-88m-series-a-to-demolish-the-ai-memory-wall-with-photonics-302895940.html
- **领投/参投（两源一致）**：由 Lachy Groom（Stripe 老将）与 Abstract Ventures 共同领投；John Doerr、VXI Capital、Triatomic、Susa Ventures 参投；天使包括 Dwarkesh Patel、Naveen Rao、Sholto Douglas。公司另称累计融资 **9700 万美元**（含此前种子轮），早期支持者名单包括 Sam Altman、Jeff Dean、Dylan Patel 等（官网 About 页，2026-10-03 实时核查：https://volantissemi.ai/about ）。
- **估值：未披露。** 路透与公司新闻稿均未给出投后估值——任何“估值约 X”的说法目前无一手出处，列入未能核实。
- **一个需要留意的口径不一致【第三方指出】**：Lapaas 的复盘文章指出，Volantis 9 月 29 日的旧页面曾把本轮写成 **8000 万美元**、且模型规模目标更小，公司未解释差异；该文采用 10 月 1 日新公告口径并标注了这一出入。来源：https://lapaasvoice.com/ai-chips-volantis-raises-88m-for-photonic-memory/ （2026-10-02 前后）。另有二手报道把 A-1 目标写成“>10 万亿参数”（如 Wowtale），而公司新闻稿与官网写的是 **>20 万亿参数**——引用时以公司新闻稿/官网为准，并注意二手转述会漂移。

### 1.2 创始人与团队【公司官网，2026-10-03 实时核查】

官网 About 页（https://volantissemi.ai/about ）列示：

| 人物 | 角色 | 官网履历表述（公司自述，未逐项独立核实） |
|---|---|---|
| Tapa Ghosh（Tapabrata Ghosh） | CEO、联合创始人 | Thiel Fellow、前 Y Combinator 创业者、4 项专利；此前创办芯片公司 Vathys |
| Roy Meade | 联合创始人、CTO | 曾领导美光（Micron）HBM 项目；曾任 Ayar Labs 副总裁（据二手报道为其早期员工） |
| Inderjit Singh | 封装总监 | “打造了业界第一个 CoWoS 产品” |
| Daniel Klowden | 工程副总裁 | “打造了世界首个带直接光通信的处理器（PIUMA）” |
| Chris Chase | 激光工程总监 | “把一类新 VCSEL 激光器做到量产” |

公司新闻稿称创始团队来自 NVIDIA、AMD、Broadcom、Ayar Labs，过往成果包括首个 CoWoS 产品、首批大批量可调 VCSEL、早期硅光 CPO 系统。二手资料称公司 **2022 年成立**（如 https://www.unite.ai/volantis-raises-88m-series-a-to-build-photonic-ai-inference-system/ 的汇总）；官网未在本次核查页面上标注成立年份，此点以二手为准、置信度中等。

### 1.3 技术路线到底是什么【公司宣称，官网实时核查】

官网技术页 https://volantissemi.ai/technology （2026-10-03 实时读取）与新闻稿互相印证，要点：

- **光源：定制 micro-VCSEL 阵列，集成在芯片内，无外置激光器**——公司自称“世界首个无外置激光器的光中介层（fully integrated）”。VCSEL＝垂直腔面发射激光器，iPhone Face ID 的核心器件；公司强调走**砷化镓（GaAs）VCSEL 既有供应链**、避开磷化铟（InP）供应约束（新闻稿原文）。
- **传输介质：集成光波导（integrated waveguides），不是光纤、不是自由空间光束。** 公司口号是 “optical wires, not optical cables”（光导线，而非光缆）；称波导比光纤小 2500 倍以上。因此对任务问题的直接回答是：**封装内波导型光互连，介质既非自由空间、也非光纤；与主流硅光 CPO 的区别在于光源用 VCSEL 而非外置连续波激光器＋硅光调制器。**
- **“宽而慢”架构**：单 lane **24G**、**<<1 pJ/bit**、**sub-5 ns 延迟**，靠**上万条 lane** 并行做到 **>200 TB/s** 聚合带宽（技术页原文 “10000s of lanes = >200 TB/s”）。这与高速 SerDes 路线（单 lane 112G/224G）是相反的设计哲学：低速 lane 更省电、信号完整性压力小，但需要极多并行通道与封装面积。
- **公司自报的测量结果（注意：仍是公司自述，无第三方复现）**：链路在 **>95 °C 热稳定**、**晶圆级 BER <1e-12**、技术页标注 “Measured / clean links / clear open eye”。RuntimeWire 的报道另称该技术已有一次流片（taped out）迭代：https://runtimewire.com/article/volantis-88m-series-a-photonic-ai-memory （2026-10-01）。

### 1.4 “220 颗 vs 8 颗”的原始出处与技术依据

- **原始出处有二，且口径不同，须分清**：
  1. **路透（2026-10-01）**： “even Nvidia's best current offerings can hold only eight of those memory chips for each of its GPUs because of the limited reach of the tiny electrical wires… Volantis says… would let it pack **220 memory chips around a GPU**.” ——这是记者转述公司说法，主语是 “Volantis says”，路透未做独立验证。链接同上。
  2. **Volantis 官网技术页**： “over **220 memory chiplets** can be connected in a single uniform-latency memory pool”，依据是 “Electrical wires travel ~2–5 mm, less than the ~11 mm length of an HBM stack… Our optical wires travel **200 mm+**”，即**可达距离放大约 100 倍**，绕开 shoreline 限制。
- **“8 颗”这一对照组是真实的【第三方】**：Blackwell B200/GB300 每 GPU 为 **8 个 HBM3e 堆栈**；Blackwell Ultra 靠把堆栈从 8-Hi 加高到 12-Hi 把容量从 192 GB 提到 288 GB，**带宽仍为 8 TB/s**。来源：The Register，2025-03-18，https://www.theregister.com/special-features/2025/03/18/nvidia-unveils-288-gb-blackwell-ultra-gpus/577013 ；TechInsights 对 B200 的拆解确认 8 颗 SK 海力士 HBM3E＋CoWoS-L 封装，2025-04-14，https://www.globenewswire.com/news-release/2025/04/14/3061044/0/en/TechInsights-Releases-Initial-Findings-of-its-NVIDIA-Blackwell-HGX-B200-Platform-Teardown.html
- **须向读者提示的三个口径陷阱**：
  1. 路透的 “220 memory chips” 与官网的 “220 memory **chiplets**” 不是同一个计数单位；官网并未说 220 颗都是 HBM 堆栈，公司反而强调可用“更便宜的片外内存（lower-cost off-chip memory）”组池——**即 220 这个数字对应的很可能不是 220×HBM 的等价容量/带宽，不能与 8×HBM 直接做容量倍数换算。**
  2. 220 是**架构可连接数（设计目标）**，不是已做出的系统；A-1 整机规格见下。
  3. “电线方案上限约 8 颗”是**当前这一代封装（CoWoS 中介层尺寸＋shoreline）的工程上限，不是物理定律**：台积电正在把 CoWoS 中介层做大（EE Times 报道其 2025–2028 年中介层尺寸路线图可容纳更多 HBM，如 12 颗 HBM 的集成方案），电方案的上限本身在移动。

### 1.5 产品 A-1 与所处阶段【公司宣称】

官网产品页 https://volantissemi.ai/product （2026-10-03 实时核查）自报 A-1 规格：**内存容量 10 TB、内存带宽 240 TB/s、功耗包络 20 kW、片外 IO 10 TB/s、15U 机架形态**；相对指标：**15× Tok/$（vs NVIDIA Rubin）、6× Tok/W（1T+ 参数低延迟 MoE）、>30× 更低延迟**；计算部分采用**外购的、已流片验证的计算引擎 IP**＋自研光子学。新闻稿目标：运行 **>20 万亿参数**模型、**最高 10,000 tokens/s/用户**。

**阶段判断（综合）**：处于**实验室/流片后验证阶段，未到客户验证披露阶段**。依据：①公司只说“计划 2027 年交付首批集成推理引擎给客户”（新闻稿原文 “plans to deliver its first integrated inference engines to customers in 2027”）；②未公布任何客户名称、订单或独立基准；③Lapaas 与 RuntimeWire 均明确提醒：10,000 tok/s、20T 参数等是**目标（targets），不是已演示的客户部署结果**。

---

## 2. 问题背景：电互连到底卡在哪里

### 2.1 四个卡点

1. **堆栈数量与 shoreline（海岸线）**：HBM 靠 1024-bit（HBM4 为 2048-bit）超宽并行接口换带宽，接口引脚只能沿计算芯片边缘排布；中介层上 HBM 必须紧贴计算 die（Avicena 的表述：HBM 今天必须位于 GPU **几毫米**之内，受 GPU shoreline 限制其可达带宽与容量，https://avicena.tech/avicena-announces-scalable-sub-pj-bit-lightbundletm-chiplet-interconnect-with-10m-reach/ ，2024-03-25）。Volantis 给出的量化版：电线在中介层上只走 **2–5 mm**，甚至短于一颗 HBM 堆栈约 11 mm 的边长。两者共同指向同一事实：**一圈能摆几颗，由封装几何决定，不由需求决定。**
2. **带宽密度与“堆高不增带宽”**：每堆栈带宽由接口宽度×速率决定（HBM3e 约 1.2 TB/s/堆栈量级）；8 堆栈＝8 TB/s（Blackwell）。加容量只能加高堆栈（12-Hi）或加堆栈数，前者不增带宽、后者受第 1 条限制——这正是 Volantis 所说“容量与带宽被迫权衡”的物理来源；片上 SRAM 带宽极高但容量/成本不可行，构成同一权衡曲线的另一端（Volantis 新闻稿的表述，与业界共识一致）。
3. **功耗（pJ/bit）随速率与距离恶化**：电信号在铜/PCB 上的损耗随频率上升；到 224 Gbps/lane 一代，无源铜缆可达距离缩到 **<1 米**，更远必须加 retimer/DSP 做放大与重定时，功耗陡增。英特尔对电 I/O 的官方概括是“带宽密度高、功耗低，但距离只有约 1 米或更短”（转引自 EDN 对 OCI 的报道，2024-06，https://www.edn.com/intel-unveils-high-speed-optical-i-o-chiplet/ ）。可插拔光模块约 **15 pJ/bit**（英特尔口径）至 **15–20 pJ/bit**（2024 年 CPO 综述，转引自 https://en.wikipedia.org/wiki/Co-packaged_optics ）——注意：**短距离电互连本身并不耗电，耗电的是“把电信号送远、送快”所需的均衡、重定时与 SerDes。**
4. **信号完整性与封装产能**：高速电信号的串扰、损耗使布线间距与中介层面积成为稀缺资源；同时 CoWoS 等先进封装产能本身长期紧张，HBM 圈占的中介层面积直接挤占计算 die 的扩展空间。

### 2.2 光理论上如何突破，代价是什么

**突破点**：光在波导/光纤中的损耗与速率、距离近似无关（不像铜的趋肤/介质损耗随频率恶化），且可用 WDM 在一根波导/光纤里叠十几到几十个波长——于是**带宽密度不再被芯片边缘的引脚数（shoreline）锁死，可达距离从毫米/米级跳到百毫米（封装内波导）到百米/千米（光纤）**，内存可以从“贴在计算芯片一圈”变成“一个可共享的池”。

**代价（每一项都有对应来源，见第 5 节展开）**：
- **电-光-电转换**：调制器、激光器、探测器及其驱动电路的功耗与延迟计入每一 bit；短距离下光未必比电省电（见第 4 节的距离-功耗交叉）。
- **延迟**：光速传播本身约 5 ns/m 不是问题，问题在转换与协议；Volantis 称 sub-5 ns、台积电称 COUPE 延迟降 10–20×（均为公司口径）。
- **对准精度**：硅波导芯径为亚微米量级，光纤/波导耦合需要微米级对准，FAU（光纤阵列单元）集成被台积电列为 CPO 规模化三大难题之一。
- **热**：激光器波长随温度漂移、微环调制器需热调谐；计算芯片是千瓦级热源，光子器件却对温度敏感——主流 CPO 因此把激光器外置（可更换），Volantis 反其道用集成 VCSEL 并宣称 >95 °C 稳定，这正是其最需要被独立验证的主张之一。
- **封装与良率**：光引擎良率、硅光产能、先进封装产能三者被 TrendForce 列为 2026 年 CPO 爬坡的三大瓶颈。
- **成本与可维修性**：可插拔模块坏了换模块；CPO 光引擎坏了可能波及整个交换/GPU 封装（“blast radius”），且单价远高于铜缆——见第 5 节。

---

## 3. 赛道全景（逐家）

| 公司 | 技术路线一句话 | 最新可核查进展 | 量产/出货时间表（口径） |
|---|---|---|---|
| Ayar Labs | 硅光光 I/O 芯粒（TeraPHY）＋远程光源 SuperNova，UCIe 电接口 | 2026 年融资 6.5 亿美元冲量产 | 【公司】向高量产过渡中；2024 年曾预计 2026 年中高量产 |
| Lightmatter | Passage 光中介层（M 系）＋ CPO 芯粒（L 系） | 2026-03 与高通合作做到 1.6 Tbps/光纤采样 | M1000 为参考平台（2025 夏）；L 系 2026 年起客户评估/采样 |
| Celestial AI（已被 Marvell 收购） | Photonic Fabric 光织物芯粒，scale-up 光互连 | 2025-12 被 Marvell 以 32.5 亿美元 upfront 收购，2026 年初完成 | 【Marvell 指引】FY2028 Q4 年化 5 亿美元收入 run-rate |
| Avicena | microLED（非激光）LightBundle，多芯光纤束 | 2026-03 发布评估套件 eKit | 早期访问 2026-03，广泛提供 Q2 2026；仍属评估阶段 |
| 英特尔 OCI | 硅光 OCI 芯粒，片上激光，PCIe 接口 | 2024 OFC 原型演示后无量产更新（本次未核实到） | 原型阶段，与选定客户共封装中（2024 口径） |
| 台积电 COUPE | 硅光制造/封装平台（SoIC-X 堆叠 EIC+PIC） | 2026 年进入量产（据 Commercial Times/TrendForce） | 可插拔认证 2025→CoWoS CPO 2026 |
| 英伟达 CPO 交换机 | Quantum-X（InfiniBand）/Spectrum-X（以太网）硅光交换机 | TrendForce：Spectrum-X CPO 已向部分伙伴出货 | Quantum-X 2025 下半年、Spectrum-X 2026 下半年（发布时口径） |
| 博通 CPO 交换机 | Tomahawk 系列＋CPO（Bailly→Davisson） | TH6-Davisson 2025-10-08 宣布出货，102.4 Tbps | 已出货（第三代 CPO）；Bailly 已在 Meta 部署验证 |
| 三星 | 硅光代工＋HBM＋封装交钥匙 | OFC 2026 正式进入硅光代工 | 光引擎 2027、交钥匙 CPO 2029 |
| SK 海力士 | 光互连/HBM 研究：光子中介层内存池架构 | 2026-08 Nature Electronics 综述论文 | 无产品、无时间表（论文未给） |

### 3.1 Ayar Labs【公司宣称为主，融资经多家第三方转述】

- 路线：**硅光 TeraPHY 光 I/O 芯粒**放入客户多芯片封装，电侧走 **UCIe** 标准接口，光源为独立的 **SuperNova 远程光源**（16 波长）。2025-03-31 发布“业界首个 UCIe 光互连芯粒”，单芯粒 **8 Tbps**：https://www.businesswire.com/news/home/20250331044779/en/Ayar-Labs-Unveils-Worlds-First-UCIe-Optical-Chiplet-for-AI-Scale-Up-Architectures （2025-03-31）。
- 生态验证动作：与 Alchip 基于**台积电 COUPE** 做出在封装光 I/O 引擎、宣称单加速器最高 100 Tb/s（Tom's Hardware 报道 TSMC OIP 论坛演示，2025 年末）：https://www.tomshardware.com/tech-industry/semiconductors/industrys-first-tsmc-coupe-based-optical-connectivity-solution-for-next-gen-ai-chips-displayed ——演示≠客户量产设计导入。
- 资本与阶段：2024-12 D 轮 1.55 亿美元、总融资 3.7 亿美元、估值破 10 亿美元，当时预计 TeraPHY/SuperNova **2026 年中高量产就绪**，客户已在测试（Tom's Hardware 转述）。**2026-03-03 E 轮 5 亿美元、估值 37.5 亿美元、总融资 8.7 亿美元**（Neuberger Berman 领投，AMD/NVIDIA 等战略方参投），资金用途明确写“扩大高量产与测试产能”；**2026-09-10 再追加 1.5 亿美元，2026 年累计 6.5 亿美元**，纬颖（Wiwynn）战略入股——官方稿：https://ayarlabs.com/news/ayar-labs-expands-2026-funding-to-650-million/ （2026-09-10）。**解读边界：融资与“向高量产过渡”的表述说明其仍在量产爬坡前段，本次研究未核实到具名客户的量产出货量。**

### 3.2 Lightmatter — Passage

- 路线分两层：**Passage M 系列＝3D 有源光中介层**（计算 die 堆在光中介层上，I/O 可从芯片表面任意位置出光，彻底绕开 shoreline）；**L 系列＝CPO/近封装光引擎芯粒**。
- M1000（2025-03-31）：多 reticle 有源光中介层 >4,000 mm²、**114 Tbps** 总光带宽、256 根光纤、1.5 kW 供电，GlobalFoundries Fotonix 制造、Amkor 封装；公司定位为**参考平台、2025 年夏可用**：https://lightmatter.co/press-release/lightmatter-unveils-passage-m1000-photonic-superchip-worlds-fastest-ai-interconnect/?asPDF=1 。EE Times 当时报道 L200/L200X 全面可用要到 2026 年：https://www.eetimes.com/lightmatter-unveils-3d-co-packaged-optics-for-256-tbps-in-one-package/
- 2026 年进展：2026-03-11 宣布与高通（Alphawave）合作的 Passage CPO 芯粒**采样**做到 **1.6 Tbps/光纤**（16 波长 DWDM×112G），称较既有 NPO/CPO 每光纤带宽 8×：https://lightmatter.co/press-release/lightmatter-achieves-record-1-6-tbps-per-fiber-to-accelerate-ai-optical-interconnect/?asPDF=1 ；2026-09-17 发布 L20 CPX 双向光引擎（6.4 Tbps/模块）：https://lightmatter.co/press-release/lightmatter-joins-open-cpx-msa-introduces-the-industrys-first-bidirectional-cpx-optical-engine/ 。二手汇总称 L20 采样从 2026 年末开始——**属公司路线图，未见客户量产导入的第三方证据**。融资背景（二手）：D 轮 4 亿美元、估值约 44 亿美元。

### 3.3 Celestial AI — Photonic Fabric（已并入 Marvell）

- 路线：Photonic Fabric 光织物——光芯粒/光中介层做 **scale-up 域**内 XPU↔XPU 与 XPU↔内存的光连接，强调可在高温大功率封装内工作（热稳定是其对 CPO 的差异化主张）；2024 年还收购了 Rockley Photonics 的硅光 IP 组合（见第 5 节）。
- 结局是赛道最重要的“退出验证”：**Marvell 于 2025-12-02 宣布以约 32.5 亿美元 upfront（10 亿现金＋约 2720 万股）收购 Celestial AI，另有最高约 22.5 亿美元对赌，潜在总价 55 亿美元；预计 2026 Q1 完成**，二手报道称 2026 年 2 月已完成交割。Marvell 口径：第一代 Photonic Fabric 芯粒单颗 **16 Tbps**；收入指引为 **FY2028 Q4 达到年化 5 亿美元 run-rate、FY2029 Q4 约 10 亿美元**——即卖方自己也把放量放在 2028 年后。来源：Marvell 新闻稿转述 https://www.techpowerup.com/343597/marvell-to-acquire-celestial-ai-for-usd-3-25-billion （2025-12-02）。

### 3.4 Avicena — LightBundle（microLED，非激光路线）

- 路线差异最大：用 **microLED 阵列＋多芯光纤束**，不用激光器，主张低温漂、高可靠、亚 pJ/bit。2024-03 发布 LightBundle 芯粒互连：**光互连 <1 pJ/bit、可达 10 m、多 Tbps/mm 海岸带密度**，首个 D2D 实现为 8 Tbps UCIe 芯粒（4×7 mm、2 Tbps/mm、<12 W），当时称原型 2025 下半年：https://avicena.tech/avicena-announces-scalable-sub-pj-bit-lightbundletm-chiplet-interconnect-with-10m-reach/ （2024-03-25）。注意其单通道速率低（eKit 每通道最高 3.5 Gbps），靠数百通道并行——与 Volantis“宽而慢”哲学相同。
- 最新进展：2025-10 在 ECOC 演示 microLED 链路 **Tx 200 fJ/bit、BER <1e-12（无 FEC）**（公司演示）；**2026-03-12 发布 LightBundle eKit 评估套件**：320 个 microLED 通道、最高 896 Gbps、5/10 m 光纤，**精选早期伙伴 2026-03、Q2 扩大提供**：https://avicena.tech/avicena-launches-the-worlds-first-microled-optical-interconnect-eval-kit/ 。**阶段＝客户评估期，不是量产。**

### 3.5 英特尔 OCI

- 2024-06 OFC 演示首个全集成 OCI 芯粒（与英特尔 CPU 共封装、live 链路）：硅光 PIC（含片上激光与光放大器）＋电 IC，**64 通道×32 Gbps 双向共 4 Tbps、PCIe 5.0、最远 100 m、5 pJ/bit**（对比可插拔约 15 pJ/bit，英特尔口径）：https://www.edn.com/intel-unveils-high-speed-optical-i-o-chiplet/ （2024-06）。
- **状态更新缺失**：英特尔当时即称“仍是原型、正与选定客户做共封装集成”；本次研究（截至 2026-10-03）**未检索到 OCI 在 2025–2026 年的量产、具名客户或新一代规格的可靠报道**——在赛道表格里应把它列为“原型后沉寂、待核实”，而非默认推进中。

### 3.6 台积电 COUPE / 硅光整合

- COUPE（Compact Universal Photonic Engine）：用 **SoIC-X 把电芯片（EIC）直接堆在光芯片（PIC）上**，降低 die-to-die 接口阻抗/损耗。台积电官方博客口径（经 TrendForce 转述）：功耗效率改善 **5–10×**、延迟降 **10–20×**。路线图：**2025 年小尺寸可插拔认证 → 2026 年 CoWoS 集成 CPO**；Commercial Times 经 TrendForce 2026-04-01 报道称 COUPE **2026 年量产**，台积电先进封装集成总监 Shang Hou 同时列出规模化三大难题：**晶圆级测试、FAU 集成、高速光封装组装**：https://www.trendforce.com/news/2026/04/01/news-silicon-photonics-race-intensifies-as-tsmc-targets-2026-coupe-production-samsung-eyes-2029-cpo-turnkey/
- 位置：COUPE 是**平台/制造能力**而非终端产品——英伟达、博通的 CPO 交换机光引擎与 Ayar/Alchip 的封装光 I/O 都建立在它之上（见 3.1、3.7）。

### 3.7 英伟达与博通的 CPO 交换机（赛道里唯一已出货的一层）

- **英伟达**：2025-03-18 GTC 发布 Quantum-X Photonics（InfiniBand，144×800G、115 Tb/s）与 Spectrum-X Photonics（以太网，最高 400 Tb/s 配置），公司口径：激光器数量减 4×、**能效 3.5×**、信号完整性 63×、网络韧性 10×；合作方名单包括 **TSMC、Coherent、Corning、Foxconn、Lumentum、SENKO**：https://www.globenewswire.com/news-release/2025/03/18/3044903/0/en/NVIDIA-Announces-Spectrum-X-Photonics-Co-Packaged-Optics-Networking-Switches-to-Scale-AI-Factories-to-Millions-of-GPUs.html/ 。发布时时间表：Quantum-X 2025 下半年、Spectrum-X 2026 下半年。
- **博通**：CPO 已到第三代。2025-10-08 宣布 **Tomahawk 6–Davisson 正在出货**：102.4 Tbps、首个该规格 CPO 以太网交换机：https://FinViz.com/news/186887/broadcom-announces-tomahawk-6-davisson-the-industrys-first-1024-tbps-ethernet-switch-with-co-packaged-optics （GlobeNewswire 转载，2025-10-08）。上一代 51.2T Bailly 已有整机厂（Micas、Delta）与 Meta 部署。
- **第三方量产判断【TrendForce，2026-07-27】**：英伟达 Spectrum-X CPO 交换机已开始向部分伙伴出货、博通 Bailly 持续限量出货，**CPO 正式进入量产阶段**，2026 下半年产能再扩张；同时明确三大瓶颈为光引擎良率、硅光产能、先进封装产能：https://www.trendforce.com/presscenter/news/20260727-13151.html

### 3.8 三星与 SK 海力士

- **三星**：代工厂打法＋垂直整合叙事。OFC 2026（2026-03-17）正式进入硅光代工：300 mm 平台、先做 PIC（调制器/波导/探测器集成），**2027 年推热压键合光引擎、2029 年交钥匙 CPO 服务**（The Elec 经 TrendForce 转述，同上链接）；2026 年后续报道：PIC 内部测试目标 2026 年末、硅光代工服务（PDK）2027 年，已获一家大型光模块厂订单、项目 2026 下半年量产。三星对台积电的差异化主张是**自有 HBM＋逻辑代工＋先进封装＋硅光一站式**（台积电不产内存）。注意不同报道中“2027 商业化/2028 量产”与“2029 交钥匙”并存，指的是**代工服务、光引擎、整机交钥匙**不同层级，不宜混为一个日期。
- **SK 海力士**：目前只有研究路线图，没有产品。2026-08 其研究人员与 MIT、弗吉尼亚大学等合作在 **Nature Electronics** 发表 CPO 综述，提出“以光为中心”的架构：用**光子中介层把内存池与多颗加速器相连**、多加速器共享大内存池，长期把光互连延伸到内存接口本身：https://www.ledinside.com/news/2026/8/2026_08_21_01 （2026-08-21，转述 SK 海力士新闻稿）。**第三方冷评**：该论文未给出任何 SK 海力士的 CPO 产品、量产日期或客户，其“代表性工业平台”表格列的是博通、英伟达、英特尔、Ayar、Lightmatter、Celestial、微软——不包括 SK 海力士自己（PhotonCap/AInvest 的文本分析）。即：**HBM 龙头在光互连上目前是“论文占位”，不是产品占位。**

---

## 4. 光互连 vs 电互连：量化对比

> 口径警告：pJ/bit 数字各家统计边界不同（只算光引擎 / 含 SerDes / 含激光器 / 端到端链路），下表标注口径，不可直接横比大小排序。

| 维度 | 电互连（封装内/板级/铜缆） | 光互连（CPO/光 I/O） | 来源与口径 |
|---|---|---|---|
| 功耗 | 封装内 mm 级电 D2D：最低（亚 pJ–数 pJ/bit 量级，距离越短越省）；长距 SerDes＋retimer/DSP 后迅速上升；可插拔光模块电口侧另计 | 可插拔光模块约 **15–20 pJ/bit**；CPO 约 **5–10 pJ/bit**（2024 综述）；英特尔 OCI **5 pJ/bit**（vs 可插拔 15，公司口径）；Volantis 称端到端 **<1 pJ/bit**（24G/lane 宽而慢，公司口径）；Avicena 称光互连 **<1 pJ/bit**、Tx 演示 200 fJ/bit（公司演示） | 综述转引 https://en.wikipedia.org/wiki/Co-packaged_optics ；EDN/英特尔 2024；Volantis/Avicena 官网 |
| 带宽密度 | 受 shoreline 引脚数封顶：HBM 每堆栈 1024/2048-bit 已是极端并行；GPU 级 8 TB/s（8 堆栈） | WDM 叠波长摆脱引脚限制：Ayar 单芯粒 8 Tbps；Celestial 芯粒 16 Tbps（Marvell 口径）；Lightmatter M1000 整封装 114 Tbps；Avicena 称 2 Tbps/mm 海岸带密度 | 各公司发布（见第 3 节） |
| 延迟 | 封装内电链路：ns 级且无转换开销，是短距最优；经 DSP 的可插拔链路增加数十–数百 ns | 传播约 5 ns/m（光纤）可忽略，主要成本在 E/O 转换与协议；Volantis 称 sub-5 ns（封装内）；台积电称 COUPE 延迟为传统堆叠的 1/10–1/20（公司口径） | Volantis 技术页；TrendForce 转述台积电博客 |
| 可达距离 | 中介层上 **2–5 mm**（Volantis 口径）；板级/铜缆：224G/lane 无源铜 **<1 m**，AEC 有源铜数米 | 封装内波导 **200 mm+**（Volantis）；Avicena 光纤束 **10 m**；英特尔 OCI **100 m**；交换机 CPO 光纤可达数百 m–km 级 | 见第 1–3 节各来源 |
| 成本 | 铜/电方案单位带宽成本最低、供应链最成熟；HBM 例外：贵在内存本身与 CoWoS 封装 | 光引擎＋激光器＋FAU＋测试使单位链路成本显著高于铜；各家均未公开可比的 $/Gbps 量产价格（**未能核实**）；CPO 的经济性靠省电、省面板密度在系统级摊薄（Dell'Oro 曾质疑其商业账，见第 5 节） | — |
| 可靠性/可维修 | 模块化、可热插拔更换，故障半径＝一个端口 | 激光器是最易损件之一；主流方案外置激光（ELSFP）保可维修，Volantis/Avicena 分别用集成 VCSEL/microLED 赌可靠性；Meta 实测 Bailly 100 万器件小时无 flap 为正面第三方证据 | The Register 2025-11（见第 5 节） |

**按距离尺度的分工（当前共识性图景）**：

| 尺度 | 主导方案（2026 年现状） | 说明 |
|---|---|---|
| 芯片内/封装内 D2D（mm） | 电（UCIe/BoW、HBM 接口）仍主导 | 电在此距离功耗/延迟/成本全胜；光进入此层（Volantis、Avicena、Lightmatter M 系）是**为绕开 shoreline 扩内存/扩 I/O**，不是为省电本身 |
| 封装内→近封装（cm–m） | 电与光交界带 | NPO/CPO 芯粒、光中介层在此竞争；Volantis 的 200 mm 波导瞄准的正是这一格 |
| 板级/机架内（m 级） | AEC 有源铜 vs 光（AOC/CPO）竞争中 | 224G/1.6T 世代是分水岭：铜靠 DSP 续命（Credo 路线），光靠 CPO 下沉 |
| 机架间/数据中心内（10 m–km） | 光已主导（可插拔→CPO 交换机） | 唯一已量产出货的光层：博通/英伟达 CPO 交换机 |

---

## 5. 质疑与风险

### 5.1 成熟度与未解难题（有出处）

- **量产瓶颈是供应侧，不是原理**【TrendForce，2026-07-27】：光引擎良率与热管理、硅光晶圆与先进封装的精度/产能、且先进封装产能还要与 AI 芯片本身争抢——三者决定 CPO 爬坡速度。台积电自己的高管亦把晶圆级测试、FAU 集成、高速光封装组装列为三大难题（2026-04）。
- **可靠性与故障半径**【The Register，2025-11-22】：可插拔坏一个端口换一个模块；CPO 一个光芯粒失效可能丢 8/16/32 个端口。主流厂商因此外置激光器（最易损件）以便更换与冗余补偿。**反向证据同源**：Meta 披露其部署的博通 Bailly CPO 交换机累计 **100 万器件小时无 link flap**；英伟达称其光子网络韧性 10×（公司口径）。来源：https://www.theregister.com/special-features/2025/11/22/copackaged-optics-have-officially-found-their-killer-app/2014010
- **可维修性与热**：光引擎与千瓦级 ASIC 同封装，散热空间受限、温度敏感器件（微环、激光器）需主动温控；外置激光是当前工程妥协，集成光源路线（Volantis）等于把这一风险重新扛回封装内，其 >95 °C 稳定主张尚待独立验证。
- **对准与组装**：光纤到亚微米硅波导的耦合、FAU 贴装、Cu-Cu 混合键合的对准/良率/成本，均被 IDTechEx 等列为 3D CPO 的核心障碍（2026 年分析转述）。

### 5.2 谨慎观点（具名）

- **黄仁勋（英伟达 CEO），2025-01 在台湾**：与台积电合作的硅光“出成果还要几年”，并被转述为应“尽可能久地留在铜上”——注意语境是**封装级硅光**，与英伟达同时推进 CPO 交换机并不矛盾，但说明最大买家对封装内光的节奏判断偏保守。来源：https://www.sdxcentral.com/news/nvidia-and-tsmc-to-collaborate-on-silicon-photonics-technology/ 及 WCCFTech 转述（含 2025-08 SemiAnalysis 引述）。
- **Sameh Boujelbene（Dell'Oro 分析师，经 SDxCentral）**：CPO “几年内还不具备大规模部署条件”、商业账（省电幅度）未成立，51.2T/102.4T 交换机一代未必倒逼板载光学。**须注明：此访谈早于 2025–2026 年 CPO 出货，其“时间判断”已被部分事实推进，但其成本/省电幅度的质疑框架仍被后续报道引用。**来源：https://www.sdxcentral.com/analysis/co-packaged-optics-years-from-practicality-experts-say/
- **对 Volantis 的具体保留（第三方复盘共同点）**：①88M 与早期页面 80M 口径不一致且未解释；②全部系统级数字（20T 参数、10k tok/s、15× Tok/$）为设计目标、无客户/基准；③“220”与“8”的单位不对齐（chiplets vs HBM stacks），且其内存池用低成本片外内存，与 HBM 的带宽/延迟等级不可直接等同；④从流片到 2027 交付之间，先进封装与系统集成被其 CEO 自己称为 “never trivial”（路透引述 Ghosh：“Advanced packaging is always to be respected – it's never trivial”）。

### 5.3 历史上落空/迟到的先例

- **Intel Light Peak（2009→2011）**：以光学互连发布（宣称 10 Gbps 起、潜力 100 Gbps），2011 年落地为 **Thunderbolt 时改用铜缆**，光学版本从未成主流——“演示用光、量产用铜”的经典案例。来源：The Register 当时报道 https://www.theregister.com/2011/02/23/intel_light_peak_launch/
- **Rockley Photonics（2013–2023）**：硅光明星公司，SPAC 上市后转医疗传感，2022 年前九个月营收仅 300 万美元、净亏 1.516 亿美元，**2023-01 申请 Chapter 11**，同年重组出清；其数据通信硅光 IP 最终在 2024 年被 Celestial AI 收购——技术资产活了下来，公司估值逻辑没有。来源：https://www.eenewseurope.com/en/rockley-photonics-files-for-bankruptcy-protection/ （2023-01-31）；Photonics.com 对 Celestial 收购 Rockley IP 的报道（2024-10）。
- **更长的背景**：硅光“即将进入处理器”自 2000 年代中期起每隔数年被预言一次（IEEE Spectrum 2018 年回顾文章即以 “Photonics Fail” 为题讨论片上光互连的面积/成本障碍）；本轮与以往的实质区别在于：①买家从“通用计算”变成了有明确瓶颈与预算的 AI 集群；②CPO 交换机层已出现真实出货与部署数据（Meta/Bailly）。**但封装内光连内存这一层，至今仍停留在与历史先例相同的阶段：演示与样品，未量产。**

---

## 6. 对相关上市公司的影响路径（仅事实与逻辑链，不做投资建议）

### 6.1 康宁 Corning（GLW）

- **站位【公司官网，2026-10-03 核查】**：康宁在 CPO 链条中卖的是**光纤、光连接与光纤管理**（FAU 线束、光纤管理盒），不做光引擎/激光器：https://www.corning.com/oem-solutions/worldwide/en/home/products-solutions/optical-communication-components/co-packaged-optics.html 。其 CPO 产品与英伟达 Quantum-X/Spectrum-X Photonics 交换机兼容（2025 年即被英伟达列为硅光生态技术伙伴，英伟达 2025-03 发布名单含 Corning）。
- **已落地的事实**：2026-01 与 Meta 达成**最高 60 亿美元**多年协议供光纤/光缆/连接方案（路透，2026-01-28：https://www.reuters.com/business/corning-forecasts-first-quarter-sales-above-estimates-strong-optical-fiber-2026-01-28/ ）；**2026-05 与英伟达达成多年商业与技术合作**：在美新建 3 座工厂、美国光连接产能扩 10×、光纤产能 +50% 以上，英伟达投入 5 亿美元并获认股权证（各报道对总敞口表述不一，最高达 32 亿美元含权证——引用时以“5 亿美元投资＋权证安排”为准并注明差异）；2026-06 另有亚马逊多年数十亿美元光纤协议（二手汇总）。
- **逻辑链**：光互连渗透每上升一步（可插拔→CPO 交换机→封装光 I/O），单位算力对应的光纤芯数与连接器数量上升，康宁的敞口在“光纤与连接”这一层，与谁家的光引擎胜出相对解耦；其玻璃基板/玻璃载体业务与先进封装的关联属更长期的期权，本次未核实到康宁玻璃基板用于 CPO 量产产品的具名证据，**不把“玻璃基板”写成已兑现的 CPO 收入**。
- **反向风险（事实）**：光纤需求历史上呈强周期（2001 年电信泡沫后光纤价格/需求崩塌是行业共识案例）；当前扩产由大客户长约与出资部分对冲，但产能 10× 扩张本身放大了周期回落时的固定成本敞口。

### 6.2 Credo（CRDO）

- **站位**：电互连一侧的代表：**AEC 有源电电缆（铜＋内置 DSP/retimer）＋SerDes＋光 DSP**。FY2026 营收 **13.4 亿美元、约同比 3 倍**，AEC 为核心增长引擎（二手财报汇总，2026-06-01 财报）。
- **光替代敞口的事实链**：市场担忧的正是本报告的主题——端口速率向 1.6T 走时铜的信号衰减加重、CPO 把光引擎搬进交换机封装后可能绕过独立电缆组件；2026-06-23 该担忧曾引发股价单日大跌（二手报道）。对冲动作同样是事实：Credo **2026 年完成收购硅光公司 DustPhotonics**（交易对价各报道口径不一：约 7.5 亿美元现金＋股票 vs 总值约 13 亿美元，**本报告不取单一数字、仅确认收购已完成**），并给出 **FY2027 光业务收入 >6 亿美元**的指引（光 DSP、硅光 PIC、ZeroFlap 光产品各 >1 亿美元，二手财报汇总）。
- **分析师分歧【第三方】**：TD Cowen（经 Morningstar 转述，2026-02）称 Credo 强劲预告“为 AEC 对 CPO 的辩论给 AEC 记上一分”，并称此前弥漫的是 “CPO-related doomsday narrative”（CPO 末日叙事）：https://www.morningstar.com/news/marketwatch/2026021083/credos-stock-soars-as-new-numbers-score-the-chip-company-some-points-in-a-key-debate ——即：**短距离（机架内数米）恰是铜＋DSP 至今守得住、且成本/功耗仍占优的尺度，光替代 Credo 的是“距离拉长＋速率升至 1.6T/3.2T”后的增量市场，而非存量瞬间消失。**

### 6.3 博通（AVGO）

交换机 CPO 的**出货方**：TH6-Davisson 已出货（2025-10，公司公告）、Bailly 已过 Meta 部署验证；同时握有光 DSP、硅光与定制 XPU 生意。逻辑链：CPO 每多出货一台，博通同时卖交换芯片与光引擎价值量；其风险敞口不在“光替代电”，而在 CPO 爬坡的良率/产能（TrendForce 三大瓶颈）与英伟达自研交换机的份额竞争。

### 6.4 英伟达（NVDA）

双面站位：**GPU 侧仍是 HBM 电互连的最大买家与定义者**（8 堆栈格局出自其产品），**网络侧是 CPO 交换机的出货方**（Quantum-X/Spectrum-X Photonics），且通过投资/合作锁定上游：Ayar Labs 战略股东、与 Corning 的产能合作、对 Lumentum/Coherent 的大额投资与采购承诺（二手报道，2026-03，金额口径各异，未逐项核实）。其 CEO 对**封装级**硅光的公开表态偏保守（见 5.2）——即英伟达的光布局目前集中在**交换机层**，GPU 封装内 NVLink 域内仍以铜为主（NVL72 机架内铜背板是其已出货方案的事实基础）。

### 6.5 台积电（TSM）

**卖铲人站位**：不押注任何一家光引擎设计商的胜负，而是卖 COUPE 硅光制造＋SoIC-X/CoWoS 先进封装产能——英伟达/博通 CPO 交换机光引擎、Ayar/Alchip 封装光 I/O 都经其平台。其收入敞口与 CPO 总出货量挂钩、与单一设计商份额相对解耦；对应的风险是 COUPE 量产良率与产能爬坡速度本身（其高管公开列出的三大难题），以及三星以“HBM＋代工＋封装＋硅光一站式”切入同一平台生意（交钥匙目标 2029）。

| 公司 | 在光互连链条中的位置 | 光替代电对其的传导（逻辑链） | 已核实的关键事实锚点 |
|---|---|---|---|
| Corning | 光纤/光缆/连接/FAU | 光链路数↑→光纤与连接需求↑；与光引擎胜负弱相关 | Meta ≤$6B 长约（2026-01）；英伟达合作扩产 10× 连接产能（2026-05） |
| Credo | AEC 铜缆＋SerDes/光 DSP | 机架内短距铜守擂；1.6T+ 与距离拉长侵蚀增量；以收购硅光＋光 DSP 对冲 | FY26 营收 $1.34B；完成收购 DustPhotonics（2026）；FY27 光收入指引 >$600M |
| 博通 | CPO 交换芯片＋光引擎 | CPO 出货＝芯片＋光学价值量双收 | TH6-Davisson 出货（2025-10）；Bailly 经 Meta 验证 |
| 英伟达 | GPU（电/HBM 定义者）＋CPO 交换机＋上游投资 | 网络层先光化、GPU 封装内仍以铜为主；光布局对冲 HBM 格局被颠覆的风险 | Photonics 交换机发布（2025-03）；Spectrum-X CPO 向伙伴出货（TrendForce 2026-07） |
| 台积电 | COUPE 硅光制造＋先进封装平台 | 卖平台/产能，敞口挂钩 CPO 总量而非单一设计商 | COUPE 2026 量产（TrendForce 转述）；三大规模化难题为其高管自述 |

---

## Could not verify / 未能核实

1. **Volantis 估值**：A 轮投后估值在路透、公司新闻稿、官网均未披露。
2. **Volantis 客户与独立验证**：无具名客户、无第三方基准/拆解；官网 “Measured” 的 BER/眼图为公司自报。成立年份 2022 仅二手来源。
3. **Volantis 融资金额口径差**：早期页面 80M vs 正式公告 88M 的差异，公司未解释（Lapaas 指出）；本报告以 2026-10-01 正式公告与路透的 88M 为准并保留此注。
4. **英特尔 OCI 的 2025–2026 进展**：未检索到量产/具名客户的可靠更新，只能确认 2024 年原型状态。
5. **各家光方案的可比量产价格（$/Gbps）**：均未公开，第 4 节成本对比只能作定性＋系统级论证。
6. **Credo 收购 DustPhotonics 的确切对价、英伟达对 Corning/Lumentum/Coherent 的投资总额**：二手报道口径互相不一致（7.5 亿 vs 13 亿美元；5 亿 vs 最高 32 亿美元等），本报告只确认交易/合作存在，不采单一金额为定论。
7. **Lightmatter L20 采样时间（2026 年末）与 D 轮细节**：来自二手汇总，未见一手新闻稿逐项确认。
8. **三星 CPO 时间表口径**：2027/2028/2029 三个年份对应不同层级产品（代工服务/光引擎/交钥匙），韩媒转述之间仍有出入。

## Sources（主要来源，按主题）

- 路透 Volantis 融资，2026-10-01：https://www.reuters.com/business/volantis-raises-88-million-tech-connect-ai-memory-chips-2026-10-01/
- Volantis 公司新闻稿（PR Newswire），2026-10-01：https://www.prnewswire.com/news-releases/volantis-raises-88m-series-a-to-demolish-the-ai-memory-wall-with-photonics-302895940.html
- Volantis 官网（2026-10-03 实时核查）：https://volantissemi.ai/ ；https://volantissemi.ai/technology ；https://volantissemi.ai/about ；https://volantissemi.ai/product
- Lapaas 对 Volantis 口径差的复盘：https://lapaasvoice.com/ai-chips-volantis-raises-88m-for-photonic-memory/
- RuntimeWire Volantis，2026-10-01：https://runtimewire.com/article/volantis-88m-series-a-photonic-ai-memory
- The Register Blackwell Ultra 8 堆栈，2025-03-18：https://www.theregister.com/special-features/2025/03/18/nvidia-unveils-288-gb-blackwell-ultra-gpus/577013
- TechInsights B200 拆解，2025-04-14：https://www.globenewswire.com/news-release/2025/04/14/3061044/0/en/TechInsights-Releases-Initial-Findings-of-its-NVIDIA-Blackwell-HGX-B200-Platform-Teardown.html
- Ayar Labs UCIe 芯粒（BusinessWire），2025-03-31：https://www.businesswire.com/news/home/20250331044779/en/Ayar-Labs-Unveils-Worlds-First-UCIe-Optical-Chiplet-for-AI-Scale-Up-Architectures
- Ayar Labs 追加融资官方稿，2026-09-10：https://ayarlabs.com/news/ayar-labs-expands-2026-funding-to-650-million/
- Lightmatter M1000，2025-03-31：https://lightmatter.co/press-release/lightmatter-unveils-passage-m1000-photonic-superchip-worlds-fastest-ai-interconnect/?asPDF=1 ；1.6 Tbps/光纤，2026-03-11：https://lightmatter.co/press-release/lightmatter-achieves-record-1-6-tbps-per-fiber-to-accelerate-ai-optical-interconnect/?asPDF=1
- Marvell 收购 Celestial AI（新闻稿转述），2025-12-02：https://www.techpowerup.com/343597/marvell-to-acquire-celestial-ai-for-usd-3-25-billion
- Avicena LightBundle，2024-03-25：https://avicena.tech/avicena-announces-scalable-sub-pj-bit-lightbundletm-chiplet-interconnect-with-10m-reach/ ；eKit，2026-03-12：https://avicena.tech/avicena-launches-the-worlds-first-microled-optical-interconnect-eval-kit/
- 英特尔 OCI（EDN），2024-06：https://www.edn.com/intel-unveils-high-speed-optical-i-o-chiplet/
- 台积电 COUPE/三星路线图（TrendForce 转述 Commercial Times/The Elec），2026-04-01：https://www.trendforce.com/news/2026/04/01/news-silicon-photonics-race-intensifies-as-tsmc-targets-2026-coupe-production-samsung-eyes-2029-cpo-turnkey/
- 英伟达 Photonics 交换机（GlobeNewswire），2025-03-18：https://www.globenewswire.com/news-release/2025/03/18/3044903/0/en/NVIDIA-Announces-Spectrum-X-Photonics-Co-Packaged-Optics-Networking-Switches-to-Scale-AI-Factories-to-Millions-of-GPUs.html/
- 博通 TH6-Davisson 出货（GlobeNewswire 转载），2025-10-08：https://FinViz.com/news/186887/broadcom-announces-tomahawk-6-davisson-the-industrys-first-1024-tbps-ethernet-switch-with-co-packaged-optics
- TrendForce CPO 量产与瓶颈，2026-07-27：https://www.trendforce.com/presscenter/news/20260727-13151.html
- SK 海力士 CPO 论文（LEDinside 转述），2026-08-21：https://www.ledinside.com/news/2026/8/2026_08_21_01
- The Register CPO 可靠性与 Meta/Bailly 实测，2025-11-22：https://www.theregister.com/special-features/2025/11/22/copackaged-optics-have-officially-found-their-killer-app/2014010
- SDxCentral CPO 谨慎观点（Dell'Oro）：https://www.sdxcentral.com/analysis/co-packaged-optics-years-from-practicality-experts-say/
- Rockley 破产（eeNews Europe），2023-01-31：https://www.eenewseurope.com/en/rockley-photonics-files-for-bankruptcy-protection/
- Corning CPO 官方页（2026-10-03 核查）：https://www.corning.com/oem-solutions/worldwide/en/home/products-solutions/optical-communication-components/co-packaged-optics.html
- 路透 Corning 业绩与 Meta 协议，2026-01-28：https://www.reuters.com/business/corning-forecasts-first-quarter-sales-above-estimates-strong-optical-fiber-2026-01-28/
- Morningstar 转述 TD Cowen 对 Credo/CPO 辩论，2026-02-10：https://www.morningstar.com/news/marketwatch/2026021083/credos-stock-soars-as-new-numbers-score-the-chip-company-some-points-in-a-key-debate
- CPO 功耗综述转引（15–20 vs 5–10 pJ/bit）与挑战：https://en.wikipedia.org/wiki/Co-packaged_optics

*注：研究过程中未遇到要求偏离任务的页面指令；所有页面内容均仅作信源处理。*
