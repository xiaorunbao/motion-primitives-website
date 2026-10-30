# Motion Primitives - 组件详细使用指南

> 本文档提供所有核心组件的详细 API、属性说明和使用示例。

---

## 📖 目录

1. [交互组件 (Interactive)](#-interactive-26个)
2. [背景效果 (Backgrounds)](#-backgrounds-10个)
3. [卡片组件 (Cards)](#-cards-7个)
4. [3D 效果 (Three)](#-three-13个)
5. [滚动动画 (Scroll)](#-scroll-7个)
6. [文字动画 (Text)](#-text-5个)
7. [视觉特效 (Effects)](#-effects-12个)
8. [页面过渡 (Transitions)](#-transitions-3个)
9. [布局组件 (Layout)](#-layout-4个)
10. [导航组件 (Navigation)](#-navigation-2个)
11. [SEO 组件 (SEO)](#-seo-12个)
12. [自定义 Hooks](#-hooks-6个)

---

## 🎯 Interactive（26个）

### 1. Dock（Dock 栏）

**文件**: [dock.tsx](../src/components/interactive/dock.tsx)

macOS 风格的 Dock 栏，鼠标靠近时图标放大。

#### Props

```typescript
interface DockProps {
  children: ReactNode;           // DockItem 子组件
  className?: string;            // 自定义类名
  magnification?: number;        // 放大倍数，默认 60
  distance?: number;             // 触发距离，默认 140
  direction?: "bottom" | "top" | "left" | "right";  // 方向，默认 "bottom"
}

interface DockItemProps {
  children: ReactNode;           // 图标内容
  className?: string;
  label?: string;                // 悬停标签
  onClick?: () => void;          // 点击事件
}
```

#### 使用示例

```tsx
import { Dock, DockItem } from '@/components/interactive';

export function MyDock() {
  return (
    <Dock magnification={60} distance={140} direction="bottom">
      <DockItem label="Home" onClick={() => console.log('Home')}>
        <HomeIcon size={24} />
      </DockItem>
      <DockItem label="Search" onClick={() => console.log('Search')}>
        <SearchIcon size={24} />
      </DockItem>
      <DockItem label="Settings">
        <SettingsIcon size={24} />
      </DockItem>
    </Dock>
  );
}
```

#### 特性
- ✅ 支持四个方向（上下左右）
- ✅ 平滑的放大动画（Framer Motion spring）
- ✅ 悬停显示标签
- ✅ 响应式设计

---

### 2. SpotlightCard（聚光灯卡片）

**文件**: [spotlight-card.tsx](../src/components/interactive/spotlight-card.tsx)

鼠标跟随的聚光灯效果卡片。

#### Props

```typescript
interface SpotlightCardProps {
  children: ReactNode;
  className?: string;
  spotlightColor?: string;       // 聚光灯颜色，默认 "rgba(0,0,0,0.2)"
}
```

#### 使用示例

```tsx
import { SpotlightCard } from '@/components/interactive';

<SpotlightCard spotlightColor="rgba(139, 92, 246, 0.15)">
  <h3>标题</h3>
  <p>鼠标移动时会有聚光灯效果</p>
</SpotlightCard>
```

---

### 3. AnimatedCounter（动画计数器）

**文件**: [animated-counter.tsx](../src/components/interactive/animated-counter.tsx)

数字从 0 动画增长到目标值。

#### Props

```typescript
interface AnimatedCounterProps {
  value: number;                 // 目标值
  duration?: number;             // 动画时长（毫秒），默认 2000
  className?: string;
  prefix?: string;               // 前缀（如 "$"）
  suffix?: string;               // 后缀（如 "%"）
  decimals?: number;             // 小数位数，默认 0
}
```

#### 使用示例

```tsx
import { AnimatedCounter } from '@/components/interactive';

<div className="grid grid-cols-3 gap-8">
  <div>
    <AnimatedCounter value={1234} prefix="$" />
    <p>收入</p>
  </div>
  <div>
    <AnimatedCounter value={98} suffix="%" decimals={1} />
    <p>完成率</p>
  </div>
  <div>
    <AnimatedCounter value={56789} duration={3000} />
    <p>用户数</p>
  </div>
</div>
```

---

### 4. Marquee（跑马灯）

**文件**: [marquee.tsx](../src/components/interactive/marquee.tsx)

无限水平或垂直滚动的跑马灯效果。

#### Props

```typescript
interface MarqueeProps {
  children: ReactNode;
  className?: string;
  direction?: 'horizontal' | 'vertical';  // 滚动方向
  speed?: number;                 // 速度，默认 40
  pauseOnHover?: boolean;         // 悬停暂停，默认 true
  reverse?: boolean;              // 反向滚动
}
```

#### 使用示例

```tsx
import { Marquee } from '@/components/interactive';

<Marquee speed={50} pauseOnHover>
  <img src="/logo1.png" alt="Logo 1" className="mx-4" />
  <img src="/logo2.png" alt="Logo 2" className="mx-4" />
  <img src="/logo3.png" alt="Logo 3" className="mx-4" />
</Marquee>

<Marquee direction="vertical" speed={30}>
  <p className="py-2">公告 1</p>
  <p className="py-2">公告 2</p>
  <p className="py-2">公告 3</p>
</Marquee>
```

---

### 5. Accordion（手风琴）

**文件**: [accordion.tsx](../src/components/interactive/accordion.tsx)

可折叠的手风琴面板。

#### Props

```typescript
interface AccordionItemProps {
  title: string;                  // 面板标题
  children: ReactNode;            // 面板内容
  defaultOpen?: boolean;          // 默认展开，默认 false
  className?: string;
}

interface AccordionProps {
  children: ReactNode;            // AccordionItem 子组件
  className?: string;
  multiple?: boolean;             // 允许同时展开多个，默认 false
}
```

#### 使用示例

```tsx
import { Accordion, AccordionItem } from '@/components/interactive';

<Accordion multiple>
  <AccordionItem title="什么是 Motion Primitives？">
    一个免费的动画组件库...
  </AccordionItem>
  <AccordionItem title="如何使用？" defaultOpen>
    直接复制粘贴代码即可...
  </AccordionItem>
  <AccordionItem title="支持哪些框架？">
    Next.js、React、Gatsby 等...
  </AccordionItem>
</Accordion>
```

---

### 6. 其他重要交互组件

#### Modal（模态框）
```tsx
import { Modal } from '@/components/interactive';

<Modal isOpen={isOpen} onClose={() => setIsOpen(false)}>
  <h2>标题</h2>
  <p>内容</p>
</Modal>
```

#### Toast（轻提示）
```tsx
import { Toast } from '@/components/interactive';

<Toast
  message="操作成功"
  type="success"  // "success" | "error" | "info" | "warning"
  duration={3000}
  onClose={() => {}}
/>
```

#### Command Palette（命令面板）
```tsx
import { CommandPalette } from '@/components/interactive';

<CommandPalette
  commands={[
    { id: '1', title: '搜索...', shortcut: '⌘K' },
    { id: '2', title: '设置', shortcut: '⌘,' },
  ]}
  onSelect={(cmd) => console.log(cmd)}
/>
```

#### Confetti（五彩纸屑）
```tsx
import { Confetti } from '@/components/interactive';

<button onClick={() => Confetti.fire()}>
  庆祝！🎉
</button>
```

---

## 🌄 Backgrounds（10个）

### 1. AuroraBackground（极光背景）

**文件**: [aurora.tsx](../src/components/backgrounds/aurora.tsx)

流动的极光效果背景。

#### Props

```typescript
interface AuroraBackgroundProps {
  children?: React.ReactNode;
  className?: string;
  showRadialGradient?: boolean;   // 显示径向渐变遮罩，默认 true
}
```

#### 使用示例

```tsx
import { AuroraBackground } from '@/components/backgrounds';

<AuroraBackground showRadialGradient>
  <div className="relative z-10">
    <h1 className="text-4xl font-bold text-white">极光背景</h1>
    <p className="text-white/80">内容在极光之上</p>
  </div>
</AuroraBackground>
```

#### CSS 变量自定义
可以通过 CSS 变量调整颜色：
```css
.mp-aurora {
  --white: #ffffff;
  --black: #000000;
  --blue-500: #3b82f6;
  --indigo-300: #818cf8;
  --blue-300: #93c5fd;
  --violet-200: #ddd6fe;
  --blue-400: #60a5fa;
  --transparent: transparent;
}
```

---

### 2. Meteors（流星效果）

**文件**: [meteors.tsx](../src/components/backgrounds/meteors.tsx)

从右上角划过的流星雨效果。

#### Props

```typescript
interface MeteorsProps {
  number?: number;                // 流星数量，默认 20
  className?: string;
}
```

#### 使用示例

```tsx
import { Meteors } from '@/components/backgrounds';

<div className="relative h-[400px] overflow-hidden rounded-lg bg-black">
  <Meteors number={30} />
  <div className="relative z-10 p-8 text-white">
    <h2>流星雨背景</h2>
  </div>
</div>
```

---

### 3. Particles（粒子系统）

**文件**: [particles.tsx](../src/components/backgrounds/particles.tsx)

浮动粒子背景效果。

#### Props

```typescript
interface ParticlesProps {
  quantity?: number;              // 粒子数量，默认 50
  color?: string;                 // 粒子颜色，默认 "#ffffff"
  size?: number;                  // 粒子大小，默认 2
  speed?: number;                 // 运动速度，默认 1
  className?: string;
}
```

---

### 4. Grid / InfiniteGrid（网格背景）

```tsx
import { Grid, InfiniteGrid } from '@/components/backgrounds';

// 静态网格
<Grid color="rgba(255,255,255,0.1)" size={40} />

// 无限滚动网格
<InfiniteGrid speed={0.5} />
```

---

### 5. GradientMesh（渐变网格）

动态变化的渐变色块背景。

```tsx
import { GradientMesh } from '@/components/backgrounds';

<GradientMesh
  colors={['#667eea', '#764ba2', '#f093fb']}
  speed={2}
  opacity={0.5}
/>
```

---

## 🃏 Cards（7个）

### 1. TiltCard（3D 倾斜卡片）

**文件**: [tilt-card.tsx](../src/components/cards/tilt-card.tsx)

跟随鼠标产生 3D 倾斜效果的卡片。

#### Props

```typescript
interface TiltCardProps {
  children: ReactNode;
  className?: string;
  tiltAmount?: number;            // 倾斜角度，默认 10（度）
  glareEnable?: boolean;          // 启用光泽效果，默认 true
  glareMaxOpacity?: number;       // 光泽最大透明度，默认 0.2
  scale?: number;                 // 悬停缩放，默认 1.02
  transitionSpeed?: number;       // 过渡速度（毫秒），默认 400
}
```

#### 使用示例

```tsx
import { TiltCard } from '@/components/cards';

<TiltCard tiltAmount={15} glareEnable scale={1.05}>
  <img src="/project.jpg" alt="Project" />
  <div className="p-6">
    <h3>项目标题</h3>
    <p>描述文本</p>
  </div>
</TiltCard>
```

---

### 2. GlowCard（发光卡片）

**文件**: [glow-card.tsx](../src/components/cards/glow-card.tsx)

悬停时产生辉光效果的卡片。

#### Props

```typescript
interface GlowCardProps {
  children: ReactNode;
  className?: string;
  glowColor?: string;             // 发光颜色，默认主题色
  glowSize?: string;              // 发光大小，默认 "20px"
  intensity?: number;             // 发光强度，默认 0.5
}
```

#### 使用示例

```tsx
import { GlowCard } from '@/components/cards';

<GlowCard glowColor="#8B5CF6" intensity={0.7}>
  <CardContent>
    <GlowIcon>
      <Sparkles />
    </GlowIcon>
    <h3>AI 功能</h3>
    <p>智能化的用户体验</p>
  </CardContent>
</GlowCard>
```

---

### 3. BentoGrid（便当盒网格）

**文件**: [bento-grid.tsx](../src/components/cards/bento-grid.tsx)

类似 Apple 风格的不规则网格布局。

#### Props

```typescript
interface BentoGridProps {
  children: ReactNode;
  className?: string;
  cols?: number;                  // 列数，默认 4
  gap?: number;                   // 间距，默认 16 (px)
}

interface BentoGridItemProps {
  children: ReactNode;
  className?: string;
  colSpan?: number;               // 跨列数
  rowSpan?: number;               // 跨行数
}
```

#### 使用示例

```tsx
import { BentoGrid, BentoGridItem } from '@/components/cards';

<BentoGrid cols={4} gap={16}>
  <BentoGridItem colSpan={2} rowSpan={2}>
    {/* 大卡片 */}
  </BentoGridItem>
  <BentoGridItem>
    {/* 小卡片 */}
  </BentoGridItem>
  <BentoGridItem>
    {/* 小卡片 */}
  </BentoGridItem>
  <BentoGridItem colSpan={2}>
    {/* 宽卡片 */}
  </BentoGridItem>
</BentoGrid>
```

---

### 4. StackCards（堆叠卡片）

**文件**: [stack-cards.tsx](../src/components/cards/stack-cards.tsx)

可展开的堆叠卡片组。

#### Props

```typescript
interface StackCardsProps {
  cards: Array<{
    id: string | number;
    content: ReactNode;
  }>;
  className?: string;
  expanded?: boolean;             // 默认展开状态
}
```

---

## 🌐 Three（13个）

### 1. Globe（3D 地球仪）

**文件**: [globe.tsx](../src/components/three/globe.tsx)

高性能 3D 地球仪组件，支持线框和点阵两种模式。

#### Props

```typescript
interface GlobeProps {
  variant?: "wireframe" | "dotted";  // 渲染模式
  color?: string;                 // 主色调，默认 "#8B5CF6"
  dotColor?: string;              // 点阵颜色（仅 dotted 模式），默认 "#ffffff"
  rotationSpeed?: number;         // 自转速度，默认 0.001
  radius?: number;                // 半径，默认 2
  showRings?: boolean;            // 显示轨道环，默认 true
  className?: string;
  style?: React.CSSProperties;    // 容器样式
}
```

#### 使用示例

```tsx
import { Globe } from '@/components/three';

// 线框模式
<Globe
  variant="wireframe"
  color="#00FFD1"
  radius={2.5}
  rotationSpeed={0.002}
/>

// 点阵模式（带连接弧线）
<Globe
  variant="dotted"
  color="#8B5CF6"
  dotColor="#ffffff"
  radius={2}
  showRings={true}
/>
```

#### 性能优化建议
- 降低 `radius` 可减少多边形数量
- 在移动设备上使用 `rotationSpeed={0}` 禁用自转
- 使用 `variant="wireframe"` 比 `dotted` 性能更好

---

### 2. FloatingShapes（悬浮几何体）

**文件**: [floating-shapes.tsx](../src/components/three/floating-shapes.tsx)

场景中悬浮的 3D 几何体（立方体、球体、圆环等）。

#### Props

```typescript
interface FloatingShapesProps {
  count?: number;                 // 形状数量，默认 20
  shapes?: Array<'cube' | 'sphere' | 'torus' | 'octahedron'>;  // 形状类型
  color?: string;                 // 颜色
  sizeRange?: [number, number];   // 尺寸范围
  speed?: number;                 // 运动速度
  className?: string;
}
```

#### 使用示例

```tsx
import { FloatingShapes } from '@/components/three';

<FloatingShapes
  count={30}
  shapes={['cube', 'sphere', 'torus']}
  color="#8B5CF6"
  sizeRange={[0.1, 0.5]}
  speed={1}
/>
```

---

### 3. NeuralNetwork（神经网络可视化）

**文件**: [neural-network.tsx](../src/components/three/neural-network.tsx)

3D 神经网络节点和连接的可视化。

#### Props

```typescript
interface NeuralNetworkProps {
  nodeCount?: number;             // 节点数，默认 50
  connectionDistance?: number;    // 连接距离阈值，默认 3
  nodeColor?: string;             // 节点颜色
  connectionColor?: string;       // 连接线颜色
  nodeSize?: number;              // 节点大小
  animationSpeed?: number;        // 动画速度
}
```

#### 使用示例

```tsx
import { NeuralNetwork } from '@/components/three';

<NeuralNetwork
  nodeCount={80}
  connectionDistance={4}
  nodeColor="#00FFD1"
  connectionColor="rgba(0, 255, 209, 0.2)"
/>
```

---

### 4. ParticleMorph（粒子变形）

**文件**: [particle-morph.tsx](../src/components/three/particle-morph.tsx)

粒子在不同形状之间变形的动画效果。

#### Props

```typescript
interface ParticleMorphProps {
  particleCount?: number;         // 粒子数，默认 1000
  shapes?: Array<string>;         // 目标形状（SVG path data 或预设名）
  morphDuration?: number;         // 变形时长（秒），默认 2
  color?: string;                 // 粒子颜色
  size?: number;                  // 粒子大小
}
```

---

### 5. ScrollProgress3D（3D 滚动进度）

**文件**: [scroll-progress-3d.tsx](../src/components/three/scroll-progress-3d.tsx)

3D 视觉化的滚动进度指示器。

#### Props

```typescript
interface ScrollProgress3DProps {
  type?: 'bar' | 'circle' | 'globe';  // 类型
  color?: string;
  thickness?: number;            // 线条粗细
  position?: 'top' | 'bottom' | 'left' | 'right';
}
```

---

### 🤖 3D Robots（机器人模型）

路径: `src/components/three/robots/`

预构建的 3D 机器人角色：

| 组件 | 文件 | 描述 |
|------|------|------|
| `BubbleBot` | [bubble-bot.tsx](../src/components/three/robots/bubble-bot.tsx) | 可爱的泡泡机器人 |
| `ChromeTitan` | [chrome-titan.tsx](../src/components/three/robots/chrome-titan.tsx) | 金属铬钛机器人 |
| `NeonSamurai` | [neon-samurai.tsx](../src/components/three/robots/neon-samurai.tsx) | 霓虹武士机器人 |

#### 使用示例

```tsx
import { NeonSamurai } from '@/components/three/robots';

<Canvas>
  <NeonSamurai
    position={[0, 0, 0]}
    scale={1}
    autoRotate
    rotationSpeed={0.5}
  />
</Canvas>
```

---

### 🔬 Experimental 3D（实验性效果）

路径: `src/components/three/experimental/`

**注意**: 这些组件可能性能消耗较大，适合展示用途。

| 组件 | 描述 | 性能影响 |
|------|------|---------|
| `DimensionalRift` | 维度裂缝效果 | ⚠️ 高 |
| `GravityWell` | 引力井模拟 | ⚠️ 高 |
| `LivingTrail` | 生命轨迹 | ⚠️ 中 |
| `MagneticFluid` | 磁性流体模拟 | 🔴 极高 |
| `SoundFabric` | 声音织物（需音频输入） | ⚠️ 中 |
| `SwarmIntelligence` | 群体智能（鸟群/鱼群） | ⚠️ 高 |

#### MagneticFluid 示例
```tsx
import { MagneticFluid } from '@/components/three/experimental';

<MagneticFluid
  particleCount={5000}
  mouseStrength={0.5}
  viscosity={0.98}
  color="#8B5CF6"
/>
```

---

## 📜 Scroll（7个）

### 1. ParallaxScroll（视差滚动）

**文件**: [parallax-scroll.tsx](../src/components/scroll/parallax-scroll.tsx)

多层视差滚动效果。

#### Props

```typescript
interface ParallaxScrollProps {
  children: ReactNode;
  className?: string;
  speed?: number;                 // 视差速度倍率，默认 0.5
  direction?: 'up' | 'down';     // 视差方向
}
```

#### 使用示例

```tsx
import { ParallaxScroll } from '@/src/components/scroll';

<section className="relative h-screen">
  {/* 背景层 - 慢速 */}
  <ParallaxScroll speed={0.2}>
    <img src="/bg.jpg" alt="" className="absolute inset-0 w-full h-full object-cover" />
  </ParallaxScroll>

  {/* 中间层 - 中速 */}
  <ParallaxScroll speed={0.5}>
    <div className="relative z-10 flex items-center justify-center h-full">
      <h1 className="text-6xl font-bold">视差滚动</h1>
    </div>
  </ParallaxScroll>

  {/* 前景层 - 快速 */}
  <ParallaxScroll speed={0.8}>
    <div className="absolute bottom-10 left-10 text-white">
      前景元素
    </div>
  </ParallaxScroll>
</section>
```

---

### 2. SectionReveal（区块揭示）

**文件**: [section-reveal.tsx](../src/components/scroll/section-reveal.tsx)

滚动到视口时的区块入场动画。

#### Props

```typescript
interface SectionRevealProps {
  children: ReactNode;
  className?: string;
  delay?: number;                 // 延迟时间（秒），默认 0
  duration?: number;              // 动画时长（秒），默认 0.6
  direction?: 'up' | 'down' | 'left' | 'right';  // 入场方向，默认 'up'
  threshold?: number;             // 触发阈值（0-1），默认 0.1
  once?: boolean;                 // 只触发一次，默认 true
}
```

#### 使用示例

```tsx
import { SectionReveal } from '@/src/components/scroll';

<SectionReveal direction="up" delay={0.2}>
  <h2>这个区块会从下方滑入</h2>
</SectionReveal>

<SectionReveal direction="left" delay={0.4}>
  <p>这个区块会从左侧滑入</p>
</SectionReveal>

<SectionReveal direction="scale" delay={0.6}>
  <img src="/image.jpg" alt="" />  {/* 缩放入场 */}
</SectionReveal>
```

---

### 3. GSAPScroll（GSAP 滚动动画）

**文件**: [gsap-scroll.tsx](../src/components/scroll/gsap-scroll.tsx)

基于 GSAP ScrollTrigger 的专业级滚动动画。

#### Props

```typescript
interface GSAPScrollProps {
  children: ReactNode;
  animations: Array<{
    target: string;               // 选择器
    animation: gsap.TweenVars;    // GSAP 动画配置
    scrollTrigger?: ScrollTrigger.Vars;  // ScrollTrigger 配置
  }>;
  className?: string;
}
```

#### 使用示例

```tsx
import { GSAPScroll } from '@/src/components/scroll';

<GSAPScroll
  animations={[
    {
      target: '.heading',
      animation: {
        y: -100,
        opacity: 0,
        duration: 1,
        ease: 'power3.out'
      },
      scrollTrigger: {
        trigger: '.section',
        start: 'top 80%',
        end: 'top 20%',
        scrub: 1
      }
    },
    {
      target: '.image',
      animation: {
        scale: 1.2,
        rotation: 5,
        duration: 1
      },
      scrollTrigger: {
        trigger: '.section',
        start: 'top center',
        scrub: true
      }
    }
  ]}
>
  <div className="section">
    <h2 className="heading">滚动时标题上移</h2>
    <img className="image" src="/photo.jpg" alt="" />
  </div>
</GSAPScroll>
```

---

### 4. 其他滚动组件

#### InfiniteScroll（无限滚动）
```tsx
import { InfiniteScroll } from '@/src/components/scroll';

<InfiniteScroll
  loadMore={() => fetchMoreData()}
  hasMore={hasMore}
  loader={<Spinner />}
>
  {items.map(item => (
    <div key={item.id}>{item.content}</div>
  ))}
</InfiniteScroll>
```

#### ScrollVideo（滚动驱动视频）
```tsx
import { ScrollVideo } from '@/src/components/scroll';

<ScrollVideo
  src="/video.mp4"
  sections={10}  // 将视频分为10段
  activeSection={currentSection}
/>
```

#### ScrollOrchestrator（滚动编排器）
用于协调多个滚动动画的时序：
```tsx
import { ScrollOrchestrator } from '@/src/components/scroll';

<ScrollOrchestrator
  timeline={[
    { at: 0, action: 'show-hero' },
    { at: 0.3, action: 'fade-in-features' },
    { at: 0.6, action: 'animate-stats' },
    { at: 0.9, action: 'show-cta' },
  ]}
/>
```

---

## ✨ Text（5个）

### 1. TextReveal（文字揭示）

**文件**: [text-reveal.tsx](../src/components/text/text-reveal.tsx)

逐字/逐行揭示的文字动画。

#### Props

```typescript
interface TextRevealProps {
  text: string;                    // 要显示的文字
  className?: string;
  delay?: number;                  // 延迟开始（秒）
  duration?: number;               // 动画时长（秒）
  by?: 'word' | 'char' | 'line';  // 揭示单位，默认 'word'
  easing?: string;                 // 缓动函数
  as?: 'h1' | 'h2' | 'h3' | 'p' | 'span';  // HTML 标签
}
```

#### 使用示例

```tsx
import { TextReveal } from '@/components/text';

<TextReveal
  text="Motion Primitives"
  by="word"
  delay={0.5}
  duration={0.8}
  as="h1"
  className="text-5xl font-bold"
/>

<TextReveal
  text="110+ 免费动画组件，让您的网站栩栩如生"
  by="line"
  delay={1.2}
  as="p"
  className="text-xl text-gray-600"
/>
```

---

### 2. GradientText（渐变文字）

**文件**: [gradient-text.tsx](../src/components/text/gradient-text.tsx)

带渐变色的文字效果。

#### Props

```typescript
interface GradientTextProps {
  text: string;
  from?: string;                   // 起始颜色，默认 "#8B5CF6"
  to?: string;                     // 结束颜色，默认 "#EC4899"
  angle?: number;                  // 渐变角度（度），默认 90
  animate?: boolean;               // 是否动画渐变，默认 false
  className?: string;
  as?: keyof JSX.IntrinsicElements;
}
```

#### 使用示例

```tsx
import { GradientText } from '@/components/text';

<GradientText
  text="Motion Primitives"
  from="#8B5CF6"
  to="#06B6D4"
  angle={135}
  animate
  as="h1"
  className="text-6xl font-bold"
/>
```

---

### 3. TextGenerate（打字机效果）

**文件**: [text-generate.tsx](../src/components/text/text-generate.tsx)

逐字打印的打字机效果。

#### Props

```typescript
interface TextGenerateProps {
  words: string;                   // 完整文本
  className?: string;
  speed?: number;                  // 打字速度（ms/字），默认 50
  cursor?: boolean;                // 显示闪烁光标，默认 true
  cursorChar?: string;             // 光标字符，默认 "|"
  onComplete?: () => void;         // 打字完成回调
  loop?: boolean;                  // 循环播放，默认 false
}
```

#### 使用示例

```tsx
import { TextGenerate } from '@/components/text';

<TextGenerate
  words="构建令人惊叹的网络体验..."
  speed={80}
  cursor
  loop
  onComplete={() => console.log('Done!')}
/>
```

---

### 4. SplitScreenText（分屏文字）

**文件**: [split-screen-text.tsx](../src/components/text/split-screen-text.tsx)

滚动时分屏切换的文字效果（类似电影片头）。

#### Props

```typescript
interface SplitScreenTextProps {
  lines: string[];                 // 文本行数组
  className?: string;
  bgColor?: string;                // 背景色
  textColor?: string;              // 文字颜色
  sticky?: boolean;                // 是否粘性定位
}
```

---

### 5. PremiumText（高级文字效果）

**文件**: [premium-text.tsx](../src/components/text/premium-text.tsx)

集合多种高级文字动画效果。

包含的效果：
- **Shimmer** - 微光扫过
- **Glitch** - 故障艺术
- **Wave** - 波浪起伏
- **Typewriter** - 打字机
- **FadeCascade** - 级联淡入

---

## 💫 Effects（12个）

### 1. MagneticButton（磁性按钮）

**文件**: [magnetic-button.tsx](../src/components/effects/magnetic-button.tsx)

吸引鼠标靠近的磁性效果按钮。

#### Props

```typescript
interface MagneticButtonProps {
  children: ReactNode;
  className?: string;
  strength?: number;               // 磁性强度，默认 0.3
  radius?: number;                 // 影响半径，默认 200
  as?: 'button' | 'a' | 'div';    // 渲染元素
  disabled?: boolean;
  onClick?: () => void;
}
```

#### 使用示例

```tsx
import { MagneticButton } from '@/components/effects';

<MagneticButton strength={0.5} radius={250}>
  <span>悬停试试</span>
</MagneticButton>
```

---

### 2. CustomCursor & FluidCursor（自定义光标）

**文件**: 
- [custom-cursor.tsx](../src/components/effects/custom-cursor.tsx)
- [fluid-cursor.tsx](../src/components/effects/fluid-cursor.tsx)

替换系统光标的自定义光标效果。

#### CustomCursor Props
```typescript
interface CustomCursorProps {
  color?: string;                  // 光标颜色
  size?: number;                   // 大小，默认 20
  mixBlendMode?: string;           // 混合模式，默认 'difference'
  hideNative?: boolean;            // 隐藏原生光标，默认 true
  className?: string;
}
```

#### FluidCursor Props
```typescript
interface FluidCursorProps {
  color?: string;
  size?: number;
  viscosity?: number;              // 粘性（延迟程度），默认 0.15
  particles?: number;              // 粒子数，默认 8
  className?: string;
}
```

#### 使用示例
```tsx
import { CustomCursor, FluidCursor } from '@/components/effects';

function MyApp() {
  return (
    <>
      {/* 简单圆形光标 */}
      <CustomCursor color="#8B5CF6" size={30} />

      {/* 流体光标（更炫酷但性能开销大） */}
      {/* <FluidCursor color="#00FFD1" viscosity={0.2} /> */}

      <main>
        {/* 应用内容 */}
      </main>
    </>
  );
}
```

---

### 3. MouseParallax（鼠标视差）

**文件**: [mouse-parallax.tsx](../src/components/effects/mouse-parallax.tsx)

基于鼠标位置的视差效果容器。

#### Props

```typescript
interface MouseParallaxProps {
  children: ReactNode;
  className?: string;
  strength?: number;               // 视差强度，默认 20（像素）
  easing?: number;                 // 缓动系数，默认 0.1
  disableOnMobile?: boolean;       // 移动端禁用，默认 true
}
```

#### 使用示例

```tsx
import { MouseParallax } from '@/components/effects';

<MouseParallax strength={30} easing={0.15}>
  <div className="relative">
    {/* 这些子元素会根据鼠标位置微移 */}
    <img src="/layer1.png" className="absolute" style={{ parallaxDepth: 0.2 }} />
    <img src="/layer2.png" className="absolute" style={{ parallaxDepth: 0.5 }} />
    <img src="/layer3.png" className="absolute" style={{ parallaxDepth: 0.8 }} />
  </div>
</MouseParallax>
```

---

### 4. ThemeToggle（主题切换）

**文件**: [theme-toggle.tsx](../src/components/effects/theme-toggle.tsx)

明暗模式切换按钮，带平滑过渡动画。

#### Props

```typescript
interface ThemeToggleProps {
  className?: string;
  size?: 'sm' | 'md' | 'lg';      // 尺寸，默认 'md'
  showLabel?: boolean;             // 显示文字标签，默认 false
  variant?: 'icon' | 'switch' | 'toggle';  // 样式变体
}
```

#### 使用示例
```tsx
import { ThemeToggle } from '@/components/effects';

<header>
  <nav>...</nav>
  <ThemeToggle variant="icon" size="lg" />
</header>
```

---

### 5. AudioReactive（音频响应）

**文件**: [audio-reactive.tsx](../src/components/effects/audio-reactive.tsx)

**实验性组件** - 对音频输入做出视觉反应。

需要用户授权麦克风权限。

#### Props
```typescript
interface AudioReactiveProps {
  children: ReactNode;
  className?: string;
  sensitivity?: number;            // 灵敏度，默认 1
  smoothing?: number;              // 平滑度，默认 0.8
  visualizer?: 'bars' | 'wave' | 'circular';  // 可视化类型
}
```

---

## 🎬 Transitions（3个）

### 1. Preloader（预加载器）

**文件**: [preloader.tsx](../src/components/transitions/preloader.tsx)

页面加载时的全屏过渡动画。

#### Props
```typescript
interface PreloaderProps {
  isLoading: boolean;              // 是否正在加载
  children: ReactNode;             // 加载完成后显示的内容
  logo?: ReactNode;                // 自定义 Logo
  duration?: number;               // 过渡动画时长（毫秒），默认 1500
  color?: string;                  // 主题色
  animation?: 'fade' | 'slide' | 'zoom' | 'flip';  // 动画类型
}
```

#### 使用示例
```tsx
import { Preloader } from '@/components/transitions';

function App() {
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    // 模拟加载
    const timer = setTimeout(() => setIsLoading(false), 2000);
    return () => clearTimeout(timer);
  }, []);

  return (
    <Preloader
      isLoading={isLoading}
      logo={<Logo />}
      animation="slide"
      color="#8B5CF6"
    >
      <MainContent />
    </Preloader>
  );
}
```

---

### 2. PageTransition（页面切换）

**文件**: [page-transition.tsx](../src/components/transitions/page-transition.tsx)

路由切换时的过渡动画。

#### Props
```typescript
interface PageTransitionProps {
  children: ReactNode;
  className?: string;
  mode?: 'wait' | 'simultaneous';  // 等待退出还是同时播放
  enterAnimation?: AnimationDefinition;
  exitAnimation?: AnimationDefinition;
}
```

---

### 3. NoiseTransition（噪声过渡）

**文件**: [noise-transition.tsx](../src/components/transitions/noise-transition.tsx)

使用噪声纹理的创意过渡效果。

---

## 📐 Layout（4个）

### 1. AnimatedMasonry（动画瀑布流）

**文件**: [animated-masonry.tsx](../src/components/layout/animated-masonry.tsx)

带入场动画的瀑布流布局。

#### Props
```typescript
interface AnimatedMasonryProps {
  columns?: number;                // 列数，默认 3
  gap?: number;                    // 间距（px），默认 16
  className?: string;
  animation?: 'fade-up' | 'scale' | 'none';  // 入场动画
  staggerDelay?: number;           // 交错延迟（秒），默认 0.1
}
```

#### 使用示例
```tsx
import { AnimatedMasonry } from '@/components/layout';

<AnimatedMasonry columns={4} gap={20} animation="fade-up">
  {images.map((img, i) => (
    <div key={i} style={{ height: img.height }}>
      <img src={img.src} alt="" />
    </div>
  ))}
</AnimatedMasonry>
```

---

### 2. HeroSections（英雄区布局）

**文件**: [hero-sections.tsx](../src/components/layout/hero-sections.tsx)

预构建的英雄区域布局变体。

包含：
- `CenteredHero` - 居中布局
- `SplitHero` - 左右分栏
- `FullWidthHero` - 全宽背景
- `ZigzagHero` - 之字形布局

---

### 3. PremiumLayouts（高级布局）

**文件**: [premium-layouts.tsx](../src/components/layout/premium-layouts.tsx)

完整的高级页面布局模板。

---

## 🧭 Navigation（2个）

### MorphingNav（形变导航）

**文件**: [morphing-nav.tsx](../src/components/navigation/morphing-nav.tsx)

形状动态变化的导航栏。

#### Props
```typescript
interface MorphingNavProps {
  links: Array<{
    href: string;
    label: string;
    icon?: ReactNode;
  }>;
  className?: string;
  variant?: 'underline' | 'pill' | 'background';  // 样式
  sticky?: boolean;                // 粘性定位
  blur?: boolean;                  // 背景模糊
}
```

#### 使用示例
```tsx
import { MorphingNav } from '@/components/navigation';

<MorphingNav
  links={[
    { href: '/', label: '首页' },
    { href: '/components', label: '组件' },
    { href: '/demos', label: '演示' },
    { href: '/docs', label: '文档' },
  ]}
  variant="pill"
  sticky
  blur
/>
```

---

## 🔍 SEO（12个）

完整的 SEO 工具包，大多数在 [layout.tsx](../src/app/layout.tsx) 中自动集成。

### 核心组件

#### 1. SEOHead（SEO 头部元数据）

**文件**: [seo-head.tsx](../src/components/seo/seo-head.tsx)

自动生成完整的 `<head>` SEO 元标签。

```tsx
import { SEOHead } from '@/components/seo';

<SEOHead
  title="页面标题"
  description="页面描述"
  canonical="https://example.com/page"
  ogImage="/og-image.png"
  keywords={['关键词1', '关键词2']}
/>
```

#### 2. JSON-LD（结构化数据）

**文件**: [json-ld.tsx](../src/components/seo/json-ld.tsx)

生成 JSON-LD 结构化数据（Schema.org）。

```tsx
import { JsonLd } from '@/components/seo';

<JsonLd
  type="Organization"
  data={{
    name: "Motion Primitives",
    url: "https://motion-primitives.dev",
    logo: "https://motion-primitives.dev/logo.png",
  }}
/>
```

支持的类型：
- `Organization`
- `WebSite`
- `Article`
- `BreadcrumbList`
- `FAQPage`
- `Product`

#### 3. SocialShare（社交分享）

**文件**: [social-share.tsx](../src/components/seo/social-share.tsx)

OpenGraph 和 Twitter Card 元标签。

```tsx
<SocialShare
  title="分享标题"
  description="分享描述"
  image="/share-image.jpg"
  twitterCreator="@yourhandle"
/>
```

#### 4. Analytics（分析代码）

**文件**: [analytics.tsx](../src/components/seo/analytics.tsx)

集成 Google Analytics、Plausible 等。

```tsx
<Analytics
  provider="google"
  id="G-XXXXXXXXXX"
/>
```

#### 5. WebVitals（性能监控）

**文件**: [web-vitals.tsx](../src/components/seo/web-vitals.tsx)

收集并上报 Core Web Vitals 数据。

```tsx
<WebVitals
  onReport={(metric) => {
    // 发送到分析服务
    analytics.track(metric.name, metric.value);
  }}
/>
```

#### 6. AutoSEO（自动 SEO）

**文件**: [auto-seo.tsx](../src/components/seo/auto-seo.tsx)

智能自动生成 SEO 元数据的包装组件。

```tsx
<AutoSEO
  pageTitle="关于我们"
  pageDescription="了解 Motion Primitives 的故事..."
  breadcrumbs={[
    { name: '首页', url: '/' },
    { name: '关于', url: '/about' },
  ]}
>
  <AboutPageContent />
</AutoSEO>
```

---

## 🪝 Hooks（6个）

### 1. useMousePosition（鼠标位置）

**文件**: [use-mouse-position.ts](../src/hooks/use-mouse-position.ts)

追踪鼠标相对于容器的位置。

#### 返回值
```typescript
interface MousePosition {
  x: number;                       // X 坐标
  y: number;                       // Y 坐标
  normalizedX: number;             // 归一化 X (-1 到 1)
  normalizedY: number;             // 归一化 Y (-1 到 1)
}
```

#### 使用示例
```tsx
import { useMousePosition } from '@/hooks';

function ParallaxImage() {
  const ref = useRef<HTMLDivElement>(null);
  const { x, y, normalizedX, normalizedY } = useMousePosition(ref);

  return (
    <div ref={ref} className="relative overflow-hidden">
      <img
        src="/image.jpg"
        style={{
          transform: `translate(${normalizedX * 20}px, ${normalizedY * 20}px)`
        }}
      />
    </div>
  );
}
```

---

### 2. useReducedMotion（减少动画偏好）

**文件**: [use-reduced-motion.ts](../src/hooks/use-reduced-motion.ts)

检测用户是否启用了"减少动画"无障碍设置。

#### 返回值
```typescript
const shouldReduceMotion: boolean;  // true 表示应该减少/禁用动画
```

#### 使用示例
```tsx
import { useReducedMotion } from '@/hooks';

function AnimatedComponent() {
  const reduceMotion = useReducedMotion();

  return (
    <motion.div
      animate={!reduceMotion ? { opacity: 1, y: 0 } : {}}
      initial={!reduceMotion ? { opacity: 0, y: 20 } : {}}
      transition={{ duration: reduceMotion ? 0 : 0.5 }}
    >
      内容
    </motion.div>
  );
}
```

**最佳实践**:
- 始终检查此 Hook 并尊重用户偏好
- 当 `reduceMotion=true` 时，立即显示最终状态
- 用于所有非必要的装饰性动画

---

### 3. useGsap（GSAP 封装）

**文件**: [use-gsap.ts](../src/hooks/use-gsap.ts)

便捷的 GSAP 动画 Hook，自动清理。

#### 使用示例
```tsx
import { useGsap } from '@/hooks';

function FadeInOnScroll({ children }) {
  const ref = useRef(null);
  const { contextSafe } = useGsap();

  useEffect(() => {
    const ctx = gsap.context(() => {
      gsap.fromTo(ref.current,
        { opacity: 0, y: 50 },
        {
          opacity: 1,
          y: 0,
          duration: 1,
          scrollTrigger: {
            trigger: ref.current,
            start: 'top 80%'
          }
        }
      );
    });

    return () => ctx.revert();  // 自动清理
  }, []);

  return <div ref={ref}>{children}</div>;
}
```

---

### 4. useLenis（平滑滚动）

**文件**: [use-lenis.ts](../src/hooks/use-lenis.ts)

初始化和配置 Lenis 平滑滚动实例。

#### 使用示例
```tsx
import { useLenis } from '@/hooks';

function App() {
  const lenis = useLenis({
    duration: 1.2,
    easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
    orientation: 'vertical',
    smoothWheel: true,
  });

  // 访问 lenis 实例
  useEffect(() => {
    lenis?.on('scroll', (e) => {
      console.log(e.velocity);
    });
  }, [lenis]);

  return <MainContent />;
}
```

---

### 5. useSvgAnimation（SVG 动画）

**文件**: [use-svg-animation.ts](../src/hooks/use-svg-animation.ts)

控制 SVG 路径动画（描边动画等）。

#### 使用示例
```tsx
import { useSvgAnimation } from '@/hooks';

function AnimatedIcon() {
  const pathRef = useRef<SVGPathElement>(null);
  const { play, pause, reset, progress } = useSvgAnimation(pathRef, {
    duration: 2,
    easing: 'easeInOut',
  });

  return (
    <svg viewBox="0 0 100 100">
      <path
        ref={pathRef}
        d="M10 10 L90 90"
        stroke="currentColor"
        strokeWidth={2}
        fill="none"
        strokeDasharray="100"
        strokeDashoffset="100"
      />
    </svg>
  );
}
```

---

### 6. useWebVitals（Web Vitals）

**文件**: [use-web-vitals.ts](../src/hooks/use-web-vitals.ts)

收集 Core Web Vitals 性能指标。

#### 监控指标
- **FCP** (First Contentful Paint) - 首次内容绘制
- **LCP** (Largest Contentful Paint) - 最大内容绘制
- **FID** (First Input Delay) - 首次输入延迟
- **CLS** (Cumulative Layout Shift) - 累积布局偏移
- **TTFB** (Time to First Byte) - 首字节时间

#### 使用示例
```tsx
import { useWebVitals } from '@/hooks';

function PerformanceMonitor() {
  useWebVitals((metric) => {
    // 发送到后端或分析服务
    fetch('/api/vitals', {
      method: 'POST',
      body: JSON.stringify(metric),
    });
    
    // 控制台输出（开发环境）
    if (process.env.NODE_ENV === 'development') {
      console.log(`[Web Vital] ${metric.name}: ${metric.value}`);
    }
  });

  return null;  // 不渲染任何 UI
}
```

---

## 🎨 主题系统集成

### 使用 ThemeProvider

项目通过 [ThemeProvider](../src/lib/theme.tsx) 提供4种设计方向：

```tsx
import { ThemeProvider } from '@/lib/theme';

function App({ children }) {
  return (
    <ThemeProvider
      defaultDirection="freestyle"  // luxury | cyberpunk | kinetic | freestyle
      defaultMode="dark"           // light | dark
    >
      {children}
    </ThemeProvider>
  );
}
```

### 在组件中使用主题

```tsx
'use client';

import { useTheme } from '@/lib/theme';

function ThemedComponent() {
  const { direction, mode, setDirection, toggleMode } = useTheme();

  return (
    <div className={`${direction === 'cyberpunk' ? 'text-cyan-400' : ''}`}>
      当前方向: {direction}
      <button onClick={toggleMode}>
        切换{mode === 'dark' ? '亮色' : '暗色'}模式
      </button>
      <button onClick={() => setDirection('luxury')}>
        切换到 Luxury
      </button>
    </div>
  );
}
```

### Tailwind 方向变体

在组件中根据方向应用不同样式：

```tsx
<div className="
  luxury:bg-zinc-900 luxury:text-yellow-500
  cyberpunk:bg-black cyberpunk:text-cyan-400
  kinetic:bg-white kinetic:text-purple-600
  freestyle:bg-orange-50 freestyle:text-orange-900
">
  内容会根据当前方向自动变化
</div>
```

---

## ⚡ 性能优化建议

### 1. 按需导入

不要导入整个组件库，只导入需要的组件：

```tsx
✅ 好的做法
import { Dock } from '@/components/interactive/dock';

❌ 不推荐
import * as Interactive from '@/components/interactive';
```

### 2. Three.js 组件优化

- 使用 `React.memo` 包裹 3D 组件
- 限制粒子数量（移动端 < 1000）
- 使用 `useMemo` 缓存几何体计算
- 考虑使用 `Suspense` 和 `lazy` 加载：

```tsx
import { lazy, Suspense } from 'react';
const Globe = lazy(() => import('@/components/three/globe'));

<Suspense fallback={<LoadingSpinner />}>
  <Globe />
</Suspense>
```

### 3. 动画性能

- 优先使用 `transform` 和 `opacity`（GPU 加速）
- 使用 `will-change` 提示浏览器优化
- 尊重 `prefers-reduced-motion`
- 复杂动画考虑降低帧率（`requestAnimationFrame` 节流）

### 4. 滚动性能

- 使用 `IntersectionObserver` 替代滚动事件监听
- GSAP ScrollTrigger 已内置优化
- Lenis 默认已优化滚动性能

---

## 🐛 常见问题与解决方案

### Q1: Three.js 组件不显示？

**可能原因**:
1. 未在 Canvas 内使用
2. 缺少灯光（Three.js 场景需要光源）
3. 相机位置不当

**解决方案**:
```tsx
import { Canvas } from '@react-three/fiber';

<Canvas camera={{ position: [0, 0, 5] }}>
  <ambientLight intensity={0.5} />
  <pointLight position={[10, 10, 10]} />
  <Globe />
</Canvas>
```

### Q2: Framer Motion 动画不流畅？

**解决方案**:
1. 确保 `transform` 属性被使用（而非 `top/left`）
2. 减少同时动画的元素数量
3. 使用 `transition` 的 `ease` 优化曲线
4. 检查是否有阻塞主线程的重计算

### Q3: 移动端性能差？

**优化策略**:
1. 减少/禁用 3D 效果
2. 降低粒子数量
3. 使用 `matchMedia` 检测设备
4. 启用 `reduceMotion` 模式

```tsx
const isMobile = typeof window !== 'undefined'
  ? window.matchMedia('(max-width: 768px)').matches
  : false;

{!isMobile && <Globe />}
```

### Q4: 如何自定义组件样式？

所有组件都支持 `className` prop，可以使用 Tailwind 类覆盖：

```tsx
<Dock className="!bg-red-500 !rounded-lg">
  {/* 子组件 */}
</Dock>
```

对于深度定制，考虑 fork 组件并修改源码。

---

## 📚 扩展阅读

- [Framer Motion 文档](https://www.framer.com/motion/)
- [GSAP 文档](https://greensock.com/docs/)
- [Three.js 文档](https://threejs.org/docs/)
- [React Three Fiber 文档](https://docs.pmnd.rs/react-three-fiber)
- [Lenis 文档](https://lenis.studioforty9.com/)
- [Tailwind CSS 文档](https://tailwindcss.com/docs)

---

## 📝 更新日志

- **2026-10-22**: 初始版本
  - 完成 115 个组件的 API 文档
  - 添加 6 个 Hooks 详细说明
  - 提供丰富的使用示例
  - 性能优化指南
  - 常见问题解答

---

*本文档与 PROJECT_STRUCTURE.md 配合使用，形成完整的项目文档体系*