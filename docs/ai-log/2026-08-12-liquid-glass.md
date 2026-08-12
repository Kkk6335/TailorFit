# 2026-08-12 Liquid Glass 导航材质重构

## 变更概要

将顶部导航栏、导航胶囊链接、CTA 按钮从「硬边描边」风格重构为 Apple Liquid Glass（液态玻璃）材质，视觉上与 iOS 原生更接近。

## 实现方式

- **双层材质架构**：CSS 多层玻璃（环境 tint + 顶部内高光 + Fresnel 内圈 + 双层克制阴影 + 按压形变）保证基础材质，WebGL 折射 shader 作为渐进增强。
- **折射原理**：整页 SVG foreignObject 快照一次性渲染为 WebGL 纹理；每帧按「玻璃屏幕坐标 + 滚动偏移」采样，采样点沿圆角矩形 SDF 法线在边缘带（16px）内径向位移，模拟凸玻璃非均匀折射；按压时折射强度按 spring 增大约 1.4 倍。
- **性能关键决策**：快照只构建一次（fonts.ready / load / resize 重建），滚动仅做采样坐标偏移，无每帧截屏成本；绘制 quad 缩到玻璃控件包围盒（非全屏 shader），scissor 裁剪写入；静止无变化时跳过重绘。
- **明暗自适应**：按各 section 背景分带（浅色 paper / 深色 slate）滚动判定 `tone-dark`，切换 tint / 边缘光 / 文字色；`dFdx/dFdy` 求 SDF 梯度做边缘环境反射采样（邻居像素混合）。
- **回退路径**：无 WebGL、快照内容校验失败（探测 24x24 区域全 0 或无变化）时移除 canvas，仅保留 CSS 材质，页面功能不受影响。

## 采用的技术取舍

- 防护障碍：`getComputedStyle().borderTopLeftRadius` 每帧读取（玻璃元素仅 5 个，可忽略）。
- 移动端隐藏的导航链接 rect 为 0，绘制前跳过（`rect.width < 2`）。
- `prefers-reduced-motion` 下按压形变跳变（无 spring 动画）。

## 文档契约变化（重要）

- 原「禁用 box-shadow / 渐变」契约解除，改为「仅允许用于 `.glass` 材质层与实色玻璃按钮」——静态检查脚本同步更新。
- 新增 Known Pitfalls：`.glass` 的 `position: relative` 会覆盖 `.site-header` 的 `sticky`，已加 `.site-header.glass` 复权规则。
- 玻璃控件必须带 `.glass` 类才会被 WebGL 折射层覆盖，否则其后方为普通 DOM 内容（无折射）。

## 验证边界

- 已通过：内嵌 JS `node --check` 语法检查；静态结构脚本。
- 未验证：真实浏览器视觉/交互（需人工打开 `index.html` 确认折射、按压、明暗切换）。