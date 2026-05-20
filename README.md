# Amazon Design — Pacvue Design Explorations

设计探索仓库，收录 Pacvue Amazon 广告模块的 UI 原型与交互方案。

---

## 📁 benchmark 2.0 renew

### Brand Benchmark v3.2 UI ✨ Latest

更新日期：2026-05-20

**👉 [点击查看 Brand Benchmark v3.2 UI 原型](https://kiwilu-bit.github.io/amazon-design/benchmark%202.0%20renew/brand-benchmark-v3.2-ui.html)**

#### v3.2 更新亮点

- **7 步新手引导（全流程覆盖）**：
  - **Step 1**：Category Depth Levels — 介绍 L1–L4 类目层级与精准对标关系
  - **Step 2**：Understanding Percentile Benchmarks — P25 / P50 / P75 含义与「高/低 better」说明
  - **Step 3**：Details in each Cell — 聚焦 VS Pxx 区域，介绍自动对标下一阶梯 + 点击跳转详情
  - **Step 4**：Performance vs Benchmark — 自动打开详情面板，定位核心指标模块
  - **Step 5**：Brand Distribution — 定位竞品分布模块
  - **Step 6**：Find Campaigns That Need Attention — 定位 Underperforming Campaigns 模块
  - **Step 7**：You're all set — 自动关闭详情页，回到首页，聚焦 Filter 区域，以「Got it!」收尾
- **Step 3 聚焦优化**：spotlight 精确定位到单元格右上角 VS Pxx 区域，而非整个单元格

---

### Brand Benchmark v3.0 UI 调整版

更新日期：2026-05-19

**👉 [点击查看 Brand Benchmark v3.0 UI 调整版原型](https://kiwilu-bit.github.io/amazon-design/benchmark%202.0%20renew/brand-benchmark-v3.0-ui.html)**

#### v3.0 UI 调整版亮点

- **完整详情面板（6 个模块）**：点击任意品牌格子进入深度分析页，包含以下模块：
  - **§1 Performance vs Benchmark**：CTR / CPC / CPM 等核心指标与 P25 / P50 / P75 基准值对比，展示 vs 上期变化幅度
  - **§2 Trend Performance**：6 个月趋势折线图，P25–P50 / P50–P75 阴影带正确展示，保证 P25 < P50 < P75 每月数据有序
  - **§3 Brand Distribution**：矩阵表格展示同类目竞品排名与指标分位
  - **§4 Category Comparison**：分 L1–L4 层级的类目对比表，sticky 第一二列，横向滚动查看各指标
  - **§5 Brand Type Distribution**：雷达图（SP / SB / SD 三类型叠加，8 轴，P0–P100 同心环）+ 右侧 Campaign Type 对比表
  - **§6 Underperforming Campaigns**：Top 10 表现欠佳广告活动，含 State / Daily Budget / 不达标 Targetings 数量 / 指标 tier 标签
- **5 步新手引导**：完整新手 Tour，步骤 3 自动打开详情面板，步骤 4–5 滚动定位至对应模块；Tour 期间屏蔽主表格 hover tooltip
- **细节优化**：
  - Tier tag 改为带背景色的圆角标签（P0–P25 绿 / P25–P50 蓝 / P50–P75 橙 / P75–P100 红）
  - 雷达图图例支持点击切换 SP / SB / SD 显示 / 隐藏
  - Trend 图 band 路径修复，蓝色 = P25–P50，橙色 = P50–P75，两段精确首尾相接

---

### Brand Benchmark v2.0

更新日期：2026-05-18

**👉 [点击查看 Brand Benchmark v2.0 原型](https://kiwilu-bit.github.io/amazon-design/benchmark%202.0%20renew/brand-benchmark-v2.0.html)**

#### v2.0 升级亮点

- **V5 Strip + Micro Grid 单元格**：左侧彩色竖条直观显示层级，右侧 3 列微型网格同屏对比 P25 / P50 / P75 基准值与涨跌幅
- **当前 Benchmark 列高亮**：所选 P25 / P50 / P75 对应列自动蓝色高亮，快速定位
- **色彩编码**：绿（P0–P25）/ 蓝（P25–P50）/ 橙（P50–P75）/ 红（P75–P100）

---

### Brand Benchmark v1.0

更新日期：2026-05-18

**👉 [点击查看 Brand Benchmark v1.0 原型](https://kiwilu-bit.github.io/amazon-design/benchmark%202.0%20renew/brand-benchmark-v1.0.html)**

#### 设计目标

> 让运营人员一目了然地了解：
> 1. 自己每个指标模块**所处的等级**（P0–P25 / P25–P50 / P50–P75 / P75–P100）
> 2. 各等级的**基准数据**（P25 · P50 · P75 具体数值）
> 3. 当前数值与**下一个等级**之间还差多少（vs P25 / P50 / P75 对比）

#### 功能亮点

- **热力图矩阵**：SP / SB / SD 三种广告类型，多指标横向对比
- **分位层级标签**：每个单元格直接显示所属 P 分位区间（P0–P25 等）
- **三档基准值**：P25 · P50 · P75 具体数值一行展示
- **三档 Delta**：与 P25 / P50 / P75 的涨跌幅对比，快速识别差距
- **Category Layer 筛选**：L1–L4 深度控制，各品牌独立联动
- **Distribution 筛选**：按分位层级过滤，颜色即图例
- **Hover Tooltip**：悬停展示详细数据，支持点击进入详情

---

*Pacvue Design · Internal Prototype*
