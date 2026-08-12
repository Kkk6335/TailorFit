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
- Liquid Glass 液态玻璃导航材质（顶栏 / 导航胶囊 / CTA 按钮：WebGL 折射 + CSS 多层玻璃，自动明暗适配与按压形变）
- WCAG 2.2 AA 可访问性：焦点状态可见、图片有 alt 文本、语义化 HTML

## 技术栈

| 层       | 技术                                         |
| -------- | -------------------------------------------- |
| 前端     | HTML5 + CSS3（自定义属性 / Grid / Flexbox）  |
| 脚本     | Vanilla JavaScript（IntersectionObserver、WebGL 折射 shader） |
| 后端     | 无                                           |
| 构建     | 无                                           |
| 部署     | 任意静态托管服务或直接双击打开               |

## 视觉风格

暖象牙纸色背景（`#f4f0e8`）+ 近黑石板色（`#2b2b28`）+ 陶土橙强调（`#bd5b3e`）基础设计语言。导航控件层（顶栏、导航胶囊、CTA 按钮）采用 Liquid Glass 液态玻璃材质：WebGL 折射 shader + CSS 多层玻璃（环境 tint、Fresnel 边缘光、双层克制阴影、按压液态形变、滚动明暗自适应）；内容层保持干净底色。无 WebGL 或快照失败时自动回退为 CSS 多层玻璃。

## 设计系统

### 颜色

| 角色 | Token | 色值 | 用途 |
| --- | --- | --- | --- |
| 背景 | `--paper` | #f4f0e8 | 页面主背景 |
| 深色背景 | `--slate` | #2b2b28 | 功能/安全/CTA 区块 |
| 正文 | `--ink` | #171716 | 主要文字 |
| 次要文字 | `--muted` | #66645f | 辅助说明 |
| 分隔线 | `--line` | #cfc8bb | 边框、分割线 |
| 强调 | `--clay` | #bd5b3e | CTA、编号、重点 |
| 深色区块文字 | `--white-ink` | #f8f5ee | 深色背景上的正文 |

### 字体

- 正文/UI：系统无衬线栈（`-apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC", "Microsoft YaHei", sans-serif`）
- 标题/品牌：Georgia 系衬线（`Georgia, "Songti SC", "STSong", serif`）
- 标签/编号：等宽字体（`Consolas, "Liberation Mono", monospace`）

### 间距与宽度

- 内容最大宽度：`1180px`（`--max-width`）
- 响应式断点：`767px`（移动端单列）

## 可访问性实现

遵循 WCAG 2.2 AA 标准：

- 所有交互元素有可见 `:focus-visible` 状态（2px 陶土橙外轮廓）
- 所有图片有描述性 `alt` 文本
- 语义化 HTML（`<header>`, `<main>`, `<section>`, `<nav>`, `<footer>`, `<article>`）
- 所有锚点链接有 `aria-label` 或可感知文本
- 尊重 `prefers-reduced-motion: reduce`：禁用滚动动画和过渡
- 关键内容不依赖 JavaScript 呈现（渐进增强）

## 关键代码模式

### 年份自动更新

```html
<span data-current-year>2026</span>
```
```js
document.querySelectorAll('[data-current-year]').forEach((node) => {
  node.textContent = new Date().getFullYear();
});
```

### 滚动入场动画（IntersectionObserver）

```css
.reveal { transition: opacity 500ms ease, transform 500ms ease; }
.js .reveal { opacity: 0; visibility: hidden; transform: translateY(16px); }
.js .reveal.is-visible { opacity: 1; visibility: visible; transform: translateY(0); }
```
- threshold: `0.12`（元素 12% 可见时触发）
- `prefers-reduced-motion: reduce` 时自动跳过动画

### JS 渐进增强检测

首行内嵌脚本添加 `.js` class，CSS 据此控制增强行为：
```html
<script>document.documentElement.classList.add('js');</script>
```

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
@('id="intro"','id="features"','id="flow"','id="safety"','id="audience"','id="experience"','assets/tailorfit-experience-qr.png','prefers-reduced-motion','aria-label','alt=','class="glass','LiquidGlass.init()','glass-render') | ForEach-Object { if ($html -notmatch [regex]::Escape($_)) { throw "Missing: $_" } }
if ($html -match '[Tt][Oo][Dd][Oo]|[Tt][Bb][Dd]|[Ll]orem ipsum') { throw 'Placeholder copy found' }
Write-Output 'Static checks passed'
```

## 仓库结构

```
.
├── index.html                  # 主页面（HTML + CSS + JS 内嵌，单文件）
├── assets/
│   └── tailorfit-experience-qr.png  # 体验版二维码图片
├── docs/
│   ├── superpowers/
│   │   ├── plans/              # 开发计划文档
│   │   └── specs/              # 设计规格文档
│   └── ai-log/                 # AI 开发日志（YYYY-MM-DD.md）
├── AGENTS.md                   # AI 开发规范与项目知识库
└── README.md                   # 本文件
```

## 文件说明

- **`index.html`** — 全部内容在此一个文件中，CSS 与 JS 均为内嵌，不依赖任何外部资源。
- **`assets/tailorfit-experience-qr.png`** — 体验版小程序二维码，本地副本保证离线可用。
- **`docs/superpowers/`** — 开发过程文档，非运行时依赖。
- **`docs/ai-log/`** — AI 开发日志，每次任务完成后写入。
- **`AGENTS.md`** — 供 AI 参与开发时阅读，包含硬规则、设计决策和知识沉淀。

## 当前限制

- 体验版二维码有效期至 **8 月 16 日**，到期后需更新 `assets/` 中的图片并同步页面文案中的有效期说明。
- 页面文案需随小程序功能迭代手动同步，无自动生成机制。
- 无分析/埋点，无法追踪页面访问数据。
- 二维码图片需保持本地路径，不可外链。
- WebGL 折射为渐进增强：无 WebGL、SVG 快照渲染失败或快照内容校验失败时自动回退为 CSS 多层玻璃材质（无折射位移）。

## 部署

直接将 `index.html` 和 `assets/` 目录部署到任意静态托管服务即可：

- GitHub Pages
- Vercel
- Netlify
- 任意 CDN 或静态服务器

无需构建步骤，无需配置后端。
