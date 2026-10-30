# Motion Primitives - 设计系统详解

> 深入理解四方向设计系统的完整规范：颜色、字体、间距、动画、组件变体

---

## 📋 目录

1. [设计系统概览](#-设计系统概览)
2. [四种设计方向](#-四种设计方向)
3. [颜色系统](#-颜色系统)
4. [字体系统](#-字体系统)
5. [间距与布局](#-间距与布局)
6. [圆角与形状](#-圆角与形状)
7. [动画与过渡](#-动画与过渡)
8. [组件样式变体](#-组件样式变体)
9. [CSS 变量参考表](#-css-变量参考表)
10. [使用指南](#-使用指南)

---

## 🎨 设计系统概览

### 核心理念

Motion Primitives 的设计系统围绕**四个独特的设计方向**构建，每个方向代表一种完整的视觉语言：

```
┌─────────────────────────────────────────────────────────────┐
│                    Motion Primitives Design System           │
├─────────────┬─────────────┬─────────────┬───────────────────┤
│   LUXURY    │  CYBERPUNK  │   KINETIC   │    FREESTYLE      │
│             │             │             │                   │
│  极简优雅   │  赛博朋克   │  动感活力   │  大胆创新         │
│  Apple风格  │  黑客帝国   │  Stripe风   │  打破常规         │
│             │             │             │                   │
│ 金色强调    │ 青色霓虹    │ 紫色活力    │ 橙色热情          │
│ 几何无衬线  │ 等宽终端    │ 圆润友好    │ 粗犷艺术          │
│ 大量留白    │ 密集信息    │ 充足内边距  │ 不对称布局        │
│ 缓慢优雅    │ 即时响应    │ 弹性动感    │ 戏剧性过渡        │
└─────────────┴─────────────┴─────────────┴───────────────────┘
```

### 技术实现

设计系统通过 **CSS 自定义属性（CSS Variables）** 实现，运行时可动态切换：

```html
<!-- HTML 元素上的方向标识 -->
<html data-direction="luxury" class="dark">
<html data-direction="cyberpunk" class="dark">
<html data-direction="kinetic" class="">
<html data-direction="freestyle" class="dark">
```

**优势**：

- ✅ **零重编译** - 切换方向无需重新构建
- ✅ **运行时动态** - JavaScript 控制切换，支持用户偏好持久化
- ✅ **类型安全** - TypeScript 完整类型定义
- ✅ **Tailwind 集成** - 通过 variants 自动应用

---

## 🧭 四种设计方向

### 1. Luxury（奢华）

**设计哲学**: "少即是多" - 极致的克制和精致

**灵感来源**:

- Apple 产品设计
- Linear 应用界面
- 高端手表品牌（Rolex, Patek Philippe）
- 奢侈品网站（Hermès, Celine）

#### 视觉特征

| 属性 | 值 | 说明 |
| ------ | ----- | ------ |
| **调色板** | 黑/白/金 | 单色调 + 金色点缀 |
| **字体** | Space Grotesk + Inter | 几何无衬线 |
| **圆角** | 0px (完全直角) | 锐利、精确 |
| **留白** | 极多 | section: 16rem |
| **字重** | 极细 (100-200) | 优雅、轻盈 |
| **字间距** | 宽松 | heading: 0.15em |
| **过渡** | 缓慢 (1200ms) | cubic-bezier(0.16, 1, 0.3, 1) |

#### 适用场景

✅ **适合**:

- 高端品牌展示
- 作品集/案例展示
- 金融/法律/咨询行业
- 艺术/文化/时尚网站

❌ **不适合**:

- 需要大量信息的仪表盘
- 游戏/娱乐类应用
- 儿童产品
- 快速操作的工具型应用

#### 代码示例

```tsx
// 强制使用 Luxury 方向（用于特定区块）
<div data-direction="luxury">
  <h1 className="text-display font-heading font-thin tracking-[0.15em] uppercase">
    EXQUISITE CRAFTSMANSHIP
  </h1>
  <p className="text-body-lg font-light tracking-[0.04em] text-muted-foreground">
    Precision engineered for those who demand perfection.
  </p>
</div>

// 使用 Tailwind variant
<h2 className="luxury:text-gold luxury:font-thin luxury:tracking-widest">
  Luxury Heading
</h2>
```

---

### 2. Cyberpunk（赛博朋克）

**设计哲学**: "未来已来" - 科技感和反乌托邦美学

**灵感来源**:

- 《银翼杀手》电影 UI
- 《攻壳机动队》界面
- 终端/Hacker 工具
- HUD（平显）设计

#### 视觉特征

| 属性 | 值 | 说明 |
| ------ | ----- | ------ |
| **调色板** | 深黑 + 青色/品红 | 高对比度霓虹 |
| **字体** | JetBrains Mono + IBM Plex Mono | 等宽字体 |
| **圆角** | 0px (完全直角) | 工业、硬朗 |
| **留白** | 紧凑 | section: 6rem |
| **字重** | 粗 (600-700) | 强烈、技术感 |
| **字间距** | 中等 | heading: 0.08em |
| **过渡** | 极快 (150ms) | cubic-bezier(0.76, 0, 0.24, 1) |

#### 视觉元素

```css
/* Cyberpunk 特有的视觉效果 */
[data-direction="cyberpunk"] {
  /* 扫描线效果 */
  background-image: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 2px,
    rgba(0, 255, 209, 0.03) 2px,
    rgba(0, 255, 209, 0.03) 4px
  );

  /* 发光文字 */
  text-shadow: 0 0 10px rgba(0, 255, 209, 0.5);

  /* 故障效果（Glitch）*/
  animation: glitch 0.3s infinite;
}
```

#### 适用场景

✅ **适合**:

- 游戏/元宇宙应用
- 科技/加密货币/区块链
- 开发者工具/IDE
- 音乐/电子音乐平台
- 科幻主题项目

❌ **不适合**:

- 传统企业网站
- 医疗/健康类应用
- 需要长时间阅读的内容
- 温馨/亲和力要求高的场景

---

### 3. Kinetic（动能）

**设计哲学**: "一切都在动" - Stripe 级别的运动设计

**灵感来源**:

- Stripe 网站
- Linear 应用
- Vercel 文档
- Notion 动画

#### 视觉特征

| 属性 | 值 | 说明 |
| ------ | ----- | ------ |
| **调色板** | 白底 + 紫/青渐变 | 清新、现代 |
| **字体** | Outfit + DM Sans | 圆润无衬线 |
| **圆角** | 大圆角 (12-16px) | 友好、柔和 |
| **留白** | 充足 | section: 10rem |
| **字重** | 中等 (500-700) | 平衡、易读 |
| **字间距** | 正常 | heading: -0.02em |
| **过渡** | 弹性 (400ms) | cubic-bezier(0.34, 1.56, 0.64, 1) |

#### 动画特点

Kinetic 方向的核心理念是**有意义的动效**：

```tsx
// 按钮悬停 - 弹性缩放
<motion.button
  whileHover={{ scale: 1.05 }}
  whileTap={{ scale: 0.95 }}
  transition={{ type: "spring", stiffness: 400, damping: 17 }}
>

// 卡片入场 - 弹跳进入
<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{
    type: "spring",
    stiffness: 300,
    damping: 20,
    delay: 0.1 * index
  }}
>

// 数字变化 - 弹性计数
<AnimatedCounter
  value={1234}
  easing={(t) => 1 - Math.pow(1 - t, 3) // ease-out cubic
/>
```

#### 适用场景

✅ **适合**:

- SaaS 产品着陆页
- 现代 Web 应用
- 创业公司/初创企业
- 设计师/创意机构
- 需要展示交互能力的演示

❌ **不适合**:

- 需要严肃/正式感的场景
 政府/金融机构（可能显得不够稳重）
- 内容密集型网站（动画可能分散注意力）
- 性能受限的低端设备

---

### 4. Freestyle（自由风格）

**设计哲学**: "规则就是用来打破的" - 大胆、意外、艺术化

**灵感来源**:

- Awwwards 获奖网站
- 实验性网页设计
- 艺术家个人网站
- 音乐/时尚/娱乐行业

#### 视觉特征

| 属性 | 值 | 说明 |
| ------ | ----- | ------ |
| **调色板** | 暖灰 + 橙色 | 温暖、有机 |
| **字体** | Syne + Manrope | 独特、表现力强 |
| **圆角** | 混合 (0-24px) | 不规则、有趣 |
| **留白** | 变化 | 根据内容调整 |
| **字重** | 粗 (700-800) | 强烈、自信 |
| **字间距** | 紧凑 | heading: -0.03em |
| **过渡** | 戏剧性 (600ms) | cubic-bezier(0.22, 1, 0.36, 1) |

#### 设计原则

Freestyle 鼓励**打破常规**：

```tsx
// 不对称布局
<div className="grid grid-cols-12 gap-4">
  <div className="col-span-7 col-start-3"> {/* 偏移 */}
    <h2 className="text-8xl font-black leading-none -rotate-2">
      BREAK THE RULES
    </h2>
  </div>
</div>

// 混合圆角
<button className="rounded-tl-none rounded-tr-3xl rounded-bl-3xl rounded-br-none">
  Irregular Button
</button>

// 大胆的颜色组合
<div className="bg-orange-500 text-blue-900 p-12 -skew-x-3">
  Unexpected Color Combo
</div>
```

#### 适用场景

✅ **适合**:

- 创意作品集
- 活动/展览/发布会
- 音乐人/艺术家/设计师
- 品牌 campaign 页面
- 实验性项目

❌ **不适合**:

- 企业内部工具
- 电子商务（可能影响转化率）
- 文档/知识库
- 需要高度一致性的大型应用

---

## 🎨 颜色系统

### HSL 格式说明

所有颜色都使用 **HSL（色相、饱和度、亮度）** 格式存储为 CSS 变量：

```css
/* 格式: H S L% */
--primary: 263 70% 50%;
/*        ↑   ↑     ↑
       色相 饱和度 亮度 */

/* 使用时通过 hsl() 函数转换 */
color: hsl(var(--primary));
background-color: hsl(var(--background));
```

**为什么选择 HSL？**

- ✅ 直观调整（改变亮度 = 改变最后一个值）
- ✅ 易于生成明暗模式变体
- ✅ 便于创建色彩系统

### 语义化颜色 Token

每个方向定义了一套**语义化的颜色 token**：

```css
/* ===== 基础颜色 ===== */
--background: H S L%;          /* 页面背景 */
--foreground: H S L%;          /* 主要文字 */
--border: H S L%;              /* 边框、分割线 */
--surface: H S L%;             /* 卡片/面板背景 */

/* ===== 语义颜色 ===== */
--primary: H S L%;             /* 主色调（按钮、链接） */
--primary-foreground: H S L%;  /* 主色调上的文字 */
--secondary: H S L%;           /* 次要颜色 */
--secondary-foreground: H S L%;/* 次要颜色上的文字 */
--muted: H S L%;               /* 弱化背景 */
--muted-foreground: H S L%;    /* 弱化文字（说明、提示） */
--accent: H S L%;              /* 强调色（高光、标签） */
--accent-foreground: H S L%;   /* 强调色上的文字 */
--glow: H S L%;                /* 发光效果 */
--selection: H S L%;           /* 文本选中颜色 */
```

### 四种方向的完整调色板

#### Luxury 调色板

```css
/* Light Mode */
[data-direction="luxury"] {
  --background: 40 20% 98%;        /* 暖白色 (#FAF8F5） */
  --foreground: 30 5% 8%;          /* 近黑色 (#141210） */
  --primary: 30 5% 8%;             /* 黑色 */
  --secondary: 40 10% 95%;         /* 浅灰 */
  --accent: 40 60% 52%;            /* 金色 (#C9A84C） */
  --border: 40 8% 90%;             /* 浅边框 */
  --surface: 40 15% 97%;           /* 卡片背景 */
}

/* Dark Mode */
[data-direction="luxury"].dark {
  --background: 30 8% 2%;          /* 深黑 (#050504） */
  --foreground: 40 12% 88%;        /* 米白 (#E0D8CC） */
  --accent: 40 55% 55%;            /* 暗金色 */
}
```

**Luxury 配色方案图示**:

```
Light Mode:
┌─────────────────────────────────────┐
│ ░░░░░░ Background (暖白) ░░░░░░░░░ │
│ ┌─────────────────────────────────┐ │
│ │ Surface (略深)                  │ │
│ │ ┌─ Primary Text (黑) ─────────┐ │ │
│ │ │ Accent (金色) ●              │ │ │
│ │ │ Muted Text (灰)              │ │ │
│ │ └──────────────────────────────┘ │ │
│ └─────────────────────────────────┘ │
│ Border (浅灰线)                     │
└─────────────────────────────────────┘
```

---

#### Cyberpunk 调色板

```css
/* Dark Mode (主要模式） */
[data-direction="cyberpunk"].dark {
  --background: 230 20% 2%;         /* 深空黑 (#050A14） */
  --foreground: 180 90% 88%;        /* 亮青色 (#E3FFFF） */
  --primary: 174 100% 50%;          /* 霓虹青 (#00FFD1） */
  --secondary: 320 85% 60%;         /* 品红 (#E040A0） */
  --accent: 320 85% 60%;            /* 品红 */
  --border: 174 50% 18%;            /* 暗青边框 */
  --surface: 230 20% 5%;            /* 深表面 */
  --glow: 174 100% 50%;             /* 青色发光 */
}

/* Light Mode */
[data-direction="cyberpunk"] {
  --background: 230 15% 4%;         /* 深蓝黑 (#0A1628） */
  --foreground: 180 80% 85%;        /* 青色 */
  /* ...其他类似但稍亮 */
}
```

**Cyberpunk 配色方案图示**:

```
Dark Mode:
┌─────────────────────────────────────┐
█▓▓▓▓ Background (深空黑) ▓▓▓▓▓▓▓▓▓█
█▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓█
█▓▓ Surface (更深) ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓█
█▓▓▓ Primary (霓虹青) █▓▓▓▓▓▓▓▓▓▓▓█
█▓▓ Secondary (品红) █▓▓▓▓▓▓▓▓▓▓▓▓█
█▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓█
█ Border (暗青发光) ██████████████████
└─────────────────────────────────────┘
  ↑ Glow effect on borders and primary elements
```

---

#### Kinetic 调色板

```css
/* Light Mode (主要模式） */
[data-direction="kinetic"] {
  --background: 0 0% 100%;          /* 纯白 (#FFFFFF） */
  --foreground: 240 10% 12%;        /* 深蓝黑 (#1E1F2C） */
  --primary: 263 70% 50%;           /* 紫色 (#7C3AED） */
  --secondary: 240 5% 96%;          /* 浅灰 */
  --accent: 172 66% 50%;            /* 青绿色 (#14B8A6） */
  --border: 240 5% 90%;             /* 浅边框 */
  --surface: 240 5% 98%;            /* 近白表面 */
}

/* Dark Mode */
[data-direction="kinetic"].dark {
  --background: 240 10% 4%;         /* 深蓝黑 (#0A0B14） */
  --foreground: 0 0% 98%;           /* 近白 */
  --primary: 263 70% 65%;           /* 亮紫色 */
  --accent: 172 66% 60%;            /* 亮青绿 */
}
```

**Kinetic 配色特点**:

- 明亮、清新、现代
- 紫色+青绿的互补配色
- 高对比度但不刺眼
- 适合长时间阅读

---

#### Freestyle 调色板

```css
/* Dark Mode (主要模式） */
[data-direction="freestyle"].dark {
  --background: 20 15% 6%;          /* 暖灰黑 (#0D0F14） */
  --foreground: 30 20% 92%;         /* 暖白 (#EBE9E1） */
  --primary: 24 95% 51%;            /* 橙色 (#FF6B35） */
  --secondary: 30 10% 20%;          /* 暗灰 */
  --accent: 45 90% 55%;             /* 黄橙色 */
  --border: 30 10% 18%;             /* 暗边框 */
  --surface: 20 12% 10%;            /* 暖深表面 */
  --glow: 24 95% 51%;               /* 橙色发光 */
}

/* Light Mode */
[data-direction="freestyle"] {
  --background: 36 33% 94%;         /* 暖白 (#F0EBE0） */
  --foreground: 24 10% 10%;         /* 近黑 */
  --primary: 24 95% 51%;            /* 橙色保持 */
  /* ... */
}
```

**Freestyle 配色特点**:

- 温暖、有机、人性化
- 橙色作为主色调（充满能量）
- 不同于常见的蓝紫色调
- 传达创造力和激情

---

## 🔤 字体系统

### 字体加载策略

项目使用 **Next.js Font Optimization** 自动优化字体加载：

```tsx
// src/lib/fonts.ts
import { Space_Grotesk, Inter, JetBrains_Mono, ... } from "next/font/google";

export const luxuryHeading = Space_Grotesk({
  subsets: ["latin"],
  variable: "--font-luxury-heading",  // CSS 变量名
  display: "swap",                      // 避免 FOIT/FOUT
  weight: ["300", "400", "500", "600", "700"],
});
```

**`display: "swap"` 的作用**：

- 字体加载期间显示系统字体（无闪烁不可见文本）
- 字体加载完成后无缝替换
- 提升感知性能

### 四组字体配对

| 方向 | 标题字体 | 正文字体 | 特点 |
| ------ | --------- | --------- | ------ |
| **Luxury** | Space Grotesk | Inter | 几何、精密、现代 |
| **Cyberpunk** | JetBrains Mono | IBM Plex Mono | 等宽、技术、终端感 |
| **Kinetic** | Outfit | DM Sans | 圆润、友好、易读 |
| **Freestyle** | Syne | Manrope | 独特、表现力、艺术感 |

### 字体变量映射

```css
/* 在 globals.css 中定义 */
[data-direction="luxury"] {
  --font-heading: var(--font-luxury-heading, 'Space Grotesk');
  --font-body: var(--font-luxury-body, 'Inter');
}

/* 在 tailwind.config.ts 中使用 */
fontFamily: {
  heading: ["var(--font-heading)", "system-ui"],
  body: ["var(--font-body)", "system-ui"],
  "luxury-heading": ["var(--font-luxury-heading)", "Space Grotesk"],
  /* ... 其他方向 */
}
```

### 排版尺度（Fluid Typography）

使用 `clamp()` 实现响应式字体，无需媒体查询：

```css
/* Tailwind config 中的定义 */
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

**实际效果**:

| 断点 | Display | Heading-1 | Body |
| ------ | --------- | ----------- | ------ |
| 375px (手机) | 3rem (48px) | 2rem (32px) | 1rem (16px) |
| 768px (平板) | 4.5rem (72px) | 2.75rem (44px) | 1rem (16px) |
| 1280px (桌面) | 6rem (96px) | 3.5rem (56px) | 1rem (16px) |
| 1920px (大屏) | 7.5rem (120px) | 4rem (64px) | 1rem (16px) |

### 各方向的排版 DNA

每个方向都有独特的排版参数：

```css
/* === Luxury === */
[data-direction="luxury"] {
  --heading-weight: 100;           /* 极细 */
  --heading-tracking: 0.15em;      /* 宽字距 */
  --heading-transform: uppercase;  /* 全大写 */
  --body-weight: 200;              /* 细体 */
  --body-tracking: 0.04em;         /* 宽字距 */
}

/* === Cyberpunk === */
[data-direction="cyberpunk"] {
  --heading-weight: 700;           /* 粗体 */
  --heading-tracking: 0.08em;
  --heading-transform: uppercase;
  --body-weight: 400;              /* 正常 */
  --body-tracking: 0.03em;
}

/* === Kinetic === */
[data-direction="kinetic"] {
  --heading-weight: 700;
  --heading-tracking: -0.02em;     /* 紧凑 */
  --heading-transform: none;       /* 正常大小写 */
  --body-weight: 500;
  --body-tracking: 0;
}

/* === Freestyle === */
[data-direction="freestyle"] {
  --heading-weight: 800;           /* 特粗 */
  --heading-tracking: -0.03em;     /* 更紧凑 */
  --heading-transform: none;
  --body-weight: 600;
  --body-tracking: -0.01em;
}
```

---

## 📏 间距与布局

### 间距系统

基于 **4px 网格**的间距系统：

```css
/* Tailwind 默认间距 + 自定义扩展 */
spacing: {
  '0': '0',
  'px': '1px',
  '0.5': '0.125rem',  /* 2px */
  '1': '0.25rem',      /* 4px */
  '2': '0.5rem',       /* 8px */
  '3': '0.75rem',      /* 12px */
  '1': '1rem',         /* 16px */
  '5': '1.25rem',      /* 20px */
  '6': '1.5rem',       /* 24px */
  '8': '2rem',         /* 32px */
  '10': '2.5rem',      /* 40px */
  '12': '3rem',        /* 48px */
  '16': '4rem',        /* 64px */
  '18': '4.5rem',      /* 72px (自定义） */
  '20': '5rem',        /* 80px */
  '22': '5.5rem',      /* 88px (自定义） */
  '24': '6rem',        /* 96px */
  '30': '7.5rem',      /* 120px (自定义） */
  '32': '8rem',        /* 128px */
  '...': '...'
}
```

### 各方向的布局 DNA

```css
/* === Luxury: 大量留白 === */
[data-direction="luxury"] {
  --space-section: 16rem;    /* 区块间距 256px */
  --space-block: 6rem;       /* 内部块间距 96px */
  --container-max: 900px;    /* 最大宽度窄 */
}

/* === Cyberpunk: 紧凑密集 === */
[data-direction="cyberpunk"] {
  --space-section: 6rem;     /* 区块间距 96px */
  --space-block: 2rem;       /* 内部块间距 32px */
  --container-max: 1400px;   /* 最大宽度宽 */
}

/* === Kinetic: 充足呼吸空间 === */
[data-direction="kinetic"] {
  --space-section: 10rem;    /* 区块间距 160px */
  --space-block: 4rem;       /* 内部块间距 64px */
  --container-max: 1200px;
}

/* === Freestyle: 灵活多变 === */
[data-direction="freestyle"] {
  --space-section: 8rem;     /* 基准值 */
  --space-block: 3rem;
  --container-max: 1300px;
  /* 实际使用时会根据内容调整 */
}
```

### 容器宽度对比

```
Luxury (900px):
┌─────────────────────┐
│                     │
│   窄而精致          │
│   专注内容          │
│                     │
└─────────────────────┘

Cyberpunk (1400px):
┌──────────────────────────────────────────┐
│                                          │
│   宽屏展示                               │
│   信息密度高                             │
│                                          │
└──────────────────────────────────────────┘

Kinetic (1200px):
┌────────────────────────────────────┐
│                                    │
│   平衡的宽度                       │
│   适合大多数内容                   │
│                                    │
└────────────────────────────────────┘
```

---

## ⭕ 圆角与形状

### 各方向的圆角系统

```css
/* === Luxury: 完全直角 === */
[data-direction="luxury"] {
  --radius-sm: 0px;
  --radius-md: 0px;
  --radius-lg: 0px;
  --radius-full: 0px;
  --button-radius: 0px;
  --card-radius: 0px;
}

/* === Cyberpunk: 直角（工业感）=== */
[data-direction="cyberpunk"] {
  --radius-sm: 0px;
  --radius-md: 0px;
  --radius-lg: 0px;
  --radius-full: 0px;
  --button-radius: 0px;
  --card-radius: 0px;
}

/* === Kinetic: 大圆角（友好）=== */
[data-direction="kinetic"] {
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --radius-full: 9999px;
  --button-radius: 9999px;     /* 药丸形按钮 */
  --card-radius: 16px;
}

/* === Freestyle: 混合圆角（不规则）=== */
[data-direction="freestyle"] {
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 24px;            /* 有时很大 */
  --radius-full: 9999px;
  --button-radius: 0px;         /* 有时直角 */
  --card-radius: 12px;          /* 通常中等 */
}
```

### 视觉示例

```
Luxury / Cyberpunk:          Kinetic:                    Freestyle:
┌──────────────────┐         ┌──────────────────┐         ╭──────────────────╮
│                  │         ╭──────────────────╮         │                  │
│   直角矩形       │         │                  │         │   不规则圆角    │
│                  │         │   圆角矩形       │         │                  │
│                  │         ╰──────────────────╯         ╰──────────────────╯
└──────────────────┘         └──────────────────┘

Button:                    Button:                     Button:
┌──────────────────┐         ╭──────────────────╮         ┌──────────────────╮
│      BUTTON      │         │      BUTTON      │         │      BUTTON      │
└──────────────────┘         ╰──────────────────╯         └──────────────────╯
```

---

## 🎬 动画与过渡

### 过渡时长对比

| 方向 | 时长 | 感受 | 用途 |
| ------ | ------ | ------ | ------ |
| **Luxury** | 1200ms | 缓慢、优雅 | 奢华感、仪式感 |
| **Cyberpunk** | 150ms | 即时、响应 | 科技感、效率 |
| **Kinetic** | 400ms | 弹性、活泼 | 互动感、愉悦 |
| **Freestyle** | 600ms | 戏剧性、表现力 | 冲击力、记忆点 |

### 缓动函数（Easing）

每个方向都有**专属的缓动曲线**：

```css
/* === Luxury: 优雅的减速曲线 === */
--transition-ease: cubic-bezier(0.16, 1, 0.3, 1);
/*
  特点: 开始快，结束慢
  效果: 优雅的减速，像高级汽车门关闭
*/

/* === Cyberpunk: 线性/即时 === */
--transition-ease: cubic-bezier(0.76, 0, 0.24, 1);
/*
  特点: 几乎线性，轻微加速
  效果: 机械、精确、无延迟感
*/

/* === Kinetic: 弹性过冲 === */
--transition-ease: cubic-bezier(0.34, 1.56, 0.64, 1);
/*
  特点: 超过目标值后回弹
  效果: 活泼、有弹性、物理感
  ⚠️ 注意: 这个曲线会导致 overshoot，不适合所有场景
*/

/* === Freestyle: 戏剧性减速 === */
--transition-ease: cubic-bezier(0.22, 1, 0.36, 1);
/*
  特点: 开始非常慢，然后快速完成
  效果: 戏剧性的揭示，像舞台幕布拉开
*/
```

### Tailwind 中的自定义缓动

```js
// tailwind.config.ts
transitionTimingFunction: {
  'luxury': 'cubic-bezier(0.16, 1, 0.3, 1)',
  'cyber': 'cubic-bezier(0.76, 0, 0.24, 1)',
  'kinetic': 'cubic-bezier(0.34, 1.56, 0.64, 1)',
  'freestyle': 'cubic-bezier(0.22, 1, 0.36, 1)',
}
```

**使用方式**:

```html
<!-- Luxury 按钮 -->
<button class="transition-all duration-1000 ease-luxury hover:scale-105">
  Luxury Button
</button>

<!-- Kinetic 按钮 -->
<button class="transition-all duration-400 ease-kinetic hover:scale-110 active:scale-95">
  Kinetic Button
</button>
```

### 内置动画预设

项目中预定义了 **30+ 种 CSS 动画**（详见 [COMPONENT_GUIDE.md](./COMPONENT_GUIDE.md)），这里列出各方向最常用的：

| 动画 | Luxury | Cyberpunk | Kinetic | Freestyle |
| ------ | -------- | ----------- | --------- | ----------- |
| **fade-up** | ✅ 常用 | ❌ 太慢 | ✅ 常用 | ✅ 常用 |
| **float** | ✅ 微妙悬浮 | ❌ | ✅ 明显浮动 | ✅ 戏剧性浮动 |
| **pulse-glow** | ✅ 金色脉冲 | ✅ 青色闪烁 | ✅ 紫色呼吸 | ✅ 橙色脉动 |
| **shimmer** | ❌ | ✅ 扫描线 | ✅ 光泽扫过 | ✅ |
| **glitch** | ❌ | ✅ **标志性** | ❌ | ❌ |
| **bounce-subtle** | ❌ | ❌ | ✅ **常用** | ❌ |
| **text-reveal** | ✅ 缓慢揭示 | ✅ 快速打字 | ✅ 弹性出现 | ✅ 戏剧性 |

---

## 🧩 组件样式变体

### 按钮变体

```css
/* === Luxury Button === */
.luxury-button {
  padding: 2rem 4rem;              /* 超大内边距 */
  border-radius: 0px;               /* 直角 */
  font-weight: 200;                 /* 极细 */
  letter-spacing: 0.3em;            /* 超宽字距 */
  text-transform: uppercase;        /* 全大写 */
  background: var(--primary);       /* 黑色或白色 */
  color: var(--primary-foreground);
  transition: all 1200ms cubic-bezier(0.16, 1, 0.3, 1);
}

.luxury-button:hover {
  background: var(--accent);        /* 变为金色 */
  transform: translateY(-2px);      /* 微妙上浮 */
}

/* === Cyberpunk Button === */
.cyberpunk-button {
  padding: 0.75rem 2rem;            /* 紧凑 */
  border-radius: 0px;
  font-weight: 600;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  background: transparent;
  border: 1px solid var(--primary); /* 青色边框 */
  color: var(--primary);
  transition: all 150ms cubic-bezier(0.76, 0, 0.24, 1);
  position: relative;
  overflow: hidden;
}

.cyberpunk-button::before {
  content: '';
  position: absolute;
  inset: 0;
  background: var(--primary);
  transform: translateX(-100%);
  transition: transform 150ms;
}

.cyberpunk-button:hover::before {
  transform: translateX(0);         /* 扫光效果 */
}

.cyberpunk-button:hover {
  color: var(--primary-foreground); /* 文字变黑 */
}

/* === Kinetic Button === */
.kinetic-button {
  padding: 1rem 2rem;
  border-radius: 9999px;            /* 药丸形 */
  font-weight: 600;
  background: var(--primary);       /* 紫色 */
  color: white;
  transition: all 400ms cubic-bezier(0.34, 1.56, 0.64, 1);
  box-shadow: 0 4px 14px rgba(124, 58, 237, 0.3);
}

.kinetic-button:hover {
  transform: translateY(-2px) scale(1.05);  /* 弹性放大 */
  box-shadow: 0 8px 25px rgba(124, 58, 237, 0.4);
}

.kinetic-button:active {
  transform: scale(0.95);            /* 按下缩小 */
}

/* === Freestyle Button === */
.freestyle-button {
  padding: 1.25rem 2.5rem;
  border-radius: 0px;                /* 有时是混合圆角 */
  font-weight: 800;                  /* 特粗 */
  background: var(--primary);       /* 橙色 */
  color: white;
  transition: all 600ms cubic-bezier(0.22, 1, 0.36, 1);
  position: relative;
}

.freestyle-button::after {
  content: '';
  position: absolute;
  inset: -2px;
  background: conic-gradient(from 0deg, var(--primary), yellow, var(--primary));
  z-index: -1;
  filter: blur(8px);
  opacity: 0;
  transition: opacity 600ms;
}

.freestyle-button:hover::after {
  opacity: 1;                        /* 彩虹光晕 */
}
```

### 卡片变体

```css
/* Luxury Card: 极简无边框 */
.luxury-card {
  border-radius: 0px;
  border: 0.5px solid var(--border);  /* 极细边框 */
  padding: 3rem;
  background: var(--surface);
  transition: all 1200ms;
}

.luxury-card:hover {
  border-color: var(--accent);       /* 金色边框 */
  box-shadow: 0 0 0 1px var(--accent);
}

/* Cyberpunk Card: 科技感边框 */
.cyberpunk-card {
  border-radius: 0px;
  border: 1px solid var(--border);
  background: var(--surface);
  position: relative;
  clip-path: polygon(
    0 0,
    calc(100% - 12px) 0,
    100% 12px,
    100% 100%,
    12px 100%,
    0 calc(100% - 12px)
  );  /* 切角效果 */
}

/* Kinetic Card: 大圆角阴影 */
.kinetic-card {
  border-radius: 16px;
  background: var(--surface);
  box-shadow:
    0 4px 6px -1px rgba(0, 0, 0, 0.05),
    0 10px 30px -5px rgba(124, 58, 237, 0.1);
  transition: all 400ms cubic-bezier(0.34, 1.56, 0.64, 1);
}

.kinetic-card:hover {
  transform: translateY(-8px);
  box-shadow:
    0 20px 40px -10px rgba(124, 58, 237, 0.2);
}

/* Freestyle Card: 不规则造型 */
.freestyle-card {
  border-radius: 12px 24px 12px 24px;  /* 混合圆角 */
  background: var(--surface);
  border: 2px solid var(--primary);
  transform: rotate(-1deg);            /* 轻微倾斜 */
  transition: all 600ms;
}

.freestyle-card:hover {
  transform: rotate(1deg) translateY(-4px);
  border-color: var(--accent);
}
```

---

## 📋 CSS 变量完整参考表

### 基础变量（所有方向通用）

| 变量名 | 类型 | 默认值 | 说明 |
| -------- | ------ | -------- | ------ |
| `--background` | Color | - | 页面背景色 |
| `--foreground` | Color | - | 主要文本颜色 |
| `--primary` | Color | - | 主色调（品牌色） |
| `--primary-foreground` | Color | - | 主色调上的文本 |
| `--secondary` | Color | - | 次要颜色 |
| `--secondary-foreground` | Color | - | 次要颜色上的文本 |
| `--muted` | Color | - | 弱化背景 |
| `--muted-foreground` | Color | - | 弱化文本 |
| `--accent` | Color | - | 强调色 |
| `--accent-foreground` | Color | - | 强调色上的文本 |
| `--border` | Color | - | 边框颜色 |
| `--surface` | Color | - | 表面/卡片背景 |
| `--glow` | Color | - | 发光效果颜色 |
| `--selection` | Color | - | 文本选中颜色 |

### 布局变量

| 变量名 | 类型 | Luxury | Cyberpunk | Kinetic | Freestyle |
| -------- | ------ | -------- | ----------- | --------- | ----------- |
| `--radius-sm` | Length | 0px | 0px | 8px | 4px |
| `--radius-md` | Length | 0px | 0px | 12px | 8px |
| `--radius-lg` | Length | 0px | 0px | 16px | 24px |
| `--radius-full` | Length | 0px | 0px | 9999px | 9999px |
| `--space-section` | Length | 16rem | 6rem | 10rem | 8rem |
| `--space-block` | Length | 6rem | 2rem | 4rem | 3rem |
| `--container-max` | Length | 900px | 1400px | 1200px | 1300px |

### 排版变量

| 变量名 | 类型 | Luxury | Cyberpunk | Kinetic | Freestyle |
| -------- | ------ | -------- | ----------- | --------- | ----------- |
| `--heading-weight` | Number | 100 | 700 | 700 | 800 |
| `--heading-tracking` | Length | 0.15em | 0.08em | -0.02em | -0.03em |
| `--heading-transform` | Keyword | uppercase | uppercase | none | none |
| `--body-weight` | Number | 200 | 400 | 500 | 600 |
| `--body-tracking` | Length | 0.04em | 0.03em | 0 | -0.01em |

### 组件变量

| 变量名 | 类型 | Luxury | Cyberpunk | Kinetic | Freestyle |
| -------- | ------ | -------- | ----------- | --------- | ----------- |
| `--button-radius` | Length | 0px | 0px | 9999px | 0px |
| `--button-padding` | Length | 2rem 4rem | 0.75rem 2rem | 1rem 2rem | 1.25rem 2.5rem |
| `--button-weight` | Number | 200 | 600 | 600 | 800 |
| `--button-tracking` | Length | 0.3em | 0.15em | 0.05em | 0.02em |
| `--button-transform` | Keyword | uppercase | uppercase | none | none |
| `--card-radius` | Length | 0px | 0px | 16px | 12px |
| `--card-border-width` | Length | 0.5px | 1px | 1px | 2px |
| `--nav-height` | Length | 6rem | 3rem | 4rem | 4rem |

### 过渡变量

| 变量名 | 类型 | Luxury | Cyberpunk | Kinetic | Freestyle |
|--------|------|--------|-----------|---------|-----------|
| `--transition-duration` | Time | 1200ms | 150ms | 400ms | 600ms |
| `--transition-ease` | Easing | (0.16,1,0.3,1) | (0.76,0,0.24,1) | (0.34,1.56,0.64,1) | (0.22,1,0.36,1) |

---

## 🎯 使用指南

### 如何在组件中使用设计系统

#### 方法 1: 直接使用 CSS 变量

```tsx
function MyComponent() {
  return (
    <div style={{
      color: 'hsl(var(--foreground))',
      backgroundColor: 'hsl(var(--surface))',
      padding: 'var(--space-block, 3rem)',
      borderRadius: 'var(--radius-lg, 16px)',
    }}>
      Content
    </div>
  );
}
```

#### 方法 2: 使用 Tailwind 类 + 方向变体

```tsx
function MyComponent() {
  return (
    <div className="
      bg-surface
      text-foreground
      p-[var(--space-block)]
      rounded-[var(--radius-lg)]
      luxury:rounded-none
      cyberpunk:rounded-none
      kinetic:rounded-2xl
      freestyle:rounded-xl
    ">
      Content
    </div>
  );
}
```

#### 方法 3: 使用 cn() 工具函数合并类名

```tsx
import { cn } from '@/lib/utils';

function MyComponent({ direction }: { direction: Direction }) {
  return (
    <div className={cn(
      'base-classes',
      direction === 'luxury' && 'luxury-specific-classes',
      direction === 'cyberpunk' && 'cyberpunk-specific-classes',
    )}>
      Content
    </div>
  );
}
```

### 创建方向感知的组件

#### 示例: 方向自适应的卡片

```tsx
// components/DirectionAwareCard.tsx
'use client';

import { useTheme } from '@/lib/theme';
import { cn } from '@/lib/utils';

interface DirectionAwareCardProps {
  children: ReactNode;
  className?: string;
}

export function DirectionAwareCard({ children, className }: DirectionAwareCardProps) {
  const { direction } = useTheme();

  return (
    <div className={cn(
      // 基础样式
      'relative overflow-hidden bg-surface border border-border p-6 transition-all duration-500',

      // Luxury 特定样式
      direction === 'luxury' && [
        'rounded-none border-opacity-30 hover:border-accent',
        'shadow-none hover:shadow-lg'
      ],

      // Cyberpunk 特定样式
      direction === 'cyberpunk' && [
        'rounded-none border-primary/30',
        'bg-gradient-to-b from-surface to-background',
        'hover:shadow-[0_0_20px_rgba(0,255,209,0.15)]'
      ],

      // Kinetic 特定样式
      direction === 'kinetic' && [
        'rounded-2xl shadow-lg',
        'hover:-translate-y-2 hover:shadow-2xl hover:shadow-primary/10',
        'active:scale-[0.98]'
      ],

      // Freestyle 特定样式
      direction === 'freestyle' && [
        'rounded-xl border-2 border-primary',
        'hover:rotate-1 hover:border-accent',
        'transition-transform duration-600'
      ],

      className
    )}>
      {children}
    </div>
  );
}
```

#### 使用示例

```tsx
import { DirectionAwareCard } from './DirectionAwareCard';
import { ThemeProvider, useTheme } from '@/lib/theme';

function App() {
  return (
    <ThemeProvider defaultDirection="kinetic">
      <Page />
    </ThemeProvider>
  );
}

function Page() {
  const { direction, setDirection, cycleDirection } = useTheme();

  return (
    <main className="min-h-screen bg-background p-8">
      {/* 方向切换器 */}
      <div className="mb-8 flex gap-2">
        {['luxury', 'cyberpunk', 'kinetic', 'freestyle'].map((d) => (
          <button
            key={d}
            onClick={() => setDirection(d as Direction)}
            className={cn(
              'px-4 py-2 rounded-lg border transition-colors',
              direction === d
                ? 'bg-primary text-primary-foreground'
                : 'bg-surface hover:bg-muted'
            )}
          >
            {d}
          </button>
        ))}
        <button
          onClick={cycleDirection}
          className="px-4 py-2 bg-accent text-accent-foreground rounded-lg"
        >
          Cycle →
        </button>
      </div>

      {/* 方向自适应卡片 */}
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 max-w-6xl mx-auto">
        <DirectionAwareCard>
          <h3 className="text-xl font-heading font-bold mb-2">Card 1</h3>
          <p className="text-muted-foreground">
            This card adapts its style based on the current design direction.
          </p>
        </DirectionAwareCard>

        <DirectionAwareCard>
          <h3 className="text-xl font-heading font-bold mb-2">Card 2</h3>
          <p className="text-muted-foreground">
            Try switching directions using the buttons above!
          </p>
        </DirectionAwareCard>

        <DirectionAwareCard>
          <h3 className="text-xl font-heading font-bold mb-2">Card 3</h3>
          <p className="text-muted-foreground">
            Each direction has unique border radius, shadows, and hover effects.
          </p>
        </DirectionAwareCard>
      </div>
    </main>
  );
}
```

### 最佳实践

#### ✅ DO - 推荐做法

1. **始终使用 CSS 变量引用颜色**

   ```css
   /* ✅ 好 */
   color: hsl(var(--foreground));

   /* ❌ 差 - 硬编码颜色 */
   color: #1E1F2C;
   ```

2. **利用方向变体做差异化**

   ```html
   <!-- ✅ 好 -->
   <div className="luxury:font-thin cyberpunk:font-bold kinetic:font-semibold">

   <!-- ❌ 差 - 用 JS 条件渲染 */
   ```

3. **尊重各方向的设计约束**
   - Luxury: 保持极简，不要添加过多装饰
   - Cyberpunk: 使用等宽字体和高对比度
   - Kinetic: 利用弹性动画和圆角
   - Freestyle: 可以打破规则，但要有意图

4. **测试所有四个方向**
   - 确保你的组件在所有方向下都可读可用
   - 特别注意对比度和可访问性

#### ❌ DON'T - 避免的做法

1. **不要硬编码方向特定的值**

   ```tsx
   // ❌ 差
   if (direction === 'luxury') {
     return <div style={{ color: '#000' }}>;
   }

   // ✅ 好
   return <div className="text-foreground">;
   ```

2. **不要忽略暗色模式**
   - 始终测试 `.dark` 类下的效果
   - 确保颜色在两种模式下都有足够对比度

3. **不要过度使用方向变体**
   - 只在真正需要差异化的地方使用
   - 保持代码可维护性

4. **不要破坏视觉一致性**
   - 同一页面/组件内保持风格统一
   - 不要在同一个卡片中混用多个方向的特性

---

## 🔧 扩展设计系统

### 添加新的 CSS 变量

**Step 1: 在 globals.css 中定义**

```css
/* 为所有方向添加新变量 */
[data-direction="luxury"] {
  --my-custom-variable: value-for-luxury;
}

[data-direction="cyberpunk"] {
  --my-custom-variable: value-for-cyberpunk;
}

/* ... 其他方向 */
```

**Step 2: 在 Tailwind 中使用（可选）**

```js
// tailwind.config.ts
theme: {
  extend: {
    colors: {
      'my-custom': 'hsl(var(--my-custom-variable))',
    }
  }
}
```

**Step 3: 在组件中使用**

```tsx
<div className="bg-my-custom">
  Uses the new custom variable
</div>
```

### 创建全新的设计方向

参考 [GETTING_STARTED.md](./GETTING_STARTED.md) 中的 "如何添加新的设计方向？" 章节。

---

## 📊 对比总结表

| 特征 | Luxury | Cyberpunk | Kinetic | Freestyle |
| ------ | -------- | ----------- | --------- | ----------- |
| **关键词** | 极简、优雅 | 科技、未来 | 动感、友好 | 大胆、创新 |
| **主色调** | 黑/白/金 | 深空/青/品红 | 白/紫/青绿 | 暖灰/橙 |
| **字体** | Space Grotesk | JetBrains Mono | Outfit | Syne |
| **圆角** | 0px (直角) | 0px (直角) | 大圆角 | 混合 |
| **留白** | 极多 | 紧凑 | 充足 | 灵活 |
| **字重** | 极细 (100-200) | 粗 (600-700) | 中等 (500-700) | 特粗 (700-800) |
| **过渡时长** | 1200ms | 150ms | 400ms | 600ms |
| **缓动曲线** | 优雅减速 | 线性/即时 | 弹性过冲 | 戏剧性 |
| **最佳场景** | 高端品牌 | 游戏/科技 | SaaS/创业 | 创意/艺术 |
| **性能影响** | 低 | 低 | 中 | 中 |
| **可访问性** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **学习曲线** | 简单 | 中等 | 简单 | 中等 |

---

## 🎓 设计理论背景

### 为什么是这四个方向？

这四个方向代表了**当代 Web 设计的四大主流趋势**：

1. **Minimalism (Luxury)** - 从 Dieter Rams 的"好设计是尽可能少设计"到 Apple 的极简主义
2. **Tech-Futurism (Cyberpunk)** - 从《银翼杀手》到赛博文化的视觉语言
3. **Playful Professionalism (Kinetic)** - Stripe、Linear 等现代 SaaS 的"专业但有趣"
4. **Brutalism/Expressionism (Freestyle)** - 对标准化设计的反叛，追求个性表达

### 设计系统的价值

1. **一致性** - 所有组件在同一设计语言下和谐共存
2. **效率** - 无需每次从零开始设计，直接使用经过验证的模式
3. **灵活性** - 四种方向覆盖大部分使用场景
4. **可维护性** - 修改变量即可全局更新
5. **可访问性** - 内置对比度和减少动画支持

---

## 📚 延伸阅读

### 设计资源

- **[Refactoring UI]** (<https://refactoringui.com/>) - 设计系统方法论
- **[Design Systems Handbook]** (<https://www.designsystems.com/>) - 行业案例
- **[Tailwind CSS Design]** (<https://tailwindcss.com/designing-with-utilities>) - 原子化 CSS 设计

### 动画原理

- **[Framer Motion Docs]** (<https://www.framer.com/motion/>) - 声明式动画
- **[GSAP Easing Visualizer]** (<https://greensock.com/ease-visualizer/>) - 可视化缓动曲线
- **[Cubic Bezier]** (<https://cubic-bezier.com/>) - 自定义缓动编辑器

### 色彩理论

- **[Color Contrast Checker]** (<https://webaim.org/resources/contrastchecker/>) - WCAG 对比度检测
- **[Coolors]** (<https://coolors.co/>) - 配色方案生成器
- **[Adobe Color]** (<https://color.adobe.com/>) - 色轮工具

---

*最后更新: 2026-10-22*
*预计阅读时间: 25分钟*
*难度等级: 🟡 中级（需要一定的 CSS/Tailwind 基础）*
*相关文档: [PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md) | [COMPONENT_GUIDE.md](./COMPONENT_GUIDE.md)*
