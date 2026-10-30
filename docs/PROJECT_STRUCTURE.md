# Motion Primitives Website - 项目结构分析文档

## 📋 项目概览

**项目名称**: motion-primitives-website  
**版本**: 2.0.0  
**描述**: 110+ 免费动画 React 组件库，专为 Next.js 设计  
**作者**: itsjwill  
**许可证**: MIT  
**官网**: https://motion-primitives.dev

### 核心特性

- ✨ **110+ 动画组件** - 覆盖背景、卡片、按钮、3D效果、滚动动画等
- 🎨 **4种设计方向** - Luxury、Cyberpunk、Kinetic、Freestyle
- ⚡ **多动画引擎支持** - Framer Motion + GSAP + Three.js + Lenis
- 🎯 **开箱即用** - 复制粘贴即可使用，零配置
- 🌙 **深色模式** - 完整的明暗主题切换
- 📱 **响应式设计** - 移动端友好

---

## 🏗️ 技术栈

### 核心框架
- **Next.js 16** (v16.1.6) - React 全栈框架
- **React 19** (v19.2.4) - UI 库
- **TypeScript 5** - 类型安全

### 动画与交互
- **Framer Motion 11** (v11.0.0) - 声明式动画库
- **GSAP 3** (v3.12.5) - 高性能动画平台
- **Three.js 0.164** (v0.164.0) - 3D 渲染引擎
- **@react-three/fiber** (v8.16.0) - React Three.js 渲染器
- **@react-three/drei** (v9.105.0) - Three.js 有用工具集
- **Lenis** (v1.1.0) - 平滑滚动库
- **@splinetool/react-spline** (v4.1.0) - Spline 3D 集成

### 样式与UI
- **Tailwind CSS 3** (v3.4.0) - 原子化 CSS 框架
- **class-variance-authority** (v0.7.0) - 变体样式管理
- **clsx** (v2.1.0) - 条件类名工具
- **tailwind-merge** (v2.3.0) - Tailwind 类名合并

### SEO与工具
- **next-sitemap** (v4.2.3) - 站点地图生成
- **schema-dts** (v1.1.2) - 结构化数据类型
- **mini-svg-data-uri** (v1.4.4) - SVG 转 Data URI
- **@next/bundle-analyzer** (v16.1.6) - 打包分析工具

---

## 📁 项目目录结构

```
motion-primitives-website/
├── .github/                      # GitHub 配置
│   ├── ISSUE_TEMPLATE/           # Issue 模板
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   └── workflows/
│       └── ci.yml                # CI/CD 配置
├── .next/                        # Next.js 构建输出（自动生成）
├── src/                          # 源代码根目录
│   ├── app/                      # Next.js App Router 页面
│   ├── components/              # 组件库（核心）
│   ├── hooks/                   # 自定义 Hooks
│   ├── lib/                     # 工具函数和配置
│   └── types/                   # TypeScript 类型定义
├── docs/                         # 项目文档
├── public/                       # 静态资源
├── .eslintrc.json               # ESLint 配置
├── .gitignore                   # Git 忽略规则
├── next.config.js               # Next.js 配置
├── next-sitemap.config.js       # 站点地图配置
├── package.json                 # 项目依赖
├── pnpm-lock.yaml              # 锁定依赖版本
├── postcss.config.js            # PostCSS 配置
├── tailwind.config.ts           # Tailwind CSS 配置
└── tsconfig.json                # TypeScript 配置
```

---

## 📄 源代码详细结构

### 1. `src/app/` - 应用路由层

Next.js App Router 的页面和布局定义。

#### 根布局
- [layout.tsx](../src/app/layout.tsx) - 应用根布局
  - 配置元数据（SEO、OpenGraph、Twitter Card）
  - 设置字体变量
  - 初始化主题系统（ThemeProvider）
  - 集成平滑滚动（SmoothScroll）
  - 方向预加载器（DirectionPreloader）

#### 主页
- [page.tsx](../src/app/page.tsx) - 首页
  - 组合多个 section 组件
  - 包含：Hero、Statement、InteractiveShowcase、ButtonShowcase、Carousel、DirectionShowcase、CodePreview、CTA

#### 页面 Sections (`_sections/`)
私有页面片段，用于构建完整页面：

| 文件 | 描述 |
|------|------|
| [hero.tsx](../src/app/_sections/hero.tsx) | 英雄区域 - 首屏展示 |
| [hero-text.tsx](../src/app/_sections/hero-text.tsx) | 文字英雄区 |
| [statement.tsx](../src/app/_sections/statement.tsx) | 品牌声明 |
| [button-showcase.tsx](../src/app/_sections/button-showcase.tsx) | 按钮展示区 |
| [carousel.tsx](../src/app/_sections/carousel.tsx) | 轮播展示 |
| [direction-showcase.tsx](../src/app/_sections/direction-showcase.tsx) | 4种设计方向展示 |
| [code-preview.tsx](../src/app/_sections/code-preview.tsx) | 代码预览区 |
| [cta.tsx](../src/app/_sections/cta.tsx) | 行动召唤区 |
| [interactive-showcase.tsx](../src/app/_sections/interactive-showcase.tsx) | 交互组件展示 |

#### Demo 页面 (`demos/`)
完整的演示页面，展示不同场景：

| 路径 | 描述 |
|------|------|
| [/demos](../src/app/demos/page.tsx) | Demos 总览 |
| [/demos/agency](../src/app/demos/agency/page.tsx) | 代理公司模板 |
| [/demos/portfolio](../src/app/demos/portfolio/page.tsx) | 作品集模板 |
| [/demos/saas](../src/app/demos/saas/page.tsx) | SaaS 产品模板 |
| [/demos/scroll](../src/app/demos/scroll/page.tsx) | 滚动动画演示 |
| [/demos/interactive-3d](../src/app/demos/interactive-3d/page.tsx) | 3D 交互演示 |
| [/demos/spline-worlds](../src/app/demos/spline-worlds/page.tsx) | Spline 3D 世界 |

#### 其他页面
- [/showcase](../src/app/showcase/page.tsx) - 组件展示页
- [/neural](../src/app/neural/page.tsx) - 神经网络特效页
- [/components](../src/app/components/page.tsx) - 组件文档总览
- [/components/[category]/[slug]](../src/app/components/[category]/[slug]/page.tsx) - 单个组件详情页

#### API 路由
- [api/og/route.tsx](../src/app/api/og/route.tsx) - 动态 OG 图片生成

---

### 2. `src/components/` - 组件库（核心）

这是项目的**核心资产**，包含所有可复用的动画组件。

#### 📊 组件统计

| 分类 | 组件数量 | 描述 |
|------|---------|------|
| **backgrounds** | 10 | 背景效果 |
| **buttons** | 1 | 高级按钮 |
| **cards** | 7 | 卡片组件 |
| **chat** | 1 | 聊天界面 |
| **core** | 4 | 核心基础组件 |
| **docs** | 3 | 文档展示组件 |
| **effects** | 12 | 视觉特效 |
| **interactive** | 26 | 交互组件 |
| **layout** | 4 | 布局组件 |
| **navigation** | 2 | 导航组件 |
| **scroll** | 7 | 滚动动画 |
| **seo** | 12 | SEO 优化组件 |
| **svg** | 1 | SVG 动画 |
| **templates** | 3 | 页面模板 |
| **text** | 5 | 文字动画 |
| **three** | 13 | 3D 效果 |
| **transitions** | 3 | 过渡动画 |
| **ui** | 3 | 基础 UI |
| **总计** | **115** | - |

---

## 🔧 详细组件说明

### 2.1 Backgrounds（背景效果）- 10个组件

路径: `src/components/backgrounds/`

| 组件文件 | 组件名 | 描述 |
|---------|--------|------|
| [aurora.tsx](../src/components/backgrounds/aurora.tsx) | `AuroraBackground` | 极光背景效果，使用渐变和模糊创造流动的极光视觉效果 |
| [gradient-blur.tsx](../src/components/backgrounds/gradient-blur.tsx) | - | 渐变模糊背景 |
| [gradient-mesh.tsx](../src/components/backgrounds/gradient-mesh.tsx) | - | 网格渐变背景 |
| [grid.tsx](../src/components/backgrounds/grid.tsx) | - | 网格线背景 |
| [infinite-grid.tsx](../src/components/backgrounds/infinite-grid.tsx) | - | 无限网格背景 |
| [meteors.tsx](../src/components/backgrounds/meteors.tsx) | - | 流星雨效果背景 |
| [particles.tsx](../src/components/backgrounds/particles.tsx) | - | 粒子系统背景 |
| [premium-backgrounds.tsx](../src/components/backgrounds/premium-backgrounds.tsx) | - | 高级背景集合 |
| [shader-backgrounds.tsx](../src/components/backgrounds/shader-backgrounds.tsx) | - | 着色器背景（WebGL） |
| [spotlight.tsx](../src/components/backgrounds/spotlight.tsx) | - | 聚光灯效果背景 |

**使用示例**:
```tsx
import { AuroraBackground } from '@/components/backgrounds';

<AuroraBackground showRadialGradient>
  <h1>内容</h1>
</AuroraBackground>
```

---

### 2.2 Buttons（按钮）- 1个组件

路径: `src/components/buttons/`

| 组件文件 | 描述 |
|---------|------|
| [premium-buttons.tsx](../src/components/buttons/premium-buttons.tsx) | 高级按钮集合，包含多种动画按钮变体 |

---

### 2.3 Cards（卡片）- 7个组件

路径: `src/components/cards/`

| 组件文件 | 组件名 | 描述 |
|---------|--------|------|
| [bento-grid.tsx](../src/components/cards/bento-grid.tsx) | `BentoGrid` | 便当盒网格布局，类似苹果风格的不规则网格 |
| [feature-showcase.tsx](../src/components/cards/feature-showcase.tsx) | - | 功能展示卡片 |
| [glow-card.tsx](../src/components/cards/glow-card.tsx) | `GlowCard` | 发光卡片，悬停时产生光晕效果 |
| [hover-card.tsx](../src/components/cards/hover-card.tsx) | - | 悬停效果卡片 |
| [living-system.tsx](../src/components/cards/living-system.tsx) | - | 动态生命系统卡片 |
| [stack-cards.tsx](../src/components/cards/stack-cards.tsx) | - | 堆叠卡片效果 |
| [tilt-card.tsx](../src/components/cards/tilt-card.tsx) | `TiltCard` | 3D 倾斜卡片，跟随鼠标倾斜 |

---

### 2.4 Chat（聊天）- 1个组件

路径: `src/components/chat/`

| 组件文件 | 描述 |
|---------|------|
| [animated-chat.tsx](../src/components/chat/animated-chat.tsx) | 动画聊天界面，消息气泡带入场动画 |

---

### 2.5 Core（核心组件）- 4个组件

路径: `src/components/core/`

这些是应用的基础设施组件：

| 组件文件 | 组件名 | 描述 |
|---------|--------|------|
| [smooth-scroll.tsx](../src/components/core/smooth-scroll.tsx) | `SmoothScroll` | 基于 Lenis 的全局平滑滚动 |
| [site-nav.tsx](../src/components/core/site-nav.tsx) | `SiteNav` | 站点导航栏 |
| [direction-picker.tsx](../src/components/core/direction-picker.tsx) | `DirectionPicker` | 设计方向选择器（Luxury/Cyberpunk/Kinetic/Freestyle） |
| [direction-preloader.tsx](../src/components/core/direction-preloader.tsx) | `DirectionPreloader` | 方向切换时的预加载过渡 |

**重要性**: 这些组件在 [layout.tsx](../src/app/layout.tsx) 中被初始化，是整个应用运行的基础。

---

### 2.6 Docs（文档组件）- 3个组件

路径: `src/components/docs/`

用于组件文档展示的工具组件：

| 组件文件 | 描述 |
|---------|------|
| [code-block.tsx](../src/components/docs/code-block.tsx) | 代码块展示组件，带语法高亮 |
| [component-preview.tsx](../src/components/docs/component-preview.tsx) | 组件预览容器，提供实时预览功能 |
| [props-table.tsx](../src/components/docs/props-table.tsx) | 属性表格组件，自动生成组件 API 文档 |

---

### 2.7 Effects（视觉特效）- 12个组件

路径: `src/components/effects/`

| 组件文件 | 描述 |
|---------|------|
| [animated-input.tsx](../src/components/effects/animated-input.tsx) | 动画输入框，聚焦时带特效 |
| [audio-reactive.tsx](../src/components/effects/audio-reactive.tsx) | 音频 reactive 特效，响应音频输入 |
| [custom-cursor.tsx](../src/components/effects/custom-cursor.tsx) | 自定义光标效果 |
| [fluid-cursor.tsx](../src/components/effects/fluid-cursor.tsx) | 流体光标，跟随鼠标的液体效果 |
| [magnetic-button.tsx](../src/components/effects/magnetic-button.tsx) | 磁性按钮，吸引鼠标靠近 |
| [magnetic-gallery.tsx](../src/components/effects/magnetic-gallery.tsx) | 磁性图片画廊 |
| [media-reveal.tsx](../src/components/effects/media-reveal.tsx) | 媒体揭示动画 |
| [mouse-parallax.tsx](../src/components/effects/mouse-parallax.tsx) | 鼠标视差效果 |
| [premium-effects.tsx](../src/components/effects/premium-effects.tsx) | 高级特效集合 |
| [reveal-on-hover.tsx](../src/components/effects/reveal-on-hover.tsx) | 悬停揭示效果 |
| [text-distortion.tsx](../src/components/effects/text-distortion.tsx) | 文字扭曲效果 |
| [theme-toggle.tsx](../src/components/effects/theme-toggle.tsx) | 主题切换按钮（明暗模式） |

---

### 2.8 Interactive（交互组件）- 26个组件 ⭐

路径: `src/components/interactive/`

这是**最大的组件分类**，包含丰富的交互模式：

| 组件文件 | 组件名 | 描述 |
|---------|--------|------|
| [accordion.tsx](../src/components/interactive/accordion.tsx) | `Accordion` | 手风琴折叠面板 |
| [animated-beam.tsx](../src/components/interactive/animated-beam.tsx) | - | 动画光束连接效果 |
| [animated-counter.tsx](../src/components/interactive/animated-counter.tsx) | `AnimatedCounter` | 数字动画计数器 |
| [animated-tabs.tsx](../src/components/interactive/animated-tabs.tsx) | - | 动画标签页 |
| [border-beam.tsx](../src/components/interactive/border-beam.tsx) | - | 边框光束扫描效果 |
| [command-palette.tsx](../src/components/interactive/command-palette.tsx) | - | 命令面板（Cmd+K） |
| [confetti.tsx](../src/components/interactive/confetti.tsx) | - | 五彩纸屑庆祝效果 |
| [dock.tsx](../src/components/interactive/dock.tsx) | `Dock` | macOS 风格 Dock 栏，带放大效果 |
| [drawer.tsx](../src/components/interactive/drawer.tsx) | - | 抽屉组件 |
| [lamp.tsx](../src/components/interactive/lamp.tsx) | - | 灯光聚拢效果 |
| [liquid-glass.tsx](../src/components/interactive/liquid-glass.tsx) | - | 液态玻璃效果 |
| [magnetic.tsx](../src/components/interactive/magnetic.tsx) | - | 磁性元素 |
| [marquee.tsx](../src/components/interactive/marquee.tsx) | - | 无限滚动跑马灯 |
| [modal.tsx](../src/components/interactive/modal.tsx) | - | 模态对话框 |
| [morphing-text.tsx](../src/components/interactive/morphing-text.tsx) | - | 文字变形动画 |
| [number-ticker.tsx](../src/components/interactive/number-ticker.tsx) | - | 数字滚动器 |
| [orbit.tsx](../src/components/interactive/orbit.tsx) | - | 轨道环绕动画 |
| [progress.tsx](../src/components/interactive/progress.tsx) | - | 进度条 |
| [ripple.tsx](../src/components/interactive/ripple.tsx) | - | 涟漪点击效果 |
| [scroll-reveal.tsx](../src/components/interactive/scroll-reveal.tsx) | - | 滚动揭示 |
| [skeleton.tsx](../src/components/interactive/skeleton.tsx) | - | 骨架屏加载 |
| [sortable-list.tsx](../src/components/interactive/sortable-list.tsx) | - | 可排序列表 |
| [spotlight-card.tsx](../src/components/interactive/spotlight-card.tsx) | `SpotlightCard` | 聚光灯卡片，鼠标跟随高光 |
| [swipe-cards.tsx](../src/components/interactive/swipe-cards.tsx) | - | 滑动卡片 |
| [switch.tsx](../src/components/interactive/switch.tsx) | - | 开关切换 |
| [toast.tsx](../src/components/interactive/toast.tsx) | - | 轻提示通知 |
| [tooltip.tsx](../src/components/interactive/tooltip.tsx) | - | 工具提示 |
| [wavy-background.tsx](../src/components/interactive/wavy-background.tsx) | - | 波浪背景 |

**明星组件 - Dock 示例**:
```tsx
import { Dock, DockItem } from '@/components/interactive';

<Dock magnification={60} distance={140} direction="bottom">
  <DockItem label="Home">
    <IconHome />
  </DockItem>
  <DockItem label="Settings">
    <IconSettings />
  </DockItem>
</Dock>
```

---

### 2.9 Layout（布局组件）- 4个组件

路径: `src/components/layout/`

| 组件文件 | 描述 |
|---------|------|
| [animated-masonry.tsx](../src/components/layout/animated-masonry.tsx) | 动画瀑布流布局 |
| [hero-sections.tsx](../src/components/layout/hero-sections.tsx) | 英雄区域布局变体 |
| [premium-layouts.tsx](../src/components/layout/premium-layouts.tsx) | 高级布局集合 |
| [sections.tsx](../src/components/layout/sections.tsx) | 区块布局组件 |

---

### 2.10 Navigation（导航）- 2个组件

路径: `src/components/navigation/`

| 组件文件 | 描述 |
|---------|------|
| [index.tsx](../src/components/navigation/index.tsx) | 导航导出 |
| [morphing-nav.tsx](../src/components/navigation/morphing-nav.tsx) | `MorphingNav` | 形变导航栏，动态形状变化 |

---

### 2.11 Scroll（滚动动画）- 7个组件

路径: `src/components/scroll/`

| 组件文件 | 描述 |
|---------|------|
| [gsap-scroll.tsx](../src/components/scroll/gsap-scroll.tsx) | GSAP 滚动触发动画 |
| [infinite-scroll.tsx](../src/components/scroll/infinite-scroll.tsx) | 无限滚动加载 |
| [parallax-scroll.tsx](../src/components/scroll/parallax-scroll.tsx) | 视差滚动效果 |
| [scroll-animations.tsx](../src/components/scroll/scroll-animations.tsx) | 滚动动画集合 |
| [scroll-orchestrator.tsx](../src/components/scroll/scroll-orchestrator.tsx) | 滚动动画编排器 |
| [scroll-video.tsx](../src/components/scroll/scroll-video.tsx) | 滚动驱动视频播放 |
| [section-reveal.tsx](../src/components/scroll/section-reveal.tsx) | 区块滚动揭示 |

---

### 2.12 SEO（搜索引擎优化）- 12个组件

路径: `src/components/seo/`

完整的 SEO 工具包：

| 组件文件 | 描述 |
|---------|------|
| [analytics.tsx](../src/components/seo/analytics.tsx) | 分析代码集成（Google Analytics 等） |
| [auto-seo.tsx](../src/components/seo/auto-seo.tsx) | 自动 SEO 优化 |
| [breadcrumbs.tsx](../src/components/seo/breadcrumbs.tsx) | 面包屑导航 |
| [canonical.tsx](../src/components/seo/canonical.tsx) | 规范链接 |
| [json-ld.tsx](../src/components/seo/json-ld.tsx) | JSON-LD 结构化数据 |
| [rss-feed.tsx](src/components/seo/rss-feed.tsx) | RSS 订阅源 |
| [seo-analyzer.tsx](../src/components/seo/seo-analyzer.tsx) | SEO 分析器 |
| [seo-head.tsx](../src/components/seo/seo-head.tsx) | SEO 头部元数据 |
| [seo-setup.tsx](../src/components/seo/seo-setup.tsx) | SEO 初始化设置 |
| [social-share.tsx](../src/components/seo/social-share.tsx) | 社交分享元标签 |
| [web-vitals.tsx](../src/components/seo/web-vitals.tsx) | Web Vitals 性能监控 |

---

### 2.13 SVG（SVG 动画）- 1个组件

路径: `src/components/svg/`

| 组件文件 | 描述 |
|---------|------|
| [animated-svg.tsx](../src/components/svg/animated-svg.tsx) | SVG 路径动画组件 |

---

### 2.14 Templates（页面模板）- 3个组件

路径: `src/components/templates/`

即用型页面模板：

| 组件文件 | 描述 |
|---------|------|
| [about-template.tsx](../src/components/templates/about-template.tsx) | 关于我们页面模板 |
| [landing-template.tsx](../src/components/templates/landing-template.tsx) | 落地页模板 |
| [pricing-template.tsx](../src/components/templates/pricing-template.tsx) | 定价页面模板 |

---

### 2.15 Text（文字动画）- 5个组件

路径: `src/components/text/`

| 组件文件 | 描述 |
|---------|------|
| [gradient-text.tsx](../src/components/text/gradient-text.tsx) | 渐变文字效果 |
| [premium-text.tsx](../src/components/text/premium-text.tsx) | 高级文字动画集合 |
| [split-screen-text.tsx](../src/components/text/split-screen-text.tsx) | 分屏文字效果 |
| [text-generate.tsx](../src/components/text/text-generate.tsx) | 打字机效果 |
| [text-reveal.tsx](../src/components/text/text-reveal.tsx) | 文字揭示动画 |

---

### 2.16 Three（3D 效果）- 13个组件

路径: `src/components/three/`

基于 Three.js 和 @react-three/fiber 的 3D 组件：

#### 核心 3D 组件
| 组件文件 | 描述 |
|---------|------|
| [globe.tsx](../src/components/three/globe.tsx) | `Globe` | 3D 地球仪（线框/点阵模式） |
| [floating-shapes.tsx](../src/components/three/floating-shapes.tsx) | - | 悬浮几何体 |
| [particle-morph.tsx](../src/components/three/particle-morph.tsx) | - | 粒子变形动画 |
| [neural-network.tsx](../src/components/three/neural-network.tsx) | - | 神经网络可视化 |
| [scroll-progress-3d.tsx](../src/components/three/scroll-progress-3d.tsx) | - | 3D 滚动进度指示器 |

#### 3D 机器人（`robots/` 子目录）
| 组件文件 | 描述 |
|---------|------|
| [bubble-bot.tsx](../src/components/three/robots/bubble-bot.tsx) | 泡泡机器人 |
| [chrome-titan.tsx](../src/components/three/robots/chrome-titan.tsx) | 铬钛机器人 |
| [neon-samurai.tsx](../src/components/three/robots/neon-samurai.tsx) | 霓虹武士 |

#### 实验性 3D 效果（`experimental/` 子目录）
| 组件文件 | 描述 |
|---------|------|
| [dimensional-rift.tsx](../src/components/three/experimental/dimensional-rift.tsx) | 维度裂缝效果 |
| [gravity-well.tsx](../src/components/three/experimental/gravity-well.tsx) | 引力井模拟 |
| [living-trail.tsx](../src/components/three/experimental/living-trail.tsx) | 生命轨迹 |
| [magnetic-fluid.tsx](../src/components/three/experimental/magnetic-fluid.tsx) | 磁性流体 |
| [sound-fabric.tsx](../src/components/three/experimental/sound-fabric.tsx) | 声音织物 |
| [swarm-intelligence.tsx](../src/components/three/experimental/swarm-intelligence.tsx) | 群体智能模拟 |

**Globe 组件示例**:
```tsx
import { Globe } from '@/components/three';

<Globe
  color="#8B5CF6"
  rotationSpeed={0.001}
  radius={2}
  variant="dotted"  // "wireframe" | "dotted"
/>
```

---

### 2.17 Transitions（过渡动画）- 3个组件

路径: `src/components/transitions/`

| 组件文件 | 描述 |
|---------|------|
| [preloader.tsx](../src/components/transitions/preloader.tsx) | 页面预加载动画 |
| [page-transition.tsx](../src/components/transitions/page-transition.tsx) | 页面切换过渡效果 |
| [noise-transition.tsx](../src/components/transitions/noise-transition.tsx) | 噪声过渡效果 |

---

### 2.18 UI（基础 UI）- 3个组件

路径: `src/components/ui/`

| 组件文件 | 描述 |
|---------|------|
| [card.tsx](../src/components/ui/card.tsx) | 基础卡片组件 |
| [spline-recolor.tsx](../src/components/ui/spline-recolor.tsx) | Spline 重新着色 |
| [splite.tsx](../src/components/ui/splite.tsx) | 分割组件 |

---

## 🪝 Hooks（自定义钩子）

路径: `src/hooks/`

| Hook 文件 | 描述 |
|-----------|------|
| [use-gsap.ts](../src/hooks/use-gsap.ts) | `useGsap` | GSAP 动画封装，提供便捷的 GSAP 使用方式 |
| [use-lenis.ts](../src/hooks/use-lenis.ts) | `useLenis` | Lenis 平滑滚动 Hook |
| [use-mouse-position.ts](../src/hooks/use-mouse-position.ts) | `useMousePosition` | 鼠标位置追踪 |
| [use-reduced-motion.ts](../src/hooks/use-reduced-motion.ts) | `useReducedMotion` | 检测用户是否偏好减少动画（无障碍） |
| [use-svg-animation.ts](../src/hooks/use-svg-animation.ts) | `useSvgAnimation` | SVG 路径动画控制 |
| [use-web-vitals.ts](../src/hooks/use-web-vitals.ts) | `useWebVitals` | Web Vitals 性能指标收集 |

**使用示例**:
```tsx
import { useMousePosition } from '@/hooks';

function Component() {
  const { x, y } = useMousePosition();
  return <div style={{ transform: `translate(${x}px, ${y}px)` }} />;
}
```

---

## 📚 Lib（工具库）

路径: `src/lib/`

### 核心工具
| 文件 | 描述 |
|------|------|
| [utils.ts](../src/lib/utils.ts) | `cn()` - 类名合并工具（clsx + tailwind-merge） |
| [theme.tsx](../src/lib/theme.tsx) | **主题系统核心** - ThemeProvider、4种设计方向管理 |
| [fonts.ts](../src/lib/fonts.ts) | 字体配置 - Google Fonts 加载（8种字体） |
| [motion.ts](../src/lib/motion.ts) | Framer Motion 动画预设和工具函数 |
| [reduced-motion.ts](../src/lib/reduced-motion.ts) | 减少动画偏好检测 |
| [seo.ts](../src/lib/seo.ts) | SEO 工具函数 |
| [auto-seo.ts](../src/lib/auto-seo.ts) | 自动 SEO 生成逻辑 |
| [component-registry.ts](../src/lib/component-registry.ts) | **组件注册表** - 所有组件的元数据索引（大文件，64KB+） |

### 主题系统详解 ([theme.tsx](../src/lib/theme.tsx))

项目实现了独特的**四方向设计系统**：

```typescript
type Direction = "luxury" | "cyberpunk" | "kinetic" | "freestyle";
type Mode = "light" | "dark";
```

#### 四种设计方向

| 方向 | 标签 | 描述 | 强调色 |
|------|------|------|--------|
| **luxury** | Luxury | 极简优雅，Apple 风格 | #C9A84C (金) |
| **cyberpunk** | Cyberpunk | 霓虹暗色界面，赛博朋克 | #00FFD1 (青) |
| **kinetic** | Kinetic | 一切都在动，Stripe 级动效 | #7C3AED (紫) |
| **freestyle** | Freestyle | 大胆创新，打破常规 | #FF6B35 (橙) |

每种方向都有独立的：
- 字体组合（heading + body）
- 颜色变量（CSS Custom Properties）
- 动画曲线（cubic-bezier）
- 组件样式变体

---

## 🎨 样式系统

### Tailwind CSS 配置 ([tailwind.config.ts](../tailwind.config.ts))

#### 颜色系统（CSS 变量驱动）
```typescript
colors: {
  border: "hsl(var(--border))",
  background: "hsl(var(--background))",
  foreground: "hsl(var(--foreground))",
  primary: { DEFAULT: "...", foreground: "..." },
  secondary: { DEFAULT: "...", foreground: "..." },
  muted: { DEFAULT: "...", foreground: "..." },
  accent: { DEFAULT: "...", foreground: "..." },
  glow: "hsl(var(--glow))",
}
```

#### 字体系统（方向感知）
```typescript
fontFamily: {
  heading: ["var(--font-heading)", "system-ui"],
  body: ["var(--font-body)", "system-ui"],
  // 各方向独立字体
  "luxury-heading": ["Space Grotesk"],
  "cyber-heading": ["JetBrains Mono"],
  "kinetic-heading": ["Outfit"],
  "free-heading": ["Syne"],
  // ... body fonts
}
```

#### 排版尺度（Fluid Typography）
```typescript
fontSize: {
  "display": ["clamp(3rem, 8vw, 7.5rem)", { lineHeight: "1.05" }],
  "display-sm": ["clamp(2.5rem, 6vw, 5rem)"],
  "heading-1": ["clamp(2rem, 4vw, 4rem)"],
  "heading-2": ["clamp(1.5rem, 3vw, 2.5rem)"],
  "heading-3": ["clamp(1.25rem, 2vw, 1.75rem)"],
  "body-lg": ["1.125rem"],
  "body-sm": ["0.875rem"],
  "caption": ["0.75rem"],
}
```

使用 `clamp()` 实现响应式字体，无需媒体查询。

#### 动画预设（30+ 种）
内置丰富的 CSS 动画：

| 动画名 | 用途 | 时长 |
|--------|------|------|
| `spotlight` | 聚光灯效果 | 2s |
| `shimmer` | 微光扫过 | 2s infinite |
| `meteor` | 流星 | 5s infinite |
| `aurora` | 极光 | 60s infinite |
| `marquee` | 跑马灯 | 自定义 |
| `border-beam` | 边框光束 | 自定义 |
| `float` | 悬浮 | 6s infinite |
| `pulse-glow` | 脉冲发光 | 2s infinite |
| `text-reveal` | 文字揭示 | 1.5s |
| `fade-up/down` | 淡入上/下 | 0.5s |
| `scale-in` | 缩放进入 | 0.5s |
| `slide-in-left/right` | 滑入 | 0.5s |
| `glow-pulse` | 光晕脉冲 | 3s |
| `bounce-subtle` | 微弹跳 | 2s |

#### 方向变体（Tailwind Variants）
```typescript
// 在 tailwind.config.ts 中自定义
addVariant("luxury", '[data-direction="luxury"] &');
addVariant("cyberpunk", '[data-direction="cyberpunk"] &');
addVariant("kinetic", '[data-direction="kinetic"] &');
addVariant("freestyle", '[data-direction="freestyle"] &');
```

使用方式：
```html
<div class="luxury:text-gold cyberpunk:text-cyan">
  <!-- 根据当前方向自动切换颜色 -->
</div>
```

#### 背景图案工具
通过插件生成的实用类：
- `bg-grid-[color]` - 网格背景
- `bg-grid-small-[color]` - 小网格背景
- `bg-dot-[color]` - 点状背景

---

## 🔄 数据流与架构

### 组件层级关系

```
RootLayout (layout.tsx)
├── ThemeProvider (主题上下文)
│   ├── DirectionPreloader (方向切换过渡)
│   │   └── {children} (页面内容)
│   └── SmoothScroll (全局平滑滚动)
└── 页面结构
    ├── SiteNav (导航)
    ├── HeroSection (英雄区)
    ├── StatementSection (声明)
    ├── InteractiveShowcase (交互展示)
    ├── ButtonShowcase (按钮展示)
    ├── CarouselSection (轮播)
    ├── DirectionShowcase (方向展示)
    ├── CodePreviewSection (代码预览)
    └── CTASection (行动召唤)
```

### 状态管理

1. **Theme Context** ([theme.tsx](../src/lib/theme.tsx))
   - 管理 `direction`（设计方向）和 `mode`（明暗模式）
   - 持久化到 localStorage
   - 通过 `data-direction` 属性应用到 `<html>` 元素

2. **局部状态**
   - 大多数组件使用内部 `useState` 管理自身状态
   - 复杂交互使用 Framer Motion 的 `useMotionValue`

### 动画策略

| 场景 | 推荐工具 | 示例组件 |
|------|---------|---------|
| UI 微交互 | Framer Motion | Dock, SpotlightCard, Accordion |
| 滚动驱动动画 | GSAP ScrollTrigger | GSAPScroll, ParallaxScroll |
| 3D 效果 | Three.js + R3F | Globe, NeuralNetwork, Robots |
| 平滑滚动 | Lenis | SmoothScroll (全局) |
| CSS 动画 | Tailwind + CSS Keyframes | Aurora, Meteors, Marquee |
| 复杂时间轴 | GSAP Timeline | PageTransition, Preloader |

---

## 📦 关于 Blocks 文件

**重要发现**: 本项目**没有传统的 blocks 目录或文件**。

### 为什么没有 Blocks？

这是一个**组件库网站**，而非基于 blocks 架构的内容管理系统（如 WordPress 或某些 Headless CMS）。其架构特点：

1. **组件化架构**：所有可复用的部分都是独立组件（115个），位于 `src/components/`
2. **Section 组合**：页面通过组合 `_sections/` 下的区块构建
3. **模板系统**：提供完整的页面模板（Landing, About, Pricing）
4. **Demo 展示**：通过 `/demos/` 路由展示不同使用场景

### 类似 Blocks 的概念

如果寻找类似 "blocks" 的概念，可以参考：

| 概念 | 位置 | 描述 |
|------|------|------|
| **Sections** | `src/app/_sections/` | 页面区块（9个） |
| **Templates** | `src/components/templates/` | 完整页面模板（3个） |
| **Layout Components** | `src/components/layout/` | 布局组件（4个） |
| **Demos** | `src/app/demos/` | 场景演示（6个） |

这种设计比传统 blocks 更灵活，因为：
- 组件可以在任何地方复用
- 不受固定 schema 限制
- 支持嵌套和组合
- 类型安全（TypeScript）

---

## 🚀 快速开始

### 安装依赖
```bash
pnpm install
# 或
npm install
```

### 开发模式
```bash
pnpm dev
# 访问 http://localhost:3000
```

### 生产构建
```bash
pnpm build
pnpm start
```

### 代码检查
```bash
pnpm lint
```

### 打包分析
```bash
pnpm analyze
# 会在浏览器中打开 bundle 分析器
```

---

## 📖 使用组件示例

### 基础用法
```tsx
import { Dock, DockItem } from '@/components/interactive';
import { AuroraBackground } from '@/components/backgrounds';
import { GlowCard } from '@/components/cards';
import { Globe } from '@/components/three';

function MyPage() {
  return (
    <AuroraBackground>
      <Globe color="#8B5CF6" radius={2.5} />
      <GlowCard>
        <h3>标题</h3>
        <p>内容</p>
      </GlowCard>
      <Dock>
        <DockItem label="Home"><IconHome /></DockItem>
        <DockItem label="About"><IconUser /></DockItem>
      </Dock>
    </AuroraBackground>
  );
}
```

### 使用 Hooks
```tsx
import { useMousePosition, useReducedMotion } from '@/hooks';

function MagneticComponent() {
  const { x, y } = useMousePosition();
  const shouldReduceMotion = useReducedMotion();

  return (
    <div
      style={{
        transform: shouldReduceMotion ? 'none' : `translate(${x * 0.1}px, ${y * 0.1}px)`
      }}
    >
      跟随鼠标的元素
    </div>
  );
}
```

---

## 🎯 设计原则

### 1. **复制粘贴优先**
所有组件都设计为可以直接复制到任何 Next.js 项目中使用。

### 2. **性能优先**
- 使用 `will-change` 优化动画性能
- 合理使用 `transform` 和 `opacity`（GPU 加速）
- Three.js 组件使用 `useMemo` 优化计算
- 支持 `prefers-reduced-motion` 无障碍

### 3. **类型安全**
- 完整的 TypeScript 类型定义
- 清晰的 Props 接口
- 组件注册表提供元数据

### 4. **方向感知**
- 4种设计方向，一键切换
- CSS 变量驱动，运行时可变
- Tailwind variants 简化条件样式

### 5. **渐进增强**
- 基础功能纯 CSS/Tailwind
- 高级功能可选引入（Three.js, GSAP）
- 优雅降级（减少动画模式）

---

## 📊 项目统计

| 指标 | 数值 |
|------|------|
| **总组件数** | 115 个 |
| **组件分类** | 18 类 |
| **自定义 Hooks** | 6 个 |
| **工具函数** | 8 个 |
| **页面 Sections** | 9 个 |
| **Demo 页面** | 6 个 |
| **页面模板** | 3 个 |
| **设计方向** | 4 种 |
| **动画引擎** | 4 个（Framer Motion, GSAP, Three.js, CSS） |
| **代码行数（估算）** | 15,000+ 行 |
| **依赖包数量** | 20 个（生产）+ 8 个（开发） |

---

## 🔗 相关资源

- **GitHub 仓库**: https://github.com/itsjwill/motion-primitives-website
- **在线演示**: https://motion-primitives.dev
- **问题反馈**: https://github.com/itsjwill/motion-primitives-website/issues
- **作者 Twitter**: [@itsjwill](https://x.com/itsjwill)

---

## 📝 文档更新日志

- **2026-10-22**: 初始版本，完整项目结构分析
  - 分析了所有 18 个组件分类
  - 详细记录了 115 个组件
  - 阐述了 4 方向设计系统
  - 解释了架构决策和数据流

---

*本文档由项目结构自动分析生成*