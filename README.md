# Super Slides Skill

创建美观、实用的演示材料。先生成 HTML 交互演示稿，可选导出 PPT/PDF。

## 功能特点

- 🎨 **30 种精选风格** — 科技未来、商务专业、创意设计、优雅文艺、温暖友好、极简聚焦
- 📊 **数据可视化** — 从 Excel 数据自动生成 SVG 图表
- 🎯 **品牌定制** — 按你的 VI 提取颜色、字体、风格
- 📦 **双版本输出** — 主版本（精美字体）+ 备份版本（系统字体，离线可用）
- 📤 **多种导出** — HTML / PPT / PDF
- 🌐 **一键部署** — 部署到 Vercel 获得分享链接

## 安装方法

```bash
# 进入你的 skills 目录
cd ~/.claude/skills/

# 克隆这个仓库
git clone https://github.com/你的用户名/super-slides.git
```

## 使用方法

在 Claude Code 中说：
- "帮我做个 PPT"
- "做演示"
- "演讲稿"
- "培训材料"

## 工作流程

```
Phase 1: 内容研究 → 生成大纲 → 用户确认
    ↓
Phase 2: 风格发现 → 浏览风格库 / 上传品牌素材
    ↓
Phase 3: 生成 HTML（主版本 + 备份版本）
    ↓
Phase 4: 交付 → 可选导出 PPT/PDF / 部署到线上
```

## 文件结构

```
super-slides/
├── SKILL.md              # Skill 主文件
├── STYLE_PRESETS.md      # 30 种风格定义
├── style-preview.html    # 风格可视化浏览器
├── viewport-base.css     # 响应式 CSS 基础
├── html-template.md      # HTML 结构模板
├── animation-patterns.md # 动画片段库
├── scripts/
│   ├── deploy.sh         # Vercel 部署脚本
│   ├── export-pdf.sh     # PDF 导出脚本
│   └── extract-pptx.py   # PPT 提取脚本
└── evals/
    └── evals.json        # 测试用例
```

## 依赖 Skills

| Skill | 用途 |
|-------|------|
| xlsx | 读取 Excel/CSV 数据 |
| pptx | 导出 PPT 格式 |
| pdf | 导出 PDF 格式 |

## 许可证

MIT License
