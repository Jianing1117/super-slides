---
name: super-slides
description: 创建美观、实用的演示材料。先生成 HTML 交互演示稿，可选导出 PPT/PDF。适用于用户想要制作演示、PPT、分享稿、演讲内容等场景。用户提到"做演示"、"做 PPT"、"做分享"、"演讲稿"、"培训材料"或直接说"super slides"时都应该触发此 skill。
---

# Super Slides

帮助用户创建美观、实用的演示材料。主输出为单文件 HTML 交互演示稿，可选导出 PPT/PDF 格式。

---

## Phase 1: 内容研究

**目标：整合多来源资料，生成完整的 Markdown 大纲，用户确认后锁定**

### Step 1.0: 入口询问

询问用户（header: "创建方式"）：
"你希望如何创建幻灯片？"

选项：
- **📁 基于现有文件** — 我有 PPT、文档或 URL，帮我增强和设计
- **📝 基于现有内容** — 我有文本或大纲，帮我结构化和设计
- **💡 从主题开始** — 我只有一个主题，需要帮我研究和生成内容

---

### 场景 1：基于现有文件

1. **收集文件**
   - 用户提供文件路径或 URL
   - 用 Read 工具读取文档/图片
   - 用 WebFetch 获取网页内容

2. **提取内容**
   - 解析文件结构
   - 提取关键信息

3. **资料补充**
   - 搜索本地工作区相关文档
   - 网络搜索补充最新案例/数据

4. **生成 Markdown 大纲**（带来源标注）

---

### 场景 2：基于现有内容

**Step 1.2-1: 询问处理方式**

询问用户（header: "内容处理"）：
"你希望如何处理你的内容？"

选项：
- **📋 保持原样** — 完全不动我的文字，只做结构化
- **📝 总结精简** — 帮我把内容压缩精简
- **✨ 扩写增强** — 帮我补充和丰富内容

**Step 1.2-2: 按选择处理**
- 保持原样 → 只做结构化，不改文字
- 总结精简 → 压缩冗余，提炼核心
- 扩写增强 → 补充案例、数据、细节

**Step 1.2-3: 资料补充**
- 搜索本地工作区相关文档
- 网络搜索补充最新案例/数据

**Step 1.2-4: 生成 Markdown 大纲**（带来源标注）

---

### 场景 3：从主题开始

**Step 1.3-1: 快速访谈（一次性问完）**

询问用户（header: "需求"）：

```
请告诉我以下信息，帮我更好地为你创建内容：

1. 你的用途是什么？你是什么角色？汇报/演讲对象是谁？
2. 大概需要多少页？使用什么语言？
3. PPT 的主题是什么？
4. 有可以提供的参考文件吗？
5. 有什么特殊要求吗？
```

**Step 1.3-2: 深入研究**
- 搜索本地工作区相关文档、笔记
- 网络搜索补充定义、案例、数据、最佳实践

**Step 1.3-3: 生成 Markdown 大纲**（带来源标注）

---

### 数据可视化处理

**触发条件：** 用户上传了 Excel/CSV 文件，或文档中包含数据表格/图表

**两种场景：**

| 场景 | 输入 | 处理方式 |
|------|------|----------|
| **从数据生成** | Excel/CSV 原始数据 | 分析数据 → 选择图表类型 → 生成 SVG |
| **美化现有图表** | 文档中的截图/图表 | 提取数据 → 重新渲染 → 生成 SVG |

---

#### Step 1.D1: 数据检测

当用户上传文件时，检查是否包含：
- `.xlsx` / `.csv` → 纯数据文件
- Word/PPT 中的表格 → 嵌入数据
- 图片中的图表 → 需要 OCR 或手动提取

---

#### Step 1.D2: 数据提取

**Excel/CSV：**
```
用 xlsx skill 读取数据
识别：表头、数据类型、数值范围
```

**现有图表（图片）：**
```
用 Read 工具查看图片
识别图表类型（柱状图/折线图/饼图/...）
提取关键数据点
如果无法准确提取，询问用户确认数据
```

**现有图表（Word/PPT 内嵌）：**
```
用 docx/pptx skill 提取
尽可能获取原始数据，而非截图
```

---

#### Step 1.D3: 图表类型推荐

根据数据特征推荐合适的图表：

| 数据特征 | 推荐图表 | 说明 |
|----------|----------|------|
| 分类 + 数值 | 柱状图 / 条形图 | 比较不同类别 |
| 时间序列 | 折线图 / 面积图 | 展示趋势变化 |
| 占比关系 | 饼图 / 环形图 | 展示部分与整体 |
| 多维度对比 | 分组柱状图 | 同时对比多个维度 |
| 相关性 | 散点图 | 展示变量关系 |
| 流程/漏斗 | 漏斗图 / 桑基图 | 展示转化流程 |

**询问用户（header: "图表"）：**
"我发现了一些数据，建议用 [图表类型] 展示，可以吗？"

选项：
- **"可以"** → 使用推荐类型
- **"换个类型"** → 用户指定其他类型
- **"不需要图表"** → 跳过，用文字呈现

---

#### Step 1.D4: 图表标记

在 Markdown 大纲中标记图表位置：

```markdown
## 第 3 页：销售数据回顾

**图表**：
{{chart: sales-chart-1}}

**要点**：
- 2024 年 Q1 销售额同比增长 23%
- 华东区域贡献最大，占比 42%
- 新产品线表现超出预期

---
```

图表数据保存到 `.claude-design/charts/sales-chart-1.json`：

```json
{
  "id": "sales-chart-1",
  "type": "bar",
  "title": "2024 Q1 各区域销售额",
  "data": {
    "labels": ["华东", "华南", "华北", "西南", "其他"],
    "values": [420, 280, 260, 150, 90]
  },
  "source": "[用户提供: sales.xlsx]"
}
```

---

#### Phase 3 生成图表 SVG

**在生成 HTML 时，读取 `.claude-design/charts/*.json`，为每个图表生成内联 SVG。**

**SVG 图表优势：**
- 矢量，无限缩放不失真
- 可以用 CSS 控制颜色（符合 VI）
- 文件小，可直接嵌入 HTML
- 支持动画（用 CSS animate path）

**生成原则：**
- 颜色使用 CSS 变量（`var(--color-primary)` 等）
- 字体使用演示稿字体（`var(--font-body)`）
- 简洁现代，去除不必要的装饰
- 数据标签清晰可读

---

### 内容来源标注

**三种来源都要整合：**

| 来源 | 说明 | 标注格式 |
|------|------|----------|
| **用户提供** | 用户直接输入/上传的内容 | `[用户提供]` |
| **本地资料** | 搜索用户工作区相关文档 | `[本地资料: xxx.md]` |
| **网络搜索** | 补充最新案例、数据、定义 | `[网络搜索]` |

> **注意**：来源标注用于帮助你了解内容来源，确认大纲后会自动清除，最终演示稿只保留干净的内容。

---

### 输出物：Markdown 大纲

```markdown
# 演示标题：XXXX

> **元信息**
> - 主题：XXX
> - 受众：XXX
> - 页数：XX 页
> - 语言：中文
> - 设计要求：结论先行、一页一重点、视觉统一

---

## 第 1 页：页面标题

**要点**：
- 要点一 [用户提供]
- 要点二 [本地资料: notes.md]
- 要点三 [网络搜索]

**口播文案**：（可选）
这里是对应的演讲稿...

---

## 第 2 页：...
```

**用户可以直接编辑这个 Markdown 文件**

---

### Step 1.4: 用户确认大纲

询问用户（header: "大纲"）：
"这是根据你的资料生成的演示稿大纲，确认吗？"

选项：
- **"确认，继续设计"**
- **"调整内容"** → 根据用户反馈修改
- **"我自己改一下"** → 等用户编辑后继续

**用户确认后执行：**
1. 删除所有来源标注（`[用户提供]`、`[本地资料: xxx]`、`[网络搜索]`）
2. 告知用户："来源标注已清除，大纲已锁定"

**只有用户确认后才进入 Phase 2**

---

## Phase 2: 风格发现

**这是"展示而非讲述"的阶段。** 大多数人无法用语言表达设计偏好。

### Step 2.0: 询问风格路径

询问用户（header: "风格"）：
"你想怎么确定演示稿的风格？"

选项：
- **"浏览风格库"（推荐）** — 从 30 种精选风格中选择
- **"上传参考素材"** — 上传 PPT、图片或品牌文档，我来提炼风格

---

### 路径 A: 浏览风格库

**触发：** 用户选择"浏览风格库"或没有参考素材

**Step 2.1A: 打开风格库**

```bash
open ~/.claude/skills/super-slides/style-preview.html
```

风格库包含 30 种风格，按使用场景组织：
- 🔮 科技未来 (5) — 技术演讲、产品发布、AI/区块链
- 💼 商务专业 (5) — 商业提案、财务报告、投资人路演
- 🎨 创意设计 (5) — 设计分享、创意提案、作品集
- 📖 优雅文艺 (6) — 文化分享、读书会、品牌故事
- 🌿 温暖友好 (4) — 教育讲座、社区分享、新手培训
- ⬜ 极简聚焦 (5) — TED 演讲、大会开场、金句展示

**Step 2.2A: 用户选择**

询问用户（header: "选择"）：
"你喜欢哪种风格？告诉我编号（如 05、12、23）"

记住选择，用于 Phase 3。风格定义在 [STYLE_PRESETS.md](STYLE_PRESETS.md)。

---

### 路径 B: 上传提炼

**触发：** 用户选择"上传参考素材"或描述了具体风格要求

**Step 2.1B: 收集参考**

询问用户（header: "素材"）：
"请分享你的参考素材（PPT、图片、品牌文档或网站链接），或者描述你想要的风格（颜色、字体、气质）"

**Step 2.2B: 提炼 VI 元素**

**如果用户提供视觉参考（PPT/图片/品牌文档）：**

1. **读取素材**
   - PPT → 用 pptx skill 提取内容
   - 图片 → 用 Read 工具读取
   - 品牌文档 → 用 Read 工具读取

2. **检查 Logo**
   - 是否存在品牌 Logo？
   - Logo 位置：左上角 / 右上角 / 右下角 / 居中
   - 如果有 Logo，记住位置，用于后续所有 slide

3. **提取颜色**
   - Primary: 主色
   - Secondary: 辅助色
   - Accent: 强调色
   - Background: 背景色

4. **提取字体**
   - Display font: 标题用
   - Body font: 内容用

5. **视觉特征**
   - 圆角: 锐利 / 柔和 / 圆润
   - 阴影: 无 / 柔和 / 硬边
   - 渐变: 无 / 微妙 / 大胆

**如果用户提供文字描述：**
- 解析描述中的具体要求
- 根据描述的气质填补空白

**提炼检查清单：**

1. **颜色**
   - Primary: 主色
   - Secondary: 辅助色
   - Accent: 强调色
   - Background: 背景色

2. **字体**
   - Display font: 标题用
   - Body font: 内容用

3. **视觉特征**
   - 圆角: 锐利 / 柔和 / 圆润
   - 阴影: 无 / 柔和 / 硬边
   - 渐变: 无 / 微妙 / 大胆

4. **整体气质**
   - 专业度: 正式 / 中性 / 友好
   - 现代感: 经典 / 现代 / 前卫
   - 情感基调: 冷静 / 温暖 / 活力

**Step 2.3B: 生成风格预览卡片**

创建单卡片 HTML 预览：
- 使用提取的颜色
- 应用字体搭配
- 展示视觉特征
- 显示气质关键词
- **如果有 Logo**：将 Logo 嵌入预览卡片

保存到 `.claude-design/custom-style-preview.html` 并在浏览器中打开。

**Step 2.4B: 用户确认**

询问用户（header: "确认"）：
"这是根据你的品牌提炼的风格，可以吗？"

选项：
- "可以，继续"
- "调整一下"

如果"调整一下"，询问要调整什么，重新生成预览。

**Step 2.5B: 保存风格配置**

确认后保存到 `.claude-design/custom-style.json`：

```json
{
  "styleName": "Custom Brand Style",
  "colors": {
    "primary": "#1E2761",
    "secondary": "#CADCFC",
    "accent": "#D4AF37",
    "background": "#0a0a0a"
  },
  "fonts": {
    "display": "Playfair Display",
    "body": "Lato"
  },
  "traits": {
    "borderRadius": "4px",
    "shadowStyle": "soft",
    "gradientUse": "subtle"
  },
  "vibe": ["professional", "luxury", "trustworthy"],
  "logo": {
    "src": "path/to/logo.png",
    "position": "top-left"
  }
}
```

> **Logo position 可选值：** `top-left`（左上角）、`top-right`（右上角）、`bottom-right`（右下角）、`center`（居中）

---

## Phase 3: 生成演示稿

**Phase 1 已确认内容，Phase 2 已确认风格 → 直接生成，无需再确认**

### Step 3.0: 加载已确认数据

从 Phase 1 获取：
- `slide_outline`: [{ type, title, content, images? }, ...]
- `slide_count`: number

从 Phase 2 获取：
- `style_id`: "01"-"30" 或
- `custom_style`: { colors, fonts, traits, vibe, logo? }

### Step 3.1: 加载风格定义

**如果是预设编号：**
1. 读取 [STYLE_PRESETS.md](STYLE_PRESETS.md) 中对应风格
2. 提取 colors、fonts、traits

**如果是定制风格：**
1. 读取 `.claude-design/custom-style.json`
2. 检查是否有 `logo` 字段

### Step 3.2: 加载模板文件

**必须读取：**
- [viewport-base.css](viewport-base.css) → 完整复制到 `<style>` 中
- [html-template.md](html-template.md) → 参考 HTML 结构
- [animation-patterns.md](animation-patterns.md) → 参考动画实现

### Step 3.3: 设计思考

**在生成代码前，先思考设计方向：**

1. **整体调性** — 根据风格定义确认调性：是极简克制，还是丰富热烈？是经典优雅，还是前卫大胆？
2. **记忆点** — 这个演示稿最让人记住的一个视觉特征是什么？
3. **动画策略** — 选择 1-2 种核心动画效果，贯穿始终（不要每页不同）

**调性参考：**
- 极简克制 → 大量留白、精确的间距、微妙的动画
- 丰富热烈 → 层叠渐变、动态背景、大胆的排版
- 经典优雅 → 衬线字体、对称布局、克制的装饰
- 前卫大胆 → 非对称、打破网格、意外的颜色组合

### Step 3.4: 设计美学指南

**字体：**
- 选择独特、有性格的字体，避免 Inter、Roboto、Arial
- 标题用 Display 字体（衬线/装饰性），正文用 Body 字体（易读）
- 从 Google Fonts 或 Fontshare 加载

**颜色：**
- 用 CSS 变量统一管理
- 主色占主导，强调色只用于关键点
- 背景要有层次：渐变、纹理、或微妙的图案

**动画：**
- 一致性 > 多样性：选择一种动画风格贯穿始终
- 重点时刻用动画：页面进入、关键数据、章节转换
- 支持 `prefers-reduced-motion`

**空间：**
- 可以尝试非对称布局、元素重叠、对角线流动
- 不要每页都居中，创造视觉节奏
- 内容密度要克制，留白是设计的一部分

**背景：**
- 避免纯色背景，添加：渐变 mesh、噪点纹理、几何图案、层叠透明度

**避免 "AI slop"：**
- ❌ 紫色渐变 + 白色背景
- ❌ 居中对称的卡片布局
- ❌ 统一的圆角
- ❌ Inter / Roboto / Space Grotesk
- ❌ 每页相同的模板结构

---

### Step 3.5: 内容密度限制

**每页内容有上限，超出则拆分：**

| 页面类型 | 最大内容 |
|----------|----------|
| 标题页 | 1 个标题 + 1 个副标题 + 可选 tagline |
| 内容页 | 1 个标题 + 4-6 个要点 或 2 段文字 |
| 功能网格 | 1 个标题 + 最多 6 张卡片（2x3 或 3x2）|
| 引用页 | 1 段引用（最多 3 行）+ 署名 |
| 代码页 | 1 个标题 + 8-10 行代码 |
| 图片页 | 1 个标题 + 1 张图片（最高 60vh）|

**内容超出？拆成多页，不要硬塞。**

---

### Step 3.6: Viewport 适配规则

**这些规则不可违反：**

```css
/* 每个 slide 必须有 */
.slide {
  height: 100vh;
  height: 100dvh;  /* 移动端动态视口 */
  overflow: hidden;
}

/* 所有字号必须用 clamp() */
h1 { font-size: clamp(2rem, 5vw, 4rem); }
p  { font-size: clamp(1rem, 2vw, 1.25rem); }

/* 图片必须有 max-height */
img { max-height: min(50vh, 400px); }

/* 需要断点适配 */
@media (max-height: 700px) { /* 紧凑布局 */ }
@media (max-height: 600px) { /* 更紧凑 */ }
@media (max-height: 500px) { /* 最小高度适配 */ }
```

**完整规则在 [viewport-base.css](viewport-base.css) 中，必须完整复制到每个演示稿。**

---

### Step 3.7: 生成 HTML

**输出结构：**

```html
<!DOCTYPE html>
<html lang="zh">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[主题]</title>

  <!-- 字体加载：选择独特字体 -->
  <link href="https://fonts.googleapis.com/css2?family=..." rel="stylesheet">

  <style>
    /* ========================================
       SECTION 1: VIEWPORT BASE（完整复制）
       ======================================== */
    /* viewport-base.css 的完整内容 */

    /* ========================================
       SECTION 2: CSS 变量
       ======================================== */
    :root {
      --color-primary: ...;
      --color-secondary: ...;
      --color-accent: ...;
      --color-bg: ...;
      --font-display: "Font Name", serif;
      --font-body: "Font Name", sans-serif;
      --radius: ...;
      --shadow: ...;
    }

    /* ========================================
       SECTION 3: 背景与氛围
       ======================================== */
    /* 渐变、纹理、图案 */

    /* ========================================
       SECTION 4: 动画定义
       ======================================== */
    /* 入场动画、微交互 */
    /* 必须包含 prefers-reduced-motion 支持 */

    /* ========================================
       SECTION 5: 布局与 Slide 样式
       ======================================== */
    .slide { ... }
    .slide-title { ... }
    .slide-content { ... }

    /* ========================================
       SECTION 6: 导航
       ======================================== */
    .nav-dots { ... }

    /* ========================================
       SECTION 7: 特殊页面样式
       ======================================== */
    /* 标题页、引用页、网格页等 */
  </style>
</head>
<body>
  <!-- Logo（如有）-->
  <img class="brand-logo" src="..." alt="Logo">

  <div class="slides-container">
    <!-- 按大纲逐页生成 -->
    <section class="slide" data-slide="1">
      <!-- 内容 -->
    </section>
    ...
  </div>

  <nav class="nav-dots">
    <button class="nav-dot" data-slide="1"></button>
    ...
  </nav>

  <script>
    /* ========================================
       SECTION 1: 导航逻辑（垂直滚动）
       ======================================== */
    // 键盘：↑ ↓ 方向键 / Page Up / Page Down / Home / End
    // 触屏：上下滑动
    // 滚动：scroll-snap 自动吸附

    /* ========================================
       SECTION 2: 动画逻辑
       ======================================== */
    // Intersection Observer 触发入场动画

    /* ========================================
       SECTION 3: 可选功能
       ======================================== */
    // 进度条、计时器等
  </script>
</body>
</html>
```

**Logo 处理：**
- 固定定位在所有 slide 上层
- 位置由 `logo.position` 决定：`top-left` / `top-right` / `bottom-right` / `center`

---

### Step 3.8: 代码质量标准

- **分区清晰** — 每个主要 section 用注释块分隔
- **命名一致** — CSS 类名使用统一命名规范
- **响应式** — 所有尺寸用 `clamp()` 或 `vw`/`vh`
- **可访问性** — 语义化标签、焦点状态、键盘导航
- **性能** — CSS 优先动画，避免复杂 JS

---

### Step 3.9: 自动验证

- [ ] 每个 `.slide` 有 `height: 100vh; overflow: hidden;`
- [ ] 所有字号使用 `clamp()`
- [ ] 图片有 `max-height` 限制
- [ ] 导航点数量 = slide 数量
- [ ] `prefers-reduced-motion` 支持
- [ ] Logo 位置正确（如有）
- [ ] 代码分区清晰，有注释
- [ ] 无 "AI slop" 特征（紫色渐变、居中卡片、Inter 字体等）

### Step 3.10: 生成输出文件夹

**统一输出为一个文件夹，方便移植：**

```
~/Desktop/[主题]-slides/
├── index.html              # 主版本（VI 字体）
├── index-backup.html       # 备份版本（系统字体）
└── assets/                 # 图片资源（如有）
    ├── logo.png
    ├── chart-data-1.png
    └── ...
```

**文件夹结构说明：**

| 文件/文件夹 | 内容 | 必须 |
|-------------|------|------|
| `index.html` | 主版本 HTML | ✅ |
| `index-backup.html` | 备份版本（系统字体） | ✅ |
| `assets/` | 用户上传的图片 | 如有则生成 |

**图片路径引用：**
```html
<!-- HTML 中使用相对路径 -->
<img src="assets/logo.png" alt="Logo">
```

**备份版本字体映射：**

```css
/* 主版本（VI 字体）*/
--font-display: "Clash Display", sans-serif;
--font-body: "Satoshi", sans-serif;

/* 备份版本（系统字体）*/
--font-display: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Microsoft YaHei", sans-serif;
--font-body: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Microsoft YaHei", sans-serif;
```

**生成方式：**
1. 创建文件夹 `~/Desktop/[主题]-slides/`
2. 生成主版本 `index.html`
3. 复制一份为 `index-backup.html`，只替换字体变量
4. 如有图片，创建 `assets/` 文件夹，复制图片进去

### Step 3.11: 保存并打开

```bash
open ~/Desktop/[主题]-slides/index.html
```

---

## Slide 模板库

| 模板类型 | 用途 | 核心元素 |
|----------|------|----------|
| `title` | 标题页 | `<h1>`, `<h2>`, tagline |
| `content` | 内容页 | `<h2>` + `<ul>` / `<ol>` |
| `two-column` | 对比 | 左右两个 `<div>` |
| `feature-grid` | 功能网格 | 2x3 或 3x2 卡片 |
| `quote` | 引用 | `<blockquote>` + `<cite>` |
| `chart` | 数据图表 | 内联 `<svg>` + 标题 + 说明 |
| `image-left` | 图左文右 | `<img>` + `<div>` |
| `image-right` | 图右文左 | `<div>` + `<img>` |
| `full-image` | 全屏图 | `<img>` + 叠加标题 |
| `section` | 章节分隔 | 大号 `<h2>` |
| `ending` | 结束页 | 感谢语 / Q&A / 联系方式 |

根据 Phase 1 大纲中每页的 `type` 字段选择对应模板。

---

## 图表 SVG 生成指南

**当 Phase 1 中标记了 `{{chart: xxx}}` 时，读取对应的 `.claude-design/charts/xxx.json`，生成内联 SVG。**

### 图表类型与 SVG 结构

**柱状图：**
```svg
<svg class="chart bar-chart" viewBox="0 0 600 400">
  <!-- 标题 -->
  <text class="chart-title" x="300" y="30">[图表标题]</text>

  <!-- Y 轴 -->
  <g class="axis y-axis">
    <line x1="60" y1="60" x2="60" y2="320" stroke="var(--text-secondary)"/>
    <!-- 刻度和标签 -->
  </g>

  <!-- X 轴 -->
  <g class="axis x-axis">
    <line x1="60" y1="320" x2="560" y2="320" stroke="var(--text-secondary)"/>
    <!-- 类别标签 -->
  </g>

  <!-- 柱子 -->
  <g class="bars">
    <rect class="bar" x="80" y="100" width="60" height="220" fill="var(--color-primary)"/>
    <!-- 更多柱子... -->
  </g>

  <!-- 数值标签 -->
  <g class="value-labels">
    <text x="110" y="90" class="value">420</text>
    <!-- 更多标签... -->
  </g>
</svg>
```

**折线图：**
```svg
<svg class="chart line-chart" viewBox="0 0 600 400">
  <!-- 坐标轴 -->

  <!-- 数据线 -->
  <polyline
    class="data-line"
    points="80,280 160,220 240,180 320,200 400,140 480,100"
    fill="none"
    stroke="var(--color-primary)"
    stroke-width="3"
  />

  <!-- 数据点 -->
  <g class="data-points">
    <circle cx="80" cy="280" r="6" fill="var(--color-accent)"/>
    <!-- 更多点... -->
  </g>
</svg>
```

**饼图/环形图：**
```svg
<svg class="chart pie-chart" viewBox="0 0 600 400">
  <!-- 使用 stroke-dasharray 技巧绘制扇形 -->
  <circle
    class="pie-segment"
    cx="200" cy="200" r="120"
    fill="none"
    stroke="var(--color-primary)"
    stroke-width="120"
    stroke-dasharray="377 754"  <!-- 周长的一半 = 2πr/2 -->
    stroke-dashoffset="0"
    transform="rotate(-90 200 200)"
  />
  <!-- 更多扇形... -->

  <!-- 图例 -->
  <g class="legend" transform="translate(400, 100)">
    <rect width="16" height="16" fill="var(--color-primary)"/>
    <text x="24" y="14">类别 A</text>
    <!-- 更多图例项... -->
  </g>
</svg>
```

### 图表样式规范

```css
/* 图表容器 */
.chart {
  max-width: 100%;
  max-height: min(50vh, 400px);
  margin: 0 auto;
}

/* 文字使用主题字体 */
.chart-title {
  font-family: var(--font-display);
  font-size: clamp(1rem, 2vw, 1.25rem);
  fill: var(--text-primary);
  text-anchor: middle;
}

.chart text {
  font-family: var(--font-body);
  font-size: clamp(0.75rem, 1.2vw, 0.875rem);
  fill: var(--text-secondary);
}

/* 数值标签突出 */
.value {
  font-weight: 600;
  fill: var(--text-primary);
}

/* 柱子/线条动画 */
.bar, .data-line, .pie-segment {
  animation: chartGrow 0.8s var(--ease-out-expo) forwards;
}

@keyframes chartGrow {
  from { transform: scaleY(0); opacity: 0; }
  to { transform: scaleY(1); opacity: 1; }
}

/* 支持 prefers-reduced-motion */
@media (prefers-reduced-motion: reduce) {
  .bar, .data-line, .pie-segment {
    animation: none;
  }
}
```

### 颜色分配

当图表有多个数据系列时，按顺序使用：
1. `var(--color-primary)`
2. `var(--color-secondary)`
3. `var(--color-accent)`
4. 自动生成同色系变体（加深/变浅）

### 图表页面模板

```html
<section class="slide chart-slide">
  <h2 class="reveal">[图表标题]</h2>
  <div class="chart-container reveal">
    <!-- 内联 SVG 图表 -->
    <svg class="chart" ...>...</svg>
  </div>
  <p class="chart-insight reveal">
    <!-- 核心洞察，一句话说明图表的关键信息 -->
  </p>
</section>
```

### 图表可靠性保障

**SVG 图表的优势：**
- ✅ 完全内联，不依赖外部文件
- ✅ 矢量格式，任意缩放不失真
- ✅ 随 HTML 一起部署，无额外请求
- ✅ 支持动画和交互

**唯一风险：字体**

SVG 中的文字使用 `var(--font-body)` 引用主题字体。如果字体从 CDN 加载：
- 在线环境 → 正常显示
- 离线环境 → fallback 到系统字体（视觉略有差异，但内容完整）

**重要演示保障方案：**

| 场景 | 推荐方案 |
|------|----------|
| 正常使用 | HTML 即可，图表 100% 可靠 |
| 重要演讲 | 同时导出 PDF 作为备份 |
| 完全离线 | 内联字体（增加 ~100KB）或用 PDF |
| 投影仪/陌生电脑 | 提前测试，或用 PDF |

**结论：图表本身不会出现"无法显示"的情况，唯一变量是字体。**

---

## Phase 4: 交付

### Step 4.1: 打开 HTML 文件

```bash
open ~/Desktop/[主题]-slides/index.html
```

### Step 4.2: 交付总结

告诉用户：

```
✅ 演示稿已生成完成！

📁 文件夹：~/Desktop/[主题]-slides/
   ├── index.html          # 主版本（精美字体）
   ├── index-backup.html   # 备份版本（系统字体）
   └── assets/             # 图片（如有）

🎨 风格：[风格名称]
📊 页数：[N] 页
📈 图表：[N] 个（如有）

📌 使用方式：
- 键盘：↑ ↓ 方向键 / Page Up / Page Down 切换页面
- 触屏：上下滑动
- 鼠标：滚动 / 点击右侧导航点
```

**然后说明两个版本的区别：**

```
📦 生成了两个版本：

1️⃣ index.html（主版本）
   → 使用你选择的精美字体
   → 需要网络加载字体（Google Fonts）
   → 适合：本地演示、在线分享

2️⃣ index-backup.html（备份版本）
   → 使用系统字体（苹方/微软雅黑等）
   → 完全离线可用，无字体加载问题
   → 适合：异地演示、陌生电脑、离线环境
```

### Step 4.2-1: 如何修改（重要！必须告知）

**在用户预览 HTML 时，主动告知：**

```
💡 需要修改？不用自己改代码！

PPT 可以直接改文字，但 HTML 你可能不知道怎么改。
没关系，直接在这里告诉我要改什么：

🗣️ 比如：
   "把第 3 页的标题改成 XXX"
   "第 5 页的那个图表数字不对，应该是 85%"
   "删掉第 7 页"
   "在结尾加一页联系方式"
   "整体颜色换成蓝色系"

我会帮你改好，你只需要刷新页面看效果就行。
不需要你懂任何 HTML！
```

### Step 4.2-2: 异地部署提醒（如有图片时必须告知）

**如果生成了 `assets/` 文件夹（有图片），必须提醒用户：**

```
⚠️ 重要提醒：需要转移到其他电脑？

你有 [N] 张图片在 assets/ 文件夹中。

📦 请复制整个文件夹：
   ✅ [主题]-slides/（整个文件夹）
   ❌ 不要只复制 index.html

💡 移植方式：
   → U 盘：复制整个 [主题]-slides 文件夹
   → 网盘：压缩整个文件夹后上传
   → 邮件：压缩后发送

🎯 最省事方案（如果担心漏文件）：
   → 使用 index-backup.html + 无本地图片
   → 单文件，复制即用，零依赖
```

**如果没有图片，简单说明：**

```
📦 转移到其他电脑：
   ✅ 复制整个 [主题]-slides 文件夹即可
   → 文件夹内所有内容都已自包含
```

### Step 4.3: 修改与后续（紧接着 Step 4.2-1）

**先询问用户：**

```
📖 预览效果如何？需要调整吗？

🗣️ 小改动 → 直接告诉我要改什么
   比如："第 3 页标题改成 XXX" / "删掉第 5 页"

🔄 大改动 / 不满意 → 选择下面的选项
```

询问用户（header: "后续"）：

选项：
- **"小改一下"** → 用户口头描述 → 直接修改 HTML → 刷新预览
- **"改动较大"** → 返回 Phase 1 调整大纲
- **"换个风格"** → 返回 Phase 2 重新选择
- **"导出 PPT/PDF"** → 进入 Step 4.4
- **"部署到线上"** → 进入 Step 4.5（获得分享链接）
- **"完美，完成"** → 结束流程

**"小改一下"的处理方式：**
1. 用户用自然语言描述要改的内容
2. 直接修改 HTML 文件
3. 告知"已改好，刷新页面看效果"
4. 回到 Step 4.3 继续询问

---

### Step 4.4: 导出其他格式（可选）

**如果用户选择"导出 PPT/PDF"：**

询问用户（header: "格式"）：
"需要导出什么格式？"

选项：
- **"PDF"** → Playwright 截图合成（推荐，效果最好）
- **"PPT"** → pptx skill 生成（可用，但部分样式可能简化）
- **"都要"** → 同时生成两种格式

---

**导出 PDF（推荐）：**

使用 Playwright 截图每个 slide，然后合成 PDF：

```bash
bash scripts/export-pdf.sh ~/Desktop/[主题]-slides/index.html
```

**特点：**
- ✅ 100% 还原 HTML 视觉效果（颜色、字体、布局）
- ✅ 图表完整保留
- ❌ 动画变成静态（这是正常的）
- ⏱️ 首次运行需下载 Chromium（~150MB），约 30-60 秒
- ⏱️ 后续导出很快（~5-10 秒）

**如果文件太大（>10MB），使用 `--compact`：**
```bash
bash scripts/export-pdf.sh ~/Desktop/[主题]-slides/index.html --compact
```
（分辨率从 1920×1080 降到 1280×720，文件减小 50-70%）

---

**导出 PPT：**

使用 pptx skill（pptxgenjs）：

```bash
# 调用 pptx skill，传入：
# - 内容大纲
# - 风格颜色/字体
# - 图片路径（如有）
```

**特点：**
- ✅ 可在 PowerPoint/Keynote 中编辑
- ⚠️ 部分高级效果可能简化（复杂动画、特殊字体）
- ⚠️ 图表转为静态图片

---

**导出完成后告知用户：**

```
✅ 导出完成！

📄 [主题]-slides.pdf（或 .pptx）
📁 位置：~/Desktop/[主题]-slides/

💡 PDF 完美还原视觉效果，PPT 可继续编辑。
```

---

### Step 4.5: 部署到线上（可选）

**如果用户选择"部署到线上"：**

```bash
bash scripts/deploy.sh ~/Desktop/[主题]-slides/
```

**首次部署会：**
1. 检查是否安装 Vercel CLI → 没有则自动安装
2. 检查是否登录 → 没有则引导用户登录（打开浏览器授权）
3. 部署完成后返回一个公开 URL

**部署完成后告知用户：**

```
✅ 已部署到线上！

🌐 URL：https://[随机].vercel.app
📱 扫码即可在手机/平板查看

📌 特点：
- 任何设备都能访问（手机、平板、电脑）
- 可以分享给其他人
- 永久有效（除非你手动删除）
- 免费托管

💡 管理方式：
- 查看所有部署：vercel.com/dashboard
- 删除部署：在 Dashboard 中点击项目 → Settings → Delete
```

**⚠️ 部署注意事项：**
- 如果有 `assets/` 文件夹（图片），会一起部署
- 主版本需要网络加载字体 → 联网环境下正常显示
- 备份版本用系统字体 → 离线也能正常显示

## 运行时生成的文件

```
.claude-design/
├── custom-style.json        # 定制风格配置（Path B）
├── custom-style-preview.html # 风格预览卡片
└── charts/
    ├── sales-chart-1.json   # 图表数据定义
    └── growth-chart-2.json
```
