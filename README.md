# TailorFit 产品介绍页

面向 TailorFit 微信小程序的独立静态宣传落地页，引导新用户扫码体验小程序。

## 项目简介

TailorFit 是一款微信小程序，帮助用户将目标、训练经验、器械条件和时间安排整理成可执行的四周训练计划。本项目是为该小程序配套的静态介绍页，提供产品说明、功能展示和体验入口。

- **定位**：纯静态产品介绍页，无后端、无用户数据收集
- **目标用户**：想认真开始训练的新手与进阶爱好者
- **主要行动入口**：微信扫码体验版二维码

## 当前已实现

- 响应式产品介绍页（桌面端 + 移动端，breakpoint 768px）
- 品牌展示与产品定位说明
- 体验版二维码入口（`assets/tailorfit-experience-qr.png`）
- 八大功能卡片展示：
  1. AI 个性化四周计划（DeepSeek）
  2. 六套内置方案（新手增肌、进阶增肌、减脂全身、居家自重、核心与体能、拉伸与活动度）
  3. 本地手动创建计划
  4. 训练仪表盘（日期条、状态、进度）
  5. 真实打卡（记录组数、重量、RPE、睡眠、心情、体重）
  6. 本地训练分析（本周 / 4 周 / 12 周）
  7. 当前计划可调整（不改原计划库）
  8. JSON 导入导出（含 API Key 保护）
- 四步使用流程（了解自己 → 选择方式 → 完成训练 → 分析变化）
- 安全与隐私说明（本地优先、AI 非医疗建议）
- 适用人群标签
- 滚动入场动画（尊重 `prefers-reduced-motion`）
- WCAG 2.2 AA 可访问性：焦点状态可见、图片有 alt 文本、语义化 HTML

## 技术栈

| 层       | 技术                                         |
| -------- | -------------------------------------------- |
| 前端     | HTML5 + CSS3（自定义属性 / Grid / Flexbox）  |
| 脚本     | Vanilla JavaScript（IntersectionObserver）   |
| 后端     | 无                                           |
| 构建     | 无                                           |
| 部署     | 任意静态托管服务或直接双击打开               |

## 视觉风格

Claude 研究札记风格：暖象牙纸色背景（`#f4f0e8`）+ 近黑石板色（`#2b2b28`）+ 陶土橙强调（`#bd5b3e`），硬边无阴影，无渐变。

## 快速开始

### 本地预览

直接用浏览器打开项目根目录下的 `index.html`：

```powershell
# Windows
Start-Process index.html

# macOS
open index.html

# Linux
xdg-open index.html
```

### 静态结构验证

```powershell
$html = Get-Content -Raw 'index.html'
@('id="intro"','id="features"','id="flow"','id="safety"','id="audience"','id="experience"','assets/tailorfit-experience-qr.png','prefers-reduced-motion','aria-label','alt=') | ForEach-Object { if ($html -notmatch [regex]::Escape($_)) { throw "Missing: $_" } }
if ($html -match '[Tt][Oo][Dd][Oo]|[Tt][Bb][Dd]|[Ll]orem ipsum') { throw 'Placeholder copy found' }
if ($html -match 'box-shadow|radial-gradient|linear-gradient') { throw 'Disallowed visual primitive found' }
Write-Output 'Static checks passed'
```

## 仓库结构

```
.
├── index.html                  # 主页面（HTML + CSS + JS 内嵌，单文件）
├── assets/
│   └── tailorfit-experience-qr.png  # 体验版二维码图片
├── docs/
│   └── superpowers/
│       ├── plans/              # 开发计划文档
│       └── specs/              # 设计规格文档
├── AGENTS.md                   # AI 开发规范与项目知识库
└── README.md                   # 本文件
```

## 文件说明

- **`index.html`** — 全部内容在此一个文件中，CSS 与 JS 均为内嵌，不依赖任何外部资源。
- **`assets/tailorfit-experience-qr.png`** — 体验版小程序二维码，本地副本保证离线可用。
- **`docs/superpowers/`** — 开发过程文档，非运行时依赖。
- **`AGENTS.md`** — 供 AI 参与开发时阅读，包含硬规则、设计决策和知识沉淀。

## 当前限制

- 体验版二维码有效期至 **8 月 16 日**，到期后需更新 `assets/` 中的图片并同步页面文案中的有效期说明。
- 页面文案需随小程序功能迭代手动同步，无自动生成机制。
- 无分析/埋点，无法追踪页面访问数据。
- 二维码图片需保持本地路径，不可外链。

## 部署

直接将 `index.html` 和 `assets/` 目录部署到任意静态托管服务即可：

- GitHub Pages
- Vercel
- Netlify
- 任意 CDN 或静态服务器

无需构建步骤，无需配置后端。
