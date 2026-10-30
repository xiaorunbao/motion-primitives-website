# Motion Primitives - 架构决策记录

> 记录重要的技术选型、架构设计决策及其原因，帮助团队理解 "为什么这样设计"

---

## 📋 目录

1. [架构概览](#-架构概览)
2. [技术栈选型](#-技术栈选型)
3. [目录结构决策](#-目录结构决策)
4. [设计系统架构](#-设计系统架构)
5. [组件设计原则](#-组件设计原则)
6. [状态管理策略](#-状态管理策略)
7. [性能架构](#-性能架构)
8. [关键决策记录 (ADR)](#-关键决策记录-adr)
9. [未来演进方向](#-未来演进方向)

---

## 🏗️ 架构概览

### 高层架构图

```
┌─────────────────────────────────────────────────────────────────┐
│                        用户浏览器                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                   Next.js App Router                     │   │
│  │  ┌─────────┐  ┌─────────────┐  ┌─────────────────────┐   │   │
│  │  │ Layout  │  │   Pages     │  │    API Routes       │   │   │
│  │  │         │  │             │  │  (/api/og, etc.)    │   │   │
│  │  └────┬────┘  └──────┬──────┘  └──────────┬──────────┘   │   │
│  │       │              │                      │            │   │
│  │  ┌────▼──────────────▼──────────────────────▼────────┐   │   │
│  │  │              React Component Tree                 │   │   │
│  │  │                                                   │   │   │
│  │  │  ┌─────────────────────────────────────────┐      │   │   │
│  │  │  │           ThemeProvider                 │      │   │   │
│  │  │  │  (Direction + Mode Context)             │      │   │   │
│  │  │  └─────────────────┬───────────────────────┘      │   │   │
│  │  │                    │                              │   │   │
│  │  │  ┌─────────────────▼───────────────────────┐      │   │   │
│  │  │  │          Core Components                │      │   │   │
│  │  │  │  SmoothScroll, SiteNav, Preloader       │      │   │   │
│  │  │  └─────────────────┬───────────────────────┘      │   │   │
│  │  │                    │                              │   │   │
│  │  │  ┌─────────────────▼───────────────────────┐      │   │   │
│  │  │  │        Page Sections / Demos            │      │   │   │
│  │  │  └─────────────────┬───────────────────────┘      │   │   │
│  │  │                    │                              │   │   │
│  │  │  ┌─────────────────▼───────────────────────┐      │   │   │
│  │  │  │     Motion Primitives Components        │      │   │   │
│  │  │  │  (115 components in 18 categories)      │      │   │   │
│  │  │  └─────────────────────────────────────────┘      │   │   │
│  │  └─────────────────────────────────────────────────┘  │   │
│  └───────────────────────────────────────────────────────┘   │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                   Animation Engines                       │   │
│  │                                                          │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌────────────────┐  │   │
│  │  │ Framer Motion│  │     GSAP     │  │   Three.js     │  │   │
│  │  │              │  │              │  │  + R3F + Drei  │  │   │
│  │  │ UI 微交互    │  │ 滚动动画     │  │  3D 效果       │  │   │
│  │  │ 声明式 API   │  │ 时间轴控制   │  │  WebGL 渲染    │  │   │
│  │  └──────────────┘  └──────────────┘  └────────────────┘  │   │
│  │                                                          │   │
│  │  ┌──────────────┐  ┌──────────────┐                      │   │
│  │  │    Lenis     │  │  CSS Animations│                     │   │
│  │  │              │  │               │                     │   │
│  │  │ 平滑滚动     │  │ 轻量级动画    │                     │   │
│  │  │ 惯性滚动     │  │ 背景效果等    │                     │   │
│  │  └──────────────┘  └──────────────┘                      │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │                   Styling System                         │   │
│  │                                                          │   │
│  │  ┌──────────────┐  ┌──────────────┐  ┌────────────────┐  │   │
│  │  │ Tailwind CSS │  │ CSS Variables│  │  Theme Tokens  │  │   │
│  │  │              │  │ (HSL format)│  │  (18 categories)│  │   │
│  │  │ 原子化 CSS   │  │ 运行时切换   │  │  4 directions  │  │   │
│  │  │ JIT 编译     │  │ 明暗模式     │  │  Light/Dark    │  │   │
│  │  └──────────────┘  └──────────────┘  └────────────────┘  │   │
│  └──────────────────────────────────────────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### 数据流

```
User Interaction
       │
       ▼
┌──────────────┐
│  Event       │
│  Handlers    │
└──────┬───────┘
       │
       ▼
┌──────────────┐     ┌──────────────┐
│  Local State │◄────│  Theme       │
│  (useState)  │     │  Context     │
└──────┬───────┘     └──────┬───────┘
       │                    │
       ▼                    ▼
┌──────────────┐     ┌──────────────┐
│  Props       │     │  CSS Vars    │
│  (downstream)│     │  (data-attr) │
└──────┬───────┘     └──────┬───────┘
       │                    │
       ▼                    ▼
┌──────────────────────────────────────┐
│         Rendered Output              │
│  (DOM + Canvas/WebGL + SVG)         │
└──────────────────────────────────────┘
```

---

## 🔧 技术栈选型

### ADR-001: 选择 Next.js 16 (App Router)

**日期**: 项目初始化
**状态: 已采纳**
**决策者**: itsjwill

#### 决策背景

需要选择一个 React 框架来构建组件库展示网站。

#### 考虑的选项

| 框架                           | 优势                                          | 劣势                    |
| ------------------------------ | --------------------------------------------- | ----------------------- |
| **Next.js (App Router)** | ✅ SSR/SSG、文件路由、Image优化、内置性能优化 | 学习曲线、约定多于配置  |
| **Vite + React**         | ✅ 快速 HMR、简单配置                         | ❌ 需要手动实现 SSR/SEO |
| **Gatsby**               | ✅ 纯 SSG、插件生态                           | ❌ 构建慢、灵活性差     |
| **Remix**                | ✅ 嵌套路由、优秀 DX                          | ❌ 生态较小、部署选项少 |

#### 决策

**选择 Next.js 16 App Router**

#### 理由

1. **SSR/SSG 支持**

   - 组件库网站需要良好的 SEO（搜索引擎优化）
   - 首屏加载性能重要（LCP < 2.5s）
   - App Router 的 Server Components 减少客户端 JS
2. **文件系统路由**

   - 组件文档页面的 URL 结构清晰：`/components/[category]/[slug]`
   - 自动代码分割（每个路由独立 bundle）
   - 布局嵌套（`layout.tsx`）
3. **内置优化**

   - `next/image` 自动图片优化（WebP、响应式、懒加载）
   - Font optimization（`next/font`）
   - Script optimization（`next/script`）
   - Bundle analysis（`@next/bundle-analyzer`）
4. **生态系统**

   - Vercel 部署零配置
   - 大量集成（Analytics、CMS、Auth）
   - 社区活跃，问题容易找到答案
5. **App Router 特有优势**

   - React Server Components（减少客户端 bundle）
   - Streaming/Suspense 边界
   - 更好的缓存策略

#### 后果

**正面**:

- ✅ 开箱即用的 SEO 和性能
- ✅ 清晰的项目结构
- ✅ 优秀的开发者体验
- ✅ Vercel 一键部署

**负面**:

- ⚠️ 需要学习 App Router 的新概念（Server Components、Loading UI 等）
- ⚠️ 某些库可能不兼容 Server Components（需要 `'use client'`）
- ⚠️ 与 Pages Router 的迁移成本（如果是旧项目）

#### 替代方案

如果未来需要更多控制：

- 可考虑 **Vite + React** + 手动 SSR（如使用 Vite SSR 插件）
- 或 **Remix**（如果嵌套路由是核心需求）

---

### ADR-002: 多引擎动画策略

**日期**: 项目初始化
**状态: 已采纳**

#### 决策背景

动画组件库需要覆盖广泛的动画场景：UI 微交互、滚动驱动动画、3D 效果、背景动画等。单一动画引擎难以满足所有需求。

#### 考虑的方案

**方案 A: 单一引擎（Framer Motion）**

- 优势: 统一 API、学习成本低
- 劣势:
  - 3D 支持弱（无 WebGL）
  - 滚动动画需额外库（`motion` 的 `useScroll` 功能有限）
  - 复杂时间轴控制不如 GSAP 灵活

**方案 B: 双引擎（Framer Motion + GSAP）**

- 优势: 覆盖大部分场景
- 劣势: 仍缺 3D 支持

**方案 C: 多引擎（Framer Motion + GSAP + Three.js + CSS）✅ 已采纳**

- 优势: 各取所长，专业场景用专业工具
- 劣势: 学习成本高、bundle 较大

#### 最终选择: 方案 C - 多引擎协作

#### 引擎分工

```
┌─────────────────────────────────────────────────────────────┐
│                    动画引擎职责划分                          │
├──────────────┬──────────────────┬───────────────────────────┤
│   引擎       │   适用场景        │        代表组件           │
├──────────────┼──────────────────┼───────────────────────────┤
│ Framer Motion│ UI 微交互        │ Dock, SpotlightCard,      │
│              │ 声明式动画        │ Accordion, AnimatedCounter │
│              │ 手势交互          │ SwipeCards, Magnetic      │
├──────────────┼──────────────────┼───────────────────────────┤
│ GSAP         │ 滚动驱动动画      │ GSAPScroll, ParallaxScroll│
│              │ 复杂时间轴        │ PageTransition, Preloader │
│              │ 精确控制          │ ScrollOrchestrator        │
├──────────────┼──────────────────┼───────────────────────────┤
│ Three.js     │ 3D 效果           │ Globe, NeuralNetwork,     │
│ (+ R3F)      │ WebGL 渲染        │ ParticleMorph, Robots    │
│              │ GPU 加速          │ Experimental effects     │
├──────────────┼──────────────────┼───────────────────────────┤
│ Lenis        │ 全局平滑滚动      │ SmoothScroll (core)      │
├──────────────┼──────────────────┼───────────────────────────┤
│ CSS          │ 轻量级动画        │ Aurora, Meteors, Marquee │
│ Animations   │ 背景效果          │ Shimmer, Gradient         │
│              │ 无 JS 开销        │ BorderBeam, Float         │
└──────────────┴──────────────────┴───────────────────────────┘
```

#### 理由

1. **Framer Motion - UI 交互之王**

   - 声明式 API，React 友好
   - 内置手势支持（drag, hover, tap, pan）
   - `AnimatePresence` 处理挂载/卸载动画
   - `MotionValue` 实现高性能响应式值
   - **最适合**: 组件级别的微交互
2. **GSAP - 时间轴大师**

   - `Timeline` 精确控制复杂序列
   - `ScrollTrigger` 强大的滚动绑定
   - 插件生态（DrawSVG, SplitText, MorphSVG）
   - 20KB+ 核心库（但功能强大）
   - **最适合**: 页面级编排、滚动叙事
3. **Three.js - 3D 唯一选择**

   - WebGL 封装，GPU 渲染
   - 几何体、材质、光照、相机
   - R3F 让其在 React 中更易用
   - Drei 提供常用 3D 组件（OrbitControls, Text3D 等）
   - **最适合**: 3D 场景、粒子系统、数据可视化
4. **Lenis - 平滑滚动专家**

   - 惯性滚动（momentum scrolling）
   - 与 GSAP ScrollTrigger 完美配合
   - 移动端触摸支持
   - 可定制摩擦力、持续时间
   - **最适合**: 全局滚动体验增强
5. **CSS Animations - 性能之选**

   - 零 JavaScript 开销
   - GPU 合成线程执行（不阻塞主线程）
   - 适合简单循环动画（背景、装饰）
   - **最适合**: 无限循环的视觉效果

#### Bundle 影响

```json
{
  "dependencies": {
    "framer-motion": "^11.0.0",     // ~35KB gzipped
    "gsap": "^3.12.5",             // ~25KB gzipped (core)
    "three": "^0.164.0",           // ~130KB (but code-split)
    "@react-three/fiber": "^8.16.0",
    "@react-three/drei": "^9.105.0",
    "lenis": "^1.1.0"              // ~5KB gzipped
  }
}
```

**优化措施**:

- Three.js 通过 `dynamic import` + `Suspense` 延迟加载
- GSAP 按需导入插件（不全部引入）
- Framer Motion tree-shakable（ESM）
- Lenis 很小，全局只需一个实例

#### 协作模式

```tsx
// 典型的多引擎协作示例
function HeroSection() {
  return (
    <section>
      {/* 1. CSS 背景: 极光效果（零 JS） */}
      <AuroraBackground />

      {/* 2. Three.js 3D: 地球仪（延迟加载） */}
      <Suspense fallback={<Loading />}>
        <Canvas>
          <Globe />
        </Canvas>
      </Suspense>

      {/* 3. Framer Motion: 文字动画 */}
      <motion.h1
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
      >
        Title
      </motion.h1>

      {/* 4. GSAP: 滚动触发 */}
      <div ref={gsapRef}>
        {/* GSAP ScrollTrigger 控制此区域 */}
      </div>
    </section>
  );
}
```

---

### ADR-003: 四方向设计系统

**日期**: 设计阶段早期
**状态: 已采纳**

#### 决策背景

大多数组件库只有一套视觉风格（或明暗两套）。但我们希望：

1. 展示组件在不同设计语言下的表现
2. 吸引不同审美偏好的用户
3. 作为设计系统的教学案例

#### 考虑的方案

**方案 A: 主题色切换（类似 Vercel/Stripe）**

- 只改变颜色（蓝 → 紫 → 绿...）
- 保持相同的形状、间距、字体
- ❌ 太单调，缺乏差异化

**方案 B: 明暗模式切换**

- Light / Dark
- ❌ 行业标准，不够独特

**方案 C: 完全不同的设计方向 ✅ 已采纳**

- 4 种独立的视觉语言
- 每种都有独特的颜色、字体、圆角、动画曲线
- 类似 Apple 的"外观"但更极致

#### 为什么是这四个方向？

选择基于 **当代 Web 设计的四大主流趋势**:

1. **Luxury (极简奢华)**

   - 代表: Apple, Linear, Hermès
   - 受众: 高端品牌、设计师、极简主义者
   - 关键词: 克制、精致、永恒
2. **Cyberpunk (赛博朋克)**

   - 代表: Blade Runner, Cyberpunk 2077, Terminal
   - 受众: 开发者、游戏玩家、科技爱好者
   - 关键词: 未来、反叛、科技感
3. **Kinetic (动能)**

   - 代表: Stripe, Vercel, Notion
   - 受众: SaaS 公司、创业团队、现代企业
   - 关键词: 活力、友好、专业但不无聊
4. **Freestyle (自由风格)**

   - 代表: Awwwards winners, Art direction
   - 受众: 创意机构、艺术家、品牌 campaign
   - 关键词: 大胆、意外、表达

#### 技术实现

**核心机制: CSS Custom Properties + data-attribute**

```html
<!-- 切换方向只需改变属性 -->
<html data-direction="luxury" class="dark">
<html data-direction="cyberpunk" class="dark">
<html data-direction="kinetic" class="">
<html data-direction="freestyle" class="dark">
```

```css
/* globals.css - 定义变量 */
[data-direction="luxury"] {
  --primary: 30 5% 8%;
  --font-heading: var(--font-luxury-heading);
  --radius: 0px;
  /* ... */
}

[data-direction="cyberpunk"] {
  --primary: 174 100% 50%;
  --font-heading: var(--font-cyber-heading);
  /* ... */
}
```

**为什么不用 CSS-in-JS (如 styled-components)?**

| 方面                    | CSS Variables           | CSS-in-JS         |
| ----------------------- | ----------------------- | ----------------- |
| **运行时切换**    | ✅ 即时（改 data-attr） | ⚠️ 需要重渲染   |
| **性能**          | ✅ 浏览器原生           | ⚠️ JS 注入样式  |
| **SSR**           | ✅ 无问题               | ⚠️ 需要处理水合 |
| **Tailwind 集成** | ✅ 完美（variants）     | ⚠️ 需要额外配置 |
| **调试**          | ✅ DevTools 直接看      | ⚠️ 需要插件     |
| **Bundle 大小**   | ✅ 0                    | ⚠️ ~10KB        |

#### 扩展性

添加新方向只需：

1. 在 `theme.tsx` 添加类型和元数据
2. 在 `globals.css` 添加 CSS 变量块
3. 在 `fonts.ts` 添加字体
4. 在 `tailwind.config.ts` 添加 variant
5. （可选）调整某些组件的默认样式

**预计工作量**: 2-4 小时

---

## 📁 目录结构决策

### ADR-004: 组件分类策略

**决策**: 按**功能和视觉类别**组织组件（而非按技术实现）

#### 考虑的方案

**方案 A: 按技术实现分类**

```
components/
├── framer-motion/    # 所有 FM 组件
├── gsap/            # 所有 GSAP 组件
├── threejs/         # 所有 Three.js 组件
└── css/             # 所有 CSS 动画组件
```

❌ **问题**: 用户不关心实现技术，只关心"我需要一个按钮"

**方案 B: 按功能分类 ✅ 已采纳**

```
components/
├── interactive/     # 交互组件（Dock, Modal, Tooltip...）
├── backgrounds/     # 背景效果（Aurora, Particles...）
├── cards/           # 卡片（GlowCard, TiltCard...）
├── text/            # 文字动画（TextReveal...）
├── scroll/          # 滚动效果（ParallaxScroll...）
├── three/           # 3D 效果（Globe, NeuralNetwork...）
└── ...
```

✅ **优势**:

- 用户可以快速找到需要的组件类型
- 符合心理模型（"我要一个卡片" vs "我要一个 Framer Motion 组件"）
- 类似 Shadcn/UI、Mantine 的组织方式

#### 分类原则

1. **按用户意图分组**

   - 用户想"添加一个背景" → 去 `backgrounds/`
   - 用户想"做一个卡片" → 去 `cards/`
   - 用户想"加个 3D 效果" → 去 `three/`
2. **控制分类数量**

   - 太少（< 5）→ 每个分类太拥挤
   - 太多（> 20）→ 难以导航
   - **当前: 18 个分类**（平衡点）
3. **允许交叉引用**

   - 一个组件可能在多个场景有用
   - 通过 `index.ts` 导出，方便查找

#### 特殊目录解释

**`core/` - 核心基础设施**

- 不是给用户直接使用的"组件"
- 是应用运行的基础（SmoothScroll, SiteNav, DirectionPicker）
- 放在 `components/` 但标记为内部

**`docs/` - 文档工具组件**

- CodeBlock, ComponentPreview, PropsTable
- 用于构建组件文档页面
- 不属于"动画组件"范畴

**`templates/` - 页面模板**

- 更高层级的抽象
- 组合多个基础组件形成完整页面
- 帮助用户快速启动项目

**`experimental/` (在 three/ 下)**

- 标记为实验性的 3D 效果
- 可能性能不佳或 API 不稳定
- 提前告知用户风险

---

## 🎨 设计系统架构

### ADR-005: CSS Variables 驱动的主题系统

**决策**: 使用 CSS 自定义属性（非 JavaScript 对象）作为主题 token 的存储。

#### 技术对比

**方案 A: JavaScript Theme Object（类似 Mantine）**

```tsx
const theme = createTheme({
  colors: {
    primary: '#7C3AED',
    // ...
  },
});

// 使用
<div style={{ color: theme.colors.primary }}>
```

**方案 B: CSS Variables ✅ 已采纳**

```css
:root {
  --primary: 263 70% 50%;
}

/* 使用 */
.color-primary {
  color: hsl(var(--primary));
}
```

#### 选择 CSS Variables 的理由

1. **运行时可变（Runtime Theming）**

   ```tsx
   // 切换主题无需重渲染
   document.documentElement.setAttribute('data-direction', 'cyberpunk');
   // 浏览器自动更新所有使用该变量的元素
   ```
2. **层叠和继承**

   ```css
   /* 父组件设置 */
   .parent { --custom-color: red; }

   /* 子组件自动继承 */
   .child { color: var(--custom-color); } /* red */
   ```
3. **与 Tailwind 完美集成**

   ```js
   // tailwind.config.ts
   colors: {
     primary: 'hsl(var(--primary))',
   }

   // 使用
   <div className="text-primary">  <!-- 自动读取 CSS 变量 -->
   ```
4. **DevTools 友好**

   - 浏览器 DevTools 可以直接查看和修改 CSS 变量
   - 无需 React DevTools 或特殊插件
5. **Zero Runtime Cost**

   - 不需要 JavaScript 注入 `<style>` 标签
   - 不增加 JS bundle 大小
   - 浏览器原生优化
6. **SSR/SSG 安全**

   - 不会出现水合不匹配（hydration mismatch）
   - 服务端和客户端看到的样式一致

#### HSL 格式选择

**为什么用 HSL 而不是 HEX 或 RGB?**

```css
/* HEX: 难以调整 */
--primary: #7C3AED;
/* 想要变亮？需要转换计算... */

/* RGB: 相对容易但不够直观 */
--primary: rgb(124, 58, 237);
/* 调整亮度需要同时改变 R/G/B */

/* HSL ✅: 最直观 */
--primary: 263 70% 50%;  /* 色相 饱和度 亮度 */
/* 变亮只需改变第三个值！ */
--primary-light: 263 70% 70%;
--primary-dark: 263 70% 30%;
```

**实际应用**:

```css
/* 定义 */
--primary: 263 70% 50%;

/* 使用 */
.element {
  color: hsl(var(--primary));                    /* 正常 */
  color: hsl(var(--primary) / 80%);              /* 降低饱和度 */
  background: hsl(var(--primary) / 15%);         /* 极淡背景 */
  border-color: hsl(var(--primary) / 30%);        /* 边框 */
  box-shadow: 0 0 20px hsl(var(--primary) / 40%); /* 发光 */
}
```

---

## 🧩 组件设计原则

### ADR-006: "Copy-Paste Ready" 设计哲学

**决策**: 所有组件都应设计为可以直接复制到任何 Next.js 项目中使用。

#### 这意味着

1. **最小依赖**

   - 组件只依赖:
     - `react` & `react-dom` (必需)
     - `framer-motion` 或 `gsap` 或 `three` (按需)
     - `@/lib/utils` (cn() 函数)
   - 不依赖项目的特定配置（除了 Tailwind）
2. **自包含样式**

   - 样式通过 Tailwind 类名内联
   - 不需要额外的 CSS 文件
   - 不依赖全局样式（除了 CSS variables）
3. **清晰的 Props API**

   - TypeScript 接口完整
   - 合理的默认值
   - 必填/可选明确标注
4. **示例即文档**

   - 每个组件都有使用示例
   - 示例代码可直接复制运行

#### 实现示例

```tsx
// ✅ 好的设计: 自包含、可移植
'use client';

import { motion, useMotionValue, useSpring } from 'framer-motion';
import { cn } from '@/lib/utils';

interface DockProps {
  children: React.ReactNode;
  className?: string;
  magnification?: number;  // 有默认值
  distance?: number;
}

export function Dock({ children, className, magnification = 60, distance = 140 }: DockProps) {
  // 实现细节...
  return (
    <motion.div className={cn('base-styles', className)}>
      {children}
    </motion.div>
  );
}

// ❌ 差的设计: 依赖外部状态、难以复用
export function Dock() {
  const theme = useTheme();  // 依赖全局 context
  const config = useConfig(); // 依赖外部配置

  return (
    <div className={theme === 'dark' ? 'dark-dock' : 'light-dock'}>
      {/* ... */}
    </div>
  );
}
```

#### 权衡

**优点**:

- ✅ 降低用户使用门槛
- ✅ 提升传播度（易于分享单个组件）
- ✅ 促进社区贡献

**缺点**:

- ⚠️ 可能导致一些重复代码（不能过度抽象）
- ⚠️ 组件间的一致性需要人工维护（没有强制的共享逻辑）
- ⚠️ 难以做全局变更（如统一修改所有组件的某个行为）

**缓解措施**:

- 提供 `index.ts` 统一导出
- 文档说明最佳实践
- 核心逻辑抽取到 `@/lib/` 共享

---

## 📊 状态管理策略

### ADR-007: 轻量级状态管理（Context + useState）

**决策**: 不引入 Redux/Zustand/Jotai 等状态管理库，使用 React 内置方案。

#### 理由

1. **状态简单**

   - 主要状态: 当前方向（direction）、模式（mode）
   - 局部状态: 各组件内部（isOpen, isActive 等）
   - 不需要复杂的状态派生、时间旅行调试
2. **Context 足够用**

   ```tsx
   // ThemeProvider 只提供 2 个状态 + 3 个方法
   interface ThemeContextValue {
     direction: Direction;
     mode: Mode;
     setDirection: (d: Direction) => void;
     setMode: (m: Mode) => void;
     toggleMode: () => void;
   }
   ```
3. **避免过度工程**

   - YAGNI (You Aren't Gonna Need It)
   - Redux boilerplate 不值得
   - 保持 bundle 小
4. **未来可扩展**

   - 如果真的需要，可以轻松迁移到 Zustand（API 类似）
   - 或者使用 Jotai（原子化状态）

#### 状态分布

```
Global State (Context)
├── ThemeProvider
│   ├── direction: 'luxury' | 'cyberpunk' | 'kinetic' | 'freestyle'
│   ├── mode: 'light' | 'dark'
│   └── methods: setDirection, setMode, toggleMode, cycleDirection
│
Local State (useState/useReducer)
├── Each Component
│   ├── isOpen (Modal, Drawer, Accordion)
│   ├── isActive (Tabs, Switch)
│   ├── count (AnimatedCounter)
│   ├── position (MouseParallax, Magnetic)
│   └── ...

Server State (if needed)
├── Future: component metadata from CMS/API
└── Future: user preferences from backend
```

#### 数据流示例

```
User clicks "Cyberpunk" button
       │
       ▼
DirectionPicker.onClick()
       │
       ▼
theme.setDirection('cyberpunk')
       │
       ▼
ThemeProvider state updates
       │
       ▼
<html data-direction="cyberpunk">  (DOM attribute changes)
       │
       ▼
CSS :root variables update (browser re-calculates)
       │
       ▼
All elements using hsl(var(--xxx)) re-render visually
       │
       ▼
User sees instant theme change ✨
```

---

## ⚡ 性能架构

### ADR-008: 渐进增强与优雅降级

**决策**: 核心功能在所有设备可用，高级特性逐步增强。

#### 分层策略

```
Layer 4: Experimental 3D (WebGPU, heavy GLSL)
    ↓ Only on high-end desktop with user opt-in
Layer 3: Standard 3D (Three.js, WebGL)
    ↓ Desktop + high-end mobile (with quality reduction)
Layer 2: Advanced Animations (GSAP, complex Framer Motion)
    ↓ Most modern devices
Layer 1: Basic Animations (CSS animations, simple FM)
    ↓ All devices (including low-end)
Layer 0: Static Content (no animation)
    ↓ Always works (reduced motion preference)
```

#### 实现方式

**1. 设备检测 + 条件渲染**

```tsx
function AdaptiveScene() {
  const [capabilities, setCapabilities] = useState({
    webgl: false,
    preferredQuality: 'low',  // 'low' | 'medium' | 'high'
  });

  useEffect(() => {
    // 检测 WebGL 支持
    const canvas = document.createElement('canvas');
    const gl = canvas.getContext('webgl') || canvas.getContext('experimental-webgl');
  
    // 检测硬件并发数（近似性能指标）
    const cores = navigator.hardwareConcurrency || 4;
  
    // 检测内存（如果可用）
    const memory = navigator.deviceMemory || 4; // GB
  
    setCapabilities({
      webgl: !!gl,
      preferredQuality: cores >= 8 && memory >= 8 ? 'high' :
                        cores >= 4 && memory >= 4 ? 'medium' : 'low',
    });
  }, []);

  if (!capabilities.webgl) {
    return <CSSFallbackAnimation />;  // Layer 1-2
  }

  return (
    <Canvas>
      {capabilities.preferredQuality === 'high' && <HighQualityScene />}
      {capabilities.preferredQuality === 'medium' && <MediumQualityScene />}
      {capabilities.preferredQuality === 'low' && <LowQualityScene />}
    </Canvas>
  );
}
```

**2. prefers-reduced-motion 媒体查询**

```tsx
import { useReducedMotion } from '@/hooks';

function AnimatedComponent() {
  const shouldReduceMotion = useReducedMotion();

  return (
    <motion.div
      // 如果用户偏好减少动画，跳过动画直接显示最终状态
      initial={shouldReduceMotion ? {} : { opacity: 0, y: 20 }}
      animate={shouldReduceMotion ? {} : { opacity: 1, y: 0 }}
      transition={shouldReduceMotion ? { duration: 0 } : { duration: 0.5 }}
    >
      Content
    </motion.div>
  );
}
```

**3. Lazy Loading + Suspense**

```tsx
const Heavy3DComponent = lazy(() => import('./Heavy3DComponent'));

function Page() {
  return (
    <>
      {/* 立即显示的内容 */}
      <Header />
    
      {/* 延迟加载的重型组件 */}
      <Suspense fallback={<Skeleton />}>
        <Heavy3DComponent />
      </Suspense>
    </>
  );
}
```

#### 权衡

**优点**:

- ✅ 广泛的设备兼容性
- ✅ 尊重用户偏好（无障碍）
- ✅ 渐进式体验（不会完全无法使用）

**缺点**:

- ⚠️ 需要维护多个版本/质量级别
- ⚠️ 增加代码复杂度和测试矩阵
- ⚠️ 可能导致"体验不一致"的反馈

---

## 📝 关键决策记录 (ADR) 索引

| ADR编号           | 标题                         | 状态   | 日期     | 影响范围  |
| ----------------- | ---------------------------- | ------ | -------- | --------- |
| **ADR-001** | 选择 Next.js 16 (App Router) | 已采纳 | 初始化   | 全局架构  |
| **ADR-002** | 多引擎动画策略               | 已采纳 | 初始化   | 技术栈    |
| **ADR-003** | 四方向设计系统               | 已采纳 | 设计阶段 | 视觉系统  |
| **ADR-004** | 组件分类策略                 | 已采纳 | 设计阶段 | 目录结构  |
| **ADR-005** | CSS Variables 主题系统       | 已采纳 | 设计阶段 | 样式架构  |
| **ADR-006** | Copy-Paste Ready 哲学        | 已采纳 | 开发阶段 | 组件设计  |
| **ADR-007** | 轻量级状态管理               | 已采纳 | 开发阶段 | 状态管理  |
| **ADR-008** | 渐进增强策略                 | 已采纳 | 开发阶段 | 性能/兼容 |

---

## 🔮 未来演进方向

### 短期（1-3个月）

#### 1. 组件测试覆盖率

**当前状态**: 缺少自动化测试
**目标**: 核心组件 80%+ 覆盖率

**计划**:

- 使用 **Testing Library** + **Jest** 进行单元测试
- 使用 **Playwright** 进行 E2E 测试
- 重点测试:
  - Props API（必填、可选、默认值）
  - 交互行为（点击、悬停、拖拽）
  - 无障碍（键盘导航、屏幕阅读器）
  - 响应式布局

#### 2. Storybook 集成

**目标**: 为每个组件生成交互式文档

**收益**:

- 可视化组件所有状态（props 组合）
- 隔离开发环境
- 自动生成 Props 文档
- 团队协作的单一事实来源

#### 3. 组件注册表完善

**当前**: [component-registry.ts](../src/lib/component-registry.ts) 存在但未充分利用

**目标**:

- 自动生成组件文档页面
- 提供搜索/过滤功能
- 显示组件依赖关系
- 版本兼容性信息

### 中期（3-6个月）

#### 4. 设计方向扩展?

**可能性**: 添加第 5 个方向？

候选方向:

- **Minimal** - 极简主义（比 Luxury 更极端）
- **Retro** - 复古风格（像素风、CRT 效果）
- **Nature** - 自然有机（绿色、圆润、手绘感）
- **Brutalism** - 粗野主义（原始 HTML 风格、高对比）

**决策标准**:

- 是否有足够独特的视觉语言？
- 是否有真实的用户需求？
- 维护成本是否可接受？

#### 5. 动画编辑器?

**愿景**: 可视化配置动画参数

```
┌─────────────────────────────────────┐
│  Animation Editor (概念)           │
├─────────────────────────────────────┤
│                                     │
│  Component: [Dock ▼]                │
│                                     │
│  Properties:                        │
│  ├─ Magnification: [====60===]     │
│  ├─ Distance:     [===140===]      │
│  ├─ Direction:    [bottom    ▼]    │
│  └─ Easing:       [spring    ▼]    │
│                                     │
│  Preview:                            │
│  ┌─────────────────────────────────┐│
│  │  📁 📂 📄 📝 (live preview)  ││
│  └─────────────────────────────────┘│
│                                     │
│  [Export Code]  [Copy to Clipboard] │
│                                     │
└─────────────────────────────────────┘
```

**技术挑战**:

- 实时预览性能
- 复杂 props 的 UI（如 easing 曲线）
- 导出代码的格式化

#### 6. Figma Plugin?

**目标**: 设计师可以在 Figma 中使用这些组件

**价值**:

- 缩短设计到开发的距离
- 设计师可以直接"试用"组件
- 生成带正确 props 的代码片段

### 长期（6-12个月）

#### 7. 组件市场/包发布?

**考虑**: 发布到 npm 作为独立包

**形态可能**:

```bash
# 安装单个组件
npm install @motion-primitives/dock
npm install @motion-primitives/globe

# 或安装完整集合
npm install motion-primitives
```

**挑战**:

- 版本管理（115 个组件的独立版本？）
- 依赖声明（peerDependencies）
- Tree-shaking 保证
- 文档站点同步

#### 8. AI 辅助组件生成?

**愿景**: 描述需求，AI 生成组件代码

```
User: "I want a card that glows purple on hover 
      and has a subtle tilt effect following the mouse"

AI: generates GlowTiltCard component with:
- Proper TypeScript types
- Framer Motion implementation
- Tailwind classes
- Accessibility attributes
- Usage example
```

**可行性**:

- 基于 LLM（GPT-4, Claude 等）
- 训练数据: 现有的 115 个组件 + 文档
- 需要: 严格的代码审查流程

#### 9. Web Components 支持?

**目标**: 框架无关的组件版本

**动机**:

- 服务 Angular/Vue/Svelte 用户
- 微前端架构中的跨框架复用
- 未来-proof（不依赖 React 生态系统）

**技术路径**:

```tsx
// 现有 React 组件
function Dock({ children, magnification }) { ... }

// 包装为 Web Component
class DockElement extends HTMLElement {
  connectedCallback() {
    const root = this.attachShadow({ mode: 'open' });
    ReactDOM.createRoot(root).render(
      <Dock magnification={this.getAttribute('magnification')}>
        <slot />
      </Dock>
    );
  }
}

customElements.define('mp-dock', DockElement);
```

**使用**:

```html
<!-- 任何框架都可以用 -->
<mp-dock magnification="60">
  <button>Item 1</button>
  <button>Item 2</button>
</mp-dock>
```

---

## 📚 参考资源

### 架构模式

- **[Component Driven Development]** ([https://www.shadcn.com/blog/component-driven-development](https://www.shadcn.com/blog/component-driven-development))
- **[Atomic Design]** ([http://bradfrost.com/blog/post/atomic-design-in-design-systems/](http://bradfrost.com/blog/post/atomic-design-in-design-systems/))
- **[Design Systems Repo Pattern]** ([https://github.com/nickbull/building-design-systems](https://github.com/nickbull/building-design-systems))

### ADR 模板

- **[Markdown ADR Template]** ([https://adr.github.io/](https://adr.github.io/))
- **[Decision Records Best Practices]** ([https://thinkrelevance.com/blog/2017/11/15/documenting-architecture-decisions](https://thinkrelevance.com/blog/2017/11/15/documenting-architecture-decisions))

### 性能架构

- **[Progressive Enhancement]** ([https://developer.mozilla.org/en-US/docs/Glossary/Progressive_enhancement](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_enhancement))
- **[Core Web Vitals]** ([https://web.dev/vitals/](https://web.dev/vitals/))

---

## 🔄 文档维护

### 更新频率

- **架构决策记录 (ADR)**: 当做出重大技术决策时更新
- **未来方向**: 每季度回顾一次
- **本文件本身**: 当架构发生重大变化时修订

### 贡献指南

如果你发现架构问题或有改进建议：

1. 创建 Issue 描述问题和建议方案
2. 如果是新的 ADR，使用模板:

   ```
   ## ADR-XXX: [标题]

   - **状态**: 提案/已采纳/已否决/已替代
   - **背景**: 为什么需要这个决策
   - **选项**: 考虑了哪些方案
   - **决策**: 最终选择了什么
   - **理由**: 为什么这样选择
   - **后果**: 正面和负面影响
   ```
3. 提交 PR 并 @maintainers review

---

*最后更新: 2026-10-22*
*文档负责人: itsjwill*
*下次回顾: 2027-01-22*
*相关文档: [PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md) | [DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md) | [PERFORMANCE.md](./PERFORMANCE.md)*
