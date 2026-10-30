# Motion Primitives - 性能优化手册

> 针对动画密集型项目的专项性能调优指南，确保 60FPS 流畅体验

---

## 📋 目录

1. [性能目标](#-性能目标)
2. [性能监控工具](#-性能监控工具)
3. [渲染性能优化](#-渲染性能优化)
4. [动画性能优化](#-动画性能优化)
5. [Three.js 性能优化](#-threejs-性能优化)
6. [字体与资源优化](#-字体与资源优化)
7. [打包体积优化](#-打包体积优化)
8. [Core Web Vitals 优化](#-core-web-vitals-优化)
9. [各方向性能特点](#-各方向性能特点)
10. [性能检查清单](#-性能检查清单)

---

## 🎯 性能目标

### 关键指标

| 指标 | 目标值 | 说明 |
| ------ | -------- | ------ |
| **FPS (帧率)** | ≥ 60 FPS | 动画流畅度 |
| **FCP** | < 1.8s | 首次内容绘制 |
| **LCP** | < 2.5s | 最大内容绘制 |
| **FID** | < 100ms | 首次输入延迟 |
| **CLS** | < 0.1 | 累积布局偏移 |
| **TTI** | < 3.8s | 可交互时间 |
| **Bundle Size** | < 300KB (gzipped) | 首屏 JS 大小 |

### 性能预算

```
┌─────────────────────────────────────────────────────┐
│              Motion Primitives Performance Budget    │
├─────────────────────────────────────────────────────┤
│                                                     │
│  Total Bundle:     < 300KB gzipped                 │
│  ├─ React Core:     ~40KB                         │
│  ├─ Framer Motion:   ~35KB                        │
│  ├─ Three.js:       < 100KB (code-split)          │
│  └─ App Code:       < 125KB                       │
│                                                     │
│  First Paint:      < 1.0s                          │
│  Content Paint:    < 1.8s                          │
│  Interactive:      < 3.8s                          │
│                                                     │
│  Runtime:                                           │
│  ├─ JS Execution:  < 100ms per frame               │
│  ├─ Layout Thrash: 0 times                         │
│  └─ Long Tasks:     < 50ms                         │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---

## 🔍 性能监控工具

### 浏览器内置工具

#### 1. Chrome DevTools Performance

**打开方式**: `F12` → `Performance` 标签

**使用步骤**:

```bash
1. 点击 "Record" (或按 Ctrl+E)
2. 在页面上操作（滚动、点击、悬停）
3. 点击 "Stop"
4. 分析结果
```

**关键指标查看**:

- **Frames** - 查看是否有掉帧（红色帧）
- **Main** - 查看 JavaScript 执行时间
- **Rendering** - 查看样式计算和布局开销
- **Painting** - 查看绘制时间

**示例分析**:

```
问题: Main 线程长时间阻塞
原因: 组件在每次渲染时都创建新对象
解决: 使用 useMemo/useCallback 缓存
```

#### 2. Chrome DevTools Rendering

**打开方式**: `F12` → `Rendering` 标签（可能在 "More tools" 中）

**有用选项**:

- ✅ **Paint flashing** - 高亮重绘区域（绿色闪烁）
- ✅ **Layer borders** - 显示合成层边界（橙色边框）
- ✅ **FPS meter** - 实时 FPS 计数器
- ✅ **Scrolling performance issues** - 滚动性能警告

#### 3. Lighthouse

**运行方式**: `F12` → `Lighthouse` 标签 → "Generate report"

**关注分数**:

- **Performance** > 90
- **Accessibility** > 95
- **Best Practices** > 90
- **SEO** > 90

### 专业工具

#### 1. React DevTools Profiler

**安装**: React Developer Tools 浏览器扩展

**使用**:

```tsx
// 在开发模式下自动可用
// 点击 React 图标 → Profiler 标签
// 点击 Record → 操作页面 → Stop

// 查看组件渲染次数和耗时
// 找到不必要的 re-render
```

**优化提示**:

```
⚠️ Component rendered unnecessarily
   Why did this render?
   Props changed: { onClick }
   Solution: Wrap onClick in useCallback
```

#### 2. Web Vitals 库

项目中已集成：

```tsx
import { useWebVitals } from '@/hooks';

function PerformanceMonitor() {
  useWebVitals((metric) => {
    // 发送到分析服务
    if (process.env.NODE_ENV === 'development') {
      console.log(`[Web Vital] ${metric.name}: ${metric.value}ms`);
    }

    // 上报到后端
    fetch('/api/vitals', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(metric),
    });
  });

  return null;
}
```

#### 3. Bundle Analyzer

**运行命令**:

```bash
pnpm analyze
# 或
ANALYZE=true pnpm build
```

**输出**: 可视化的 bundle 组成图，找出大依赖。

---

## ⚡ 渲染性能优化

### 理解浏览器渲染管线

```
JavaScript ──→ Style ──→ Layout ──→ Paint ──→ Composite
   │            │         │         │          │
   │            │         │         │          └── GPU 合成（快）
   │            │         │         └── 像素填充（较慢）
   │            │         └── 几何计算（慢）
   │            └── CSS 计算（较快）
   └── 代码执行
```

**目标**: 尽量在早期阶段完成工作，避免触发昂贵的 Layout 和 Paint。

### 1. 避免 Layout Thrashing（布局抖动）

**❌ 错误做法**: 强制同步布局

```tsx
function BadComponent() {
  const ref = useRef<HTMLDivElement>(null);

  const handleClick = () {
    // 读取（触发 layout）
    const width = ref.current.offsetWidth;

    // 写入（使上一次读取的缓存失效）
    ref.current.style.width = `${width + 10}px`;

    // 再次读取（再次触发 layout！）
    const newWidth = ref.current.offsetWidth; // 💥 Layout Thrash!
  };

  return <div ref={ref} onClick={handleClick} />;
}
```

**✅ 正确做法**: 批量读写

```tsx
function GoodComponent() {
  const ref = useRef<HTMLDivElement>(null);

  const handleClick = () {
    // 先批量读取
    const width = ref.current.offsetWidth;
    const height = ref.current.offsetHeight;

    // 再批量写入
    requestAnimationFrame(() => {
      ref.current.style.width = `${width + 10}px`;
      ref.current.style.height = `${height + 10}px`;
    });
  };

  return <div ref={ref} onClick={handleClick} />;
}
```

### 2. 使用 GPU 加速属性

**GPU 友好属性**（不触发 Layout/Paint）:

- `transform`
- `opacity`

**避免使用的属性**（会触发 Layout 或 Paint）:

- `width`, `height`, `top`, `left`, `right`, `bottom`
- `margin`, `padding`
- `border-width`, `outline-width`
- `font-size`, `line-height`

**示例对比**:

```css
/* ❌ 差 - 触发 Layout */
.animated-element {
  animation: move-bad 1s infinite;
}

@keyframes move-bad {
  from { left: 0; }
  to { left: 100px; }
}

/* ✅ 好 - 只使用 transform */
.animated-element {
  animation: move-good 1s infinite;
  will-change: transform;
}

@keyframes move-good {
  from { transform: translateX(0); }
  to { transform: translateX(100px); }
}
```

### 3. 使用 will-change 提示

**作用**: 提前告知浏览器该元素会变化，浏览器可以提前优化。

**使用场景**:

```css
/* 对于频繁动画的元素 */
.dock-item {
  will-change: transform, opacity;
}

.globe-canvas {
  will-change: transform;
}

.particle-system {
  will-change: transform, opacity;
}
```

**⚠️ 注意事项**:

- 不要过度使用（会占用过多内存）
- 只在确实需要时添加
- 动画结束后移除：`will-change: auto`

**错误示例**:

```css
/* ❌ 不要对所有元素使用 */
* {
  will-change: transform; /* 内存爆炸！ */
}

/* ✅ 正确用法 */
.is-animating {
  will-change: transform;
}

.animation-complete {
  will-change: auto; /* 重置 */
}
```

### 4. 减少 DOM 节点数量

**问题**: 过多的 DOM 节点会增加样式计算和布局的时间。

**解决方案**:

```tsx
// ❌ 差 - 生成大量节点
function BadParticles({ count = 1000 }) {
  return (
    <div>
      {Array.from({ length: count }).map((_, i) => (
        <div
          key={i}
          className="particle"
          style={{
            left: Math.random() * 100 + '%',
            top: Math.random() * 100 + '%',
          }}
        />
      ))}
    </div>
  );
}

// ✅ 好 - 使用 Canvas/WebGL
function GoodParticles({ count = 1000 }) {
  const canvasRef = useRef<HTMLCanvasElement>(null);

  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    const particles = Array.from({ length: count }, () => ({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      // ...
    }));

    function animate() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      particles.forEach(p => {
        ctx.beginPath();
        ctx.arc(p.x, p.y, 2, 0, Math.PI * 2);
        ctx.fill();
      });
      requestAnimationFrame(animate);
    }

    animate();
  }, []);

  return <canvas ref={canvasRef} />;
}
```

### 5. 使用 Content Visibility

**CSS 属性**: `content-visibility`

**作用**: 跳过屏幕外内容的渲染工作。

```css
/* 对于长列表或复杂区块 */
.section {
  content-visibility: auto;
  contain-intrinsic-size: 0 500px; /* 预估高度 */
}

/* 对于首屏以下的内容 */
.below-fold {
  content-visibility: auto;
}
```

**适用场景**:

- 长列表/表格
- 复杂的卡片网格
- Tab 内容（非活动 tab）
- 折叠/展开内容

---

## 🎬 动画性能优化

### Framer Motion 优化

#### 1. 使用正确的动画属性

```tsx
// ✅ 优先使用这些属性（GPU 加速）
<motion.div
  animate={{
    x: 100,           // transform: translateX
    scale: 1.2,        // transform: scale
    rotate: 45,        // transform: rotate
    opacity: 1,        // opacity
  }}
/>

// ❌ 避免这些属性（触发 Layout）
<motion.div
  animate={{
    width: '100%',     // 触发 layout
    height: '200px',   // 触发 layout
    top: 50,           // 触发 layout
    margin: 20,        // 触发 layout
  }}
/>
```

#### 2. 使用 transform 模板

Framer Motion 的 `transform` 属性自动合并为单个 `transform` 字符串：

```tsx
// ✅ Framer Motion 自动优化
<motion.div
  animate={{
    x: 100,
    y: 50,
    scale: 1.2,
    rotate: 15,
  }}
/>
// 生成: transform: translateX(100px) translateY(50px) scale(1.2) rotate(15deg)
// 只触发一次 composite！
```

#### 3. 合理使用 Animation Controls

```tsx
// ✅ 使用 useAnimation 控制复杂动画序列
function ComplexAnimation() {
  const controls = useAnimation();

  useEffect(() => {
    // 定义动画序列
    const sequence = async () => {
      await controls.start({ opacity: 0, y: -20 });
      await controls.start({ opacity: 1, y: 0, transition: { duration: 0.5 } });
      await controls.start({ scale: 1.05, transition: { yoyo: Infinity, duration: 2 } });
    };
    sequence();
  }, [controls]);

  return <motion.div animate={controls} />;
}
```

#### 4. 减少不必要的 re-render

```tsx
// ❌ 差 - 每次父组件更新都重新创建 props
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <button onClick={() => setCount(c => c + 1)}>
        Count: {count}
      </button>
      {/* 每次 count 变化，onClick 都是新函数 */}
      <Child onClick={() => console.log('click')} />
    </>
  );
}

// ✅ 好 - 缓存回调
function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log('click');
  }, []); // 空依赖，只创建一次

  return (
    <>
      <button onClick={() => setCount(c => c + 1)}>Count: {count}</button>
      <Child onClick={handleClick} />
    </>
  );
}

// 或者使用 React.memo
const Child = React.memo(function Child({ onClick }) {
  return <button onClick={onClick}>Click me</button>;
});
```

#### 5. 使用 Lazy/Motion 延迟加载

```tsx
// 对于不在首屏的复杂动画组件
import { lazy, Suspense } from 'react';

const HeavyAnimation = lazy(() =>
  import('./HeavyAnimation').then(mod => ({ default: mod.HeavyAnimation }))
);

function Page() {
  return (
    <main>
      {/* 首屏内容 */}

      {/* 延迟加载 */}
      <Suspense fallback={<LoadingSpinner />}>
        <HeavyAnimation />
      </Suspense>
    </main>
  );
}
```

### GSAP 优化

#### 1. 使用 ScrollTrigger 的合理配置

```tsx
// ✅ 好的 ScrollTrigger 配置
useGSAP(() => {
  gsap.from('.element', {
    y: 100,
    opacity: 0,
    duration: 1,
    scrollTrigger: {
      trigger: '.element',
      start: 'top 80%',     // 元素进入视口 80% 时开始
      end: 'top 20%',       // 元素到达顶部 20% 时结束
      toggleActions: 'play none none reverse', // 优化：只在需要时播放
      // scrub: true 会更耗性能，只在需要平滑绑定时使用
    },
  });
});
```

#### 2. 批量动画操作

```tsx
// ❌ 差 - 多次独立调用
gsap.to('.item1', { x: 100 });
gsap.to('.item2', { x: 100 });
gsap.to('.item3', { x: 100 });

// ✅ 好 - 批量操作
gsap.to(['.item1', '.item2', '.item3'], {
  x: 100,
  stagger: 0.1,  // 交错动画
});
```

#### 3. 使用 Timeline 组织复杂动画

```tsx
// ✅ 使用 Timeline 自动优化
const tl = gsap.timeline({
  defaults: {
    ease: 'power2.out',
    duration: 0.8,
  },
});

tl.from('.heading', { y: 50, opacity: 0 })
  .from('.subheading', { y: 30, opacity: 0 }, '-=0.5') // 重叠 0.5s
  .from('.cta-button', { scale: 0.8, opacity: 0 }, '-=0.3');
```

---

## 🌐 Three.js 性能优化

Three.js 是性能消耗最大的部分，需要特别注意！

### 1. 减少多边形数量

```tsx
// ❌ 差 - 高精度几何体
<mesh>
  <sphereGeometry args={[2, 64, 64]} /> {/* 64x64 = 4096 个三角形 */}
</mesh>

// ✅ 好 - 适度精度
<mesh>
  <sphereGeometry args={[2, 32, 32]} /> {/* 32x32 = 1024 个三角形 */}
</mesh>

// ✅ 更好 - 低精度（远距离物体）
<mesh>
  <sphereGeometry args={[2, 16, 16]} /> {/* 16x16 = 256 个三角形 */}
</mesh>
```

### 2. 使用 InstancedMesh（实例化渲染）

对于大量相同几何体（粒子、树木等）：

```tsx
import { useMemo } from 'react';
import { useFrame } from '@react-three/fiber';
import * as THREE from 'three';

function InstancedParticles({ count = 1000 }) {
  const meshRef = useRef();

  const dummy = useMemo(() => new THREE.Object3D(), []);

  const particles = useMemo(() => {
    return Array.from({ length: count }, () => ({
      position: [
        (Math.random() - 0.5) * 10,
        (Math.random() - 0.5) * 10,
        (Math.random() - 0.5) * 10,
      ],
      speed: Math.random() * 0.01 + 0.005,
    }));
  }, [count]);

  useFrame((state) => {
    particles.forEach((particle, i) => {
      const { position, speed } = particle;

      // 更新位置
      position[1] += speed;
      if (position[1] > 5) position[1] = -5;

      // 应用变换
      dummy.position.set(...position);
      dummy.updateMatrix();
      meshRef.current.setMatrixAt(i, dummy.matrix);
    });

    meshRef.current.instanceMatrix.needsUpdate = true;
  });

  return (
    <instancedMesh ref={meshRef} args={[null, null, count]}>
      <sphereGeometry args={[0.05, 8, 8]} />
      <meshBasicMaterial color="#8B5CF6" />
    </instancedMesh>
  );
}

// 性能提升: 1000 个独立 Mesh → 1 个 InstancedMesh
// 渲染调用从 1000 次降到 1 次！
```

### 3. 使用 LOD（Level of Detail）

根据距离动态调整模型精度：

```tsx
import { LOD, useFrame } from '@react-three/fiber';

function AdaptiveGlobe({ position }) {
  const lodRef = useRef();

  return (
    <LOD ref={lodRef} position={position}>
      {/* 近距离：高精度 */}
      <mesh levelDistance={0}>
        <sphereGeometry args={[2, 64, 64]} />
        <meshStandardMaterial color="#8B5CF6" wireframe />
      </mesh>

      {/* 中距离：中精度 */}
      <mesh levelDistance={10}>
        <sphereGeometry args={[2, 32, 32]} />
        <meshStandardMaterial color="#8B5CF6" wireframe />
      </mesh>

      {/* 远距离：低精度 */}
      <mesh levelDistance={20}>
        <sphereGeometry args={[2, 16, 16]} />
        <meshStandardMaterial color="#8B5CF6" wireframe />
      </mesh>

      {/* 很远：极低精度 */}
      <mesh levelDistance={40}>
        <sphereGeometry args={[2, 8, 8]} />
        <meshBasicMaterial color="#8B5CF6" />
      </mesh>
    </LOD>
  );
}
```

### 4. 优化材质和纹理

```tsx
// ❌ 差 - 高分辨率纹理
<meshStandardMaterial map={highResTexture} />

// ✅ 好 - 压缩纹理
// 使用 .ktx2 格式（GPU 压缩）
// 或使用低分辨率 + detail texture

// ✅ 更好 - 简单材质
<meshBasicMaterial color="#8B5CF6" wireframe /> {/* 最轻量 */}

// ✅ 中等 - 标准材质但减少计算
<meshStandardMaterial
  color="#8B5CF6"
  roughness={0.5}
  metalness={0.1}
  // 避免使用：
  // - normalMap （法线贴图 - 昂贵）
  // - emissiveMap （自发光贴图）
  // - environmentMap （环境贴图 - 除非必要）
/>
```

### 5. 控制 Render Loop

```tsx
// ❌ 差 - 始终渲染（即使不可见）
function AlwaysRendering() {
  useFrame((state) => {
    // 每帧都执行...
  });
}

// ✅ 好 - 条件渲染
function SmartRendering({ isActive }) {
  useFrame((state) => {
    if (!isActive) return; // 跳过计算

    // 只在活跃时执行...
  });
}

// ✅ 更好 - 使用 frustumCulling
<Canvas>
  {/* R3F 默认启用视锥体剔除 */}
  <Globe position={[0, 0, -100]} /> {/* 不在视野内时不渲染 */}
</Canvas>
```

### 6. 使用 Suspense 和 Lazy 加载 3D 组件

```tsx
import { lazy, Suspense } from 'react';

// 延迟加载重型 3D 组件
const Globe = lazy(() => import('@/components/three/globe'));
const NeuralNetwork = lazy(() => import('@/components/three/neural-network'));

function Page() {
  return (
    <main>
      {/* 非关键内容延迟加载 */}
      <Suspense fallback={<CanvasLoadingSpinner />}>
        <Canvas camera={{ position: [0, 0, 5] }}>
          <ambientLight intensity={0.5} />
          <Globe />
        </Canvas>
      </Suspense>

      {/* 其他 3D 场景 */}
      <section>
        <h2>Neural Network</h2>
        <Suspense fallback={<div className="h-96 bg-muted animate-pulse" />}>
          <NeuralNetwork nodeCount={50} />
        </Suspense>
      </section>
    </main>
  );
}
```

### 7. 移动端降级策略

```tsx
import { useState, useEffect } from 'react';

function Responsive3DScene() {
  const [isMobile, setIsMobile] = useState(false);
  const [isLowPower, setIsLowPower] = useState(false);

  useEffect(() => {
    // 检测设备类型
    setIsMobile(window.innerWidth < 768);

    // 检测硬件并发数（近似判断性能）
    const cores = navigator.hardwareConcurrency || 4;
    setIsLowPower(cores <= 4 || isMobile);
  }, []);

  if (isLowPower) {
    // 降级方案：使用 CSS/SVG 替代
    return <CSSFallbackAnimation />;
  }

  if (isMobile) {
    // 移动端：降低质量
    return (
      <Canvas>
        <Globe
          radius={1.5}           {/* 更小 */}
          variant="wireframe"   {/* 更轻量 */}
          rotationSpeed={0}     {/* 禁用自转 */}
        />
      </Canvas>
    );
  }

  // 桌面端：完整质量
  return (
    <Canvas>
      <Globe
        radius={2}
        variant="dotted"
        rotationSpeed={0.001}
      />
    </Canvas>
  );
}
```

---

## 🔤 字体与资源优化

### 字体优化策略

#### 1. 使用 font-display: swap

项目已配置（见 [fonts.ts](../src/lib/fonts.ts)）：

```typescript
export const luxuryHeading = Space_Grotesk({
  display: "swap",  // ✅ 已配置
  subsets: ["latin"],
});
```

**效果**：

- 字体加载期间显示系统字体
- 字体就绪后无缝切换（无 FOIT - Flash Of Invisible Text）

#### 2. 子集化字体（Subset Fonts）

只包含需要的字符：

```typescript
export const headingFont = Inter({
  subsets: ["latin"],  // ✅ 只包含拉丁字符
  // 不包含 cyrillic, greek 等（除非需要）
});
```

#### 3. 使用可变字体（Variable Fonts）

可变字体文件更小（一个文件替代多个字重）：

```typescript
// ✅ 可变字体（推荐）
export const myFont = Inter({
  variable: "--font-my",
  weight: ["100", "200", "300", "400", "500", "600", "700", "800", "900"],
});

// ❌ 多个静态字体文件（更大）
// Inter-Thin.woff2
// Inter-Regular.woff2
// Inter-Bold.woff2
// ... 总共 9 个文件
```

#### 4. 预加载关键字体

在 `layout.tsx` 中：

```tsx
export default function RootLayout({ children }) {
  return (
    <html>
      <head>
        {/* 预加载字体文件 */}
        <link
          rel="preload"
          href="/fonts/space-grotesk-var.woff2"
          as="font"
          type="font/woff2"
          crossOrigin="anonymous"
        />
      </head>
      <body>{children}</body>
    </html>
  );
}
```

### 图片优化

#### 1. 使用 Next.js Image 组件

```tsx
import Image from 'next/image';

// ✅ 好的做法
<Image
  src="/hero.jpg"
  alt="Hero image"
  width={1200}
  height={630}
  priority  // 首屏图片标记为优先加载
  placeholder="blur"  // 模糊占位
  blurDataURL={blurDataUrl}  // 极小的模糊图片
  sizes="(max-width: 768px) 100vw, 50vw"  // 响应式尺寸
/>

// ❌ 差的做法
<img src="/hero.jpg" alt="Hero image" width="1200" height="630" />
```

#### 2. 使用现代图片格式

Next.js Image 自动优化：

- 自动转换为 WebP/AVIF（如果浏览器支持）
- 响应式 srcset
- 懒加载（非首屏图片）

#### 3. 3D 资源优化

```tsx
// 对于 Spline / GLTF 模型
// 使用 Draco 压缩
// 降低纹理分辨率（最大 1024x1024）
// 使用 .glb 格式（二进制，比 .gltf 更小）
```

---

## 📦 打包体积优化

### 1. 分析当前 Bundle

```bash
# 运行 bundle analyzer
pnpm analyze
```

**查看报告**:

- 找出最大的依赖
- 识别重复的代码
- 发现可以 tree-shaking 的未使用代码

### 2. Tree Shaking 优化

**确保 ES Module 导入**:

```js
// ✅ 支持 tree-shaking（ESM）
import { animate, motion } from 'framer-motion';

// ❌ 不支持 tree-shaking（CommonJS 或整体导入）
const framer = require('framer-motion');
const _ = require('lodash');  // 整个 lodash 都被打包！
```

**对于 lodash**:

```bash
# 安装 lodash-es（ESM 版本）
pnpm add lodash-es

# 按需导入
import debounce from 'lodash-es/debounce';  // 只打包 debounce
```

### 3. 代码分割（Code Splitting）

Next.js 自动进行路由级代码分割，手动控制：

```tsx
// 动态导入大型组件
const HeavyChart = dynamic(() => import('./HeavyChart'), {
  loading: () => <ChartSkeleton />,
  ssr: false,  // 客户端 only 组件
});

const ThreeScene = dynamic(() => import('./ThreeScene'), {
  loading: () => <CanvasSkeleton />,
});
```

### 4. 外部化大依赖

对于不经常变化的库，可以使用 CDN：

```js
// next.config.js
module.exports = {
  // 外部化 Three.js（从 CDN 加载）
  externals: {
    three: 'three',
  },
};
```

然后在 `_document.tsx` 中引入 CDN：

```tsx
<script
  src="https://unpkg.com/three@0.164.0/build/three.min.js"
  defer
></script>
```

⚠️ **注意**: 这会影响 SSR，谨慎使用。

---

## 📊 Core Web Vitals 优化

### 1. LCP（Largest Contentful Paint）优化

**影响因素**:

- 首屏大图/视频
- 文本块（标题、段落）
- 背景图片

**优化策略**:

```tsx
// ✅ 1. 预加载关键资源
<link rel="preload" as="image" href="/hero-image.webp" />

// ✅ 2. 优化首屏图片
<Image
  src="/hero.webp"
  alt="Hero"
  priority  // 关键！告诉 Next.js 这是 LCP 候选
  quality={85}  // 平衡质量和大小
/>

// ✅ 3. 避免 LCP 元素延迟加载
// 不要懒加载首屏的关键内容！

// ❌ 4. 避免首屏有大型 3D 场景
// 将 3D 内容放到首屏下方
```

### 2. FID（First Input Delay）优化

**目标**: 用户首次交互时响应迅速 (< 100ms)

**优化策略**:

```tsx
// ✅ 1. 减少主线程阻塞
// - 分割长任务
// - 使用 requestIdleCallback
// - Web Workers 处理复杂计算

// ✅ 2. 延迟非关键 JavaScript
// 第三方脚本使用 defer/async
// 分析代码异步加载

// ✅ 3. 优化事件监听器
// 使用 passive: true（对于滚动/触摸事件）
element.addEventListener('scroll', handler, { passive: true });
```

### 3. CLS（Cumulative Layout Shift）优化

**目标**: 视觉稳定性 (< 0.1)

**常见原因及解决方案**:

```tsx
// ❌ 原因 1: 图片没有设置尺寸
<img src="/photo.jpg" />
// 解决: 设置 width/height 或使用 aspect-ratio

// ✅ 方案 A: Next.js Image（自动处理）
<Image src="/photo.jpg" width={800} height={600} />

// ✅ 方案 B: CSS aspect-ratio
<div style={{ aspectRatio: '4/3', background: '#eee' }}>
  <img src="/photo.jpg" style={{ width: '100%', height: '100%', objectFit: 'cover' }} />
</div>

// ❌ 原因 2: 动态注入内容
// 广告、动态列表、字体加载

// ✅ 为字体加载预留空间
.heading {
  font-family: var(--font-heading);
  min-height: 1em;  // 预留空间
}

// ❌ 原因 3: 动画导致的位移
// 使用 transform 而不是 top/left

// ✅ 使用 transform
<motion.div initial={{ opacity: 0, y: 20 }} animate={{ opacity: 1, y: 0 }}>
  Content
</motion.div>
```

---

## 🧭 各方向性能特点

### Luxury（奢华）

**性能特征**:

- ✅ **最轻量** - 极简设计，少装饰
- ✅ **动画缓慢** - 1200ms 过渡，CPU 占用低
- ✅ **无复杂效果** - 无发光、模糊等昂贵操作
- ⚠️ **大量留白** - 可能导致页面较长

**优化建议**:

- 几乎无需特殊优化
- 关注图片压缩（高质量图片是重点）
- 字体加载优化（细字体重视觉影响大）

**预期性能**: ⭐⭐⭐⭐⭐ (优秀)

---

### Cyberpunk（赛博朋克）

**性能特征**:

- ⚠️ **扫描线效果** - 可能使用伪元素/CSS 动画
- ⚠️ **发光效果** - `box-shadow` / `text-shadow` / `filter: blur()`
- ⚠️ **Glitch 动画** - 多层文本叠加
- ✅ **快速过渡** - 150ms，不会长时间占用主线程

**优化建议**:

```css
/* 优化 Cyberpunk 发光效果 */
.cyberpunk-glow {
  /* ❌ 差: filter: blur() 非常昂贵 */
  filter: blur(10px);

  /* ✅ 好: 使用 box-shadow 模拟（GPU 加速） */
  box-shadow: 0 0 20px rgba(0, 255, 209, 0.5),
              0 0 40px rgba(0, 255, 209, 0.3),
              0 0 60px rgba(0, 255, 209, 0.1);

  /* ✅ 更好: 使用 will-change */
  will-change: box-shadow, transform;
}

/* 优化扫描线 */
.scanlines::after {
  content: '';
  position: absolute;
  inset: 0;
  background: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 2px,
    rgba(0, 255, 209, 0.03) 2px,
    rgba(0, 255, 209, 0.03) 4px
  );
  pointer-events: none;  /* 不影响交互 */
  will-change: transform;  /* 如果动画的话 */
}
```

**预期性能**: ⭐⭐⭐⭐ (良好)

---

### Kinetic（动能）

**性能特征**:

- ⚠️ **弹性动画** - cubic-bezier overshoot 可能触发额外帧
- ⚠️ **大量圆角** - border-radius 有一定开销
- ⚠️ **阴影效果** - 多层 box-shadow
- ✅ **中等过渡时长** - 400ms，平衡

**优化建议**:

```tsx
// 优化 Kinetic 弹性动画
<motion.button
  whileHover={{ scale: 1.05 }}
  whileTap={{ scale: 0.95 }}
  transition={{
    type: "spring",
    stiffness: 400,      // 弹性刚度
    damping: 17,         // 阻尼（越小越弹）
    mass: 1,             // 质量
  }}
>
  Click me
</motion.button>

// 性能调优:
// - stiffness 越高，动画越快，但可能看起来僵硬
// - damping 越高，振荡越少，越快稳定
// - 对于高频交互（如 Dock），适当降低 stiffness
```

**预期性能**: ⭐⭐⭐⭐ (良好)

---

### Freestyle（自由风格）

**性能特征**:

- ⚠️ **不规则形状** - clip-path, mix-blend-mode
- ⚠️ **渐变背景** - conic-gradient, radial-gradient
- ⚠️ **混合模式** - blend-mode 计算昂贵
- ⚠️ **倾斜/旋转** - transform 但可能触发重绘
- ✅ **戏剧性过渡** - 600ms，不太频繁

**优化建议**:

```css
/* 优化 Freestyle 混合模式 */
.freestyle-card {
  /* ❌ 差: mix-blend-mode 对性能影响大 */
  mix-blend-mode: multiply;

  /* ✅ 好: 使用半透明颜色模拟 */
  background: rgba(255, 107, 53, 0.8);
}

/* 优化不规则裁剪 */
.freestyle-shape {
  /* clip-path 本身性能还行 */
  /* 但配合动画时要小心 */
  clip-path: polygon(0 0, 100% 0, 100% calc(100% - 20px), 20px 100%, 0 100%);

  /* 动画时使用 transform 而不是 clip-path */
  transition: transform 600ms cubic-bezier(0.22, 1, 0.36, 1);
  /* 不要动画 clip-path！ */
}

/* 优化渐变背景 */
.freestyle-bg {
  /* ❌ 差: 大面积动画渐变 */
  background: linear-gradient(45deg, #ff6b35, #f7931e, #ffcc02);
  background-size: 400% 400%;
  animation: gradient 8s linear infinite;  /* 每帧重绘！ */

  /* ✅ 好: 静态渐变 + 小范围动画 */
  background: linear-gradient(45deg, #ff6b35, #f7931e);
  /* 或者使用伪元素做小范围动画 */
}
```

**预期性能**: ⭐⭐⭐ (中等)

---

## ✅ 性能检查清单

### 开发阶段

- [ ] **安装性能监控**
  - [ ] React DevTools Profiler
  - [ ] Chrome DevTools Performance
  - [ ] Lighthouse CI（可选）

- [ ] **代码审查**
  - [ ] 避免在循环/渲染中创建新对象/函数
  - [ ] 使用 `useMemo` / `useCallback` 缓存昂贵计算
  - [ ] 检查是否有不必要的 re-render（React DevTools）

- [ ] **动画审查**
  - [ ] 所有动画使用 `transform` 和 `opacity`
  - [ ] 避免动画 `width` / `height` / `top` / `left`
  - [ ] 合理使用 `will-change`（不过度）
  - [ ] 尊重 `prefers-reduced-motion`

- [ ] **3D 组件审查**
  - [ ] Three.js 组件使用 `lazy` + `Suspense` 加载
  - [ ] 几何体精度适中（32 段左右）
  - [ ] 移动端提供降级方案
  - [ ] 使用 `InstancedMesh` 处理大量相同对象

### 构建阶段

- [ ] **Bundle 分析**
  - [ ] 运行 `pnpm analyze`
  - [ ] 检查包大小是否超预算
  - [ ] 识别可优化的依赖

- [ ] **代码分割**
  - [ ] 路由级分割（Next.js 自动）
  - [ ] 大型组件动态导入
  - [ ] 第三方库按需导入

- [ ] **资源优化**
  - [ ] 图片使用 Next.js Image
  - [ ] 字体已子集化和预加载
  - [ ] CSS 已 purge（Tailwind JIT）

### 部署阶段

- [ ] **生产环境测试**
  - [ ] Lighthouse 分数 > 90
  - [ ] Web Vitals 达标（FCP < 1.8s, LCP < 2.5s, FID < 100ms, CLS < 0.1）
  - [ ] 3G/4G 网络下测试

- [ ] **真实用户监控 (RUM)**
  - [ ] 集成 Web Vitals 收集
  - [ ] 设置性能预算告警
  - [ ] 监控 P95/P99 指标

### 持续监控

- [ ] **性能回归检测**
  - [ ] CI/CD 中集成 Lighthouse CI
  - [ ] Bundle size 变更告警
  - [ ] 性能指标趋势追踪

- [ ] **用户反馈**
  - [ ] 监控"页面卡顿"反馈
  - [ ] 分析低分页面的共同点
  - [ ] 定期回顾和优化

---

## 🛠️ 性能优化速查表

### 快速修复常见问题

| 问题 | 原因 | 解决方案 | 影响 |
| ------ | ------ | --------- | ------ |
| **页面加载慢** | Bundle 太大 | 代码分割、Tree Shaking | ⭐⭐⭐⭐⭐ |
| **动画卡顿** | 使用了非 GPU 属性 | 改用 transform/opacity | ⭐⭐⭐⭐⭐ |
| **3D 掉帧** | 几何体太复杂 | 降低精度、InstancedMesh | ⭐⭐⭐⭐ |
| **首屏白屏** | 字体/JS 阻塞 | preload、优先加载 | ⭐⭐⭐⭐ |
| **布局跳动** | 图片/字体未预留空间 | 设置尺寸、aspect-ratio | ⭐⭐⭐ |
| **交互延迟** | 主线程阻塞 | Web Worker、任务分割 | ⭐⭐⭐ |
| **内存泄漏** | 事件监听器未清理 | useEffect cleanup | ⭐⭐⭐ |
| **移动端发热** | 3D/动画太多 | 降级方案、减少粒子 | ⭐⭐⭐ |

### 性能工具箱

```bash
# 开发工具
pnpm dev                    # 开发服务器（支持 HMR）
pnpm analyze                # Bundle 分析
pnpm build                  # 生产构建

# Lighthouse CLI
npx lighthouse https://yoursite.com --output html --output-path ./report

# WebPageTest（在线工具）
# https://www.webpagetest.org/

# Bundlephobia（检查包大小）
# https://bundlephobia.com/
```

---

## 📈 性能基准测试

### 目标设备性能分级

| 设备类型 | 典型设备 | 预期 FPS | 限制 |
| --------- | --------- | ---------- | ------ |
| **高端桌面** | M1/M2/M3 Mac, RTX 3080+ | 60+ | 无 |
| **中端桌面** | Intel i5/i7, GTX 1660 | 60 | 复杂 3D 场景 |
| **低端桌面** | 老旧 PC, 集显 | 30-60 | 禁用 3D |
| **高端手机** | iPhone 14/15 Pro | 60 | 限制粒子数 |
| **中端手机** | iPhone 12, Pixel 6 | 30-60 | 禁用实验性 3D |
| **低端手机** | Android 低端, 旧 iPhone | 30 | 仅 CSS 动画 |

### 各方向在不同设备上的表现

| 方向 | 高端桌面 | 中端手机 | 低端手机 |
| ------ | --------- | --------- | --------- |
| **Luxury** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Cyberpunk** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Kinetic** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Freestyle** | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |

---

## 🎓 进阶主题

### 1. OffscreenCanvas（离屏 Canvas）

将 Canvas 渲染移到 Web Worker：

```tsx
// 主线程
const offscreen = canvas.transferControlToOffscreen();
worker.postMessage({ canvas: offscreen }, [offscreen]);

// Worker 线程
self.onmessage = (e) => {
  const gl = e.data.canvas.getContext('webgl');
  // 在 Worker 中渲染 3D 场景
};
```

**优势**: 完全不阻塞主线程

**兼容性**: 现代浏览器支持

### 2. WebGL 2.0 特性

如果使用 raw Three.js（非 R3F）：

```javascript
// 使用 instanced rendering
const geometry = new.InstancedBufferGeometry();
const material = new.RawShaderMaterial({
  vertexShader: `...`,
  fragmentShader: `...`,
});

// 使用 Transform Feedback（GPU 粒子计算）
const transformFeedback = new.TransformFeedback();
```

### 3. WebGPU（未来）

下一代图形 API，性能更好：

```javascript
// 目前实验性支持
if ('gpu' in navigator) {
  const adapter = await navigator.gpu.requestAdapter();
  const device = await adapter.requestDevice();
  // 使用 WebGPU 渲染
}
```

---

## 📚 参考资源

### 官方文档

- **[Web.dev - Performance]** (<https://web.dev/performance/>) - Google 性能指南
- **[MDN - Web Performance]** (<https://developer.mozilla.org/en-US/docs/Web/Performance>) - MDN 性力文档
- **[Next.js - Optimization]** (<https://nextjs.org/docs/app/building-your-application/optimizing>) - Next.js 优化

### 工具

- **[Chrome DevTools]** (<https://developer.chrome.com/docs/devtools/>) - 开发者工具
- **[Lighthouse]** (<https://developers.google.com/web/tools/lighthouse>) - 性能审计
- **[WebPageTest]** (<https://www.webpagetest.org/>) - 在线性能测试
- **[Bundlephobia]** (<https://bundlephobia.com/>) - 包大小分析

### 深入阅读

- **[Rendering Performance]** (<https://web.dev/rendering-performance/>) - 渲染性能深度解析
- **[Optimize Core Web Vitals]** (<https://web.dev/vitals/>) - Core Web Vitals 优化
- **[Three.js Performance]** (<https://threejs.org/examples/#webgl_performance>) - Three.js 性能示例

---

## 🔄 版本历史

- **2026-10-22**: 初始版本
  - 完整的性能优化体系
  - 覆盖渲染、动画、3D、字体、打包
  - 四种方向的性能特点分析
  - 实用的检查清单和速查表

---

*最后更新: 2026-10-22*
*预计阅读时间: 30分钟*
*难度等级: 🟠 中高级（需要一定的性能优化经验）*
*相关文档: [PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md) | [DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md)*
