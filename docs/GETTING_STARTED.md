# Motion Primitives - 快速开始指南

> 5分钟从零到运行，快速上手 Motion Primitives 组件库

---

## 📋 目录

1. [环境要求](#-环境要求)
2. [安装步骤](#-安装步骤)
3. [启动项目](#-启动项目)
4. [你的第一个动画组件](#-你的第一个动画组件)
5. [项目结构速览](#-项目结构速览)
6. [开发工作流](#-开发工作流)
7. [常见问题](#-常见问题)

---

## 🖥️ 环境要求

### 必需软件

| 软件 | 版本要求 | 用途 |
| ------ | --------- | ------ |
| **Node.js** | >= 18.0.0 | JavaScript 运行时 |
| **pnpm** | >= 8.0.0 | 包管理器（推荐） |
| **Git** | 最新版 | 版本控制 |

### 可选工具

| 工具 | 用途 | 安装命令 |
| ------ | ------ | --------- |
| VS Code | 代码编辑器 | [下载](https://code.visualstudio.com/) |
| Chrome DevTools | 浏览器调试 | 随 Chrome 安装 |
| React DevTools | React 调试 | [VS Code 扩展](https://marketplace.visualstudio.com/items?itemName=Reactjs-tools.react-code) |

### 检查环境

打开终端，运行以下命令验证环境：

```bash
# 检查 Node.js 版本（需要 >= 18）
node --version
# 输出示例: v20.11.0

# 检查 pnpm 版本（需要 >= 8）
pnpm --version
# 输出示例: 8.15.0

# 检查 Git 版本
git --version
# 输出示例: git version 2.43.0
```

如果版本不满足要求，请先升级：

```bash
# 升级 Node.js（推荐使用 nvm）
nvm install --lts
nvm use --lts

# 升级 pnpm
npm install -g pnpm@latest
```

---

## 📦 安装步骤

### 方式一：克隆仓库（推荐）

```bash
# 1. 克隆项目到本地
git clone https://github.com/itsjwill/motion-primitives-website.git

# 2. 进入项目目录
cd motion-primitives-website

# 3. 安装依赖
pnpm install
```

### 方式二：直接下载

1. 访问 GitHub 仓库：<https://github.com/itsjwill/motion-primitives-website>
2. 点击绿色的 **Code** 按钮
3. 选择 **Download ZIP**
4. 解压到目标文件夹
5. 打开终端，进入该目录：

   ```bash
   cd motion-primitives-website
   pnpm install
   ```

### 安装过程说明

`pnpm install` 会：

- ✅ 读取 `package.json` 中的依赖声明
- ✅ 下载约 **28个包**（20个生产依赖 + 8个开发依赖）
- ✅ 生成 `node_modules/` 目录和 `pnpm-lock.yaml` 锁定文件
- ⏱️ **预计耗时**: 1-3分钟（取决于网络速度）

**首次安装可能遇到的问题**：

#### 问题 1: pnpm 未找到

```bash
# 解决方案：安装 pnpm
npm install -g pnpm
```

#### 问题 2: 权限错误（Linux/Mac）

```bash
# 解决方案：使用 sudo 或修复 npm 权限
sudo pnpm install
# 或者配置 npm 使用用户目录
npm config set prefix '~/.npm-global'
```

#### 问题 3: 网络慢/超时

```bash
# 解决方案：使用国内镜像源
pnpm config set registry https://registry.npmmirror.com
pnpm install
```

---

## 🚀 启动项目

### 开发模式

开发模式支持**热更新**（Hot Module Replacement），修改代码后浏览器自动刷新。

```bash
# 启动开发服务器
pnpm dev
```

**预期输出**:

```
> motion-primitives-website@2.0.0 dev
> next dev

 ▲ Next.js 16.1.6
 - Local:        http://localhost:3000
 - Environments: .env.local
 - Network:      http://192.168.x.x:3000

 ✓ Starting...
 ✓ Ready in 2.3s
```

**访问地址**:

- 本地访问：<http://localhost:3000>
- 局域网访问：<http://192.168.x.x:3000（可在手机上测试）>

### 停止服务器

在终端按 `Ctrl + C` 停止开发服务器。

### 其他常用命令

```bash
# 生产构建（优化、压缩、代码分割）
pnpm build

# 启动生产服务器（需要先执行 pnpm build）
pnpm start

# 代码检查（ESLint）
pnpm lint

# 分析打包体积
pnpm analyze
# 会在浏览器中打开 bundle analyzer
```

---

## ✨ 你的第一个动画组件

让我们创建一个简单的页面，使用 Motion Primitives 的核心组件。

### 步骤 1: 创建新文件

在 `src/app/` 下创建 `demo-page/page.tsx`：

```tsx
// src/app/demo-page/page.tsx
"use client";

import { Dock, DockItem } from "@/components/interactive";
import { AuroraBackground } from "@/components/backgrounds";
import { AnimatedCounter } from "@/components/interactive";
import { TextReveal } from "@/components/text";

export default function DemoPage() {
  return (
    <AuroraBackground>
      <div className="relative z-10 min-h-screen flex flex-col items-center justify-center gap-16 px-6">
        {/* 标题动画 */}
        <TextReveal
          text="Hello, Motion Primitives!"
          by="word"
          delay={0.5}
          as="h1"
          className="text-5xl md:text-7xl font-bold text-white text-center"
        />

        {/* 动画计数器 */}
        <div className="grid grid-cols-3 gap-8 text-white">
          <div className="text-center">
            <AnimatedCounter value={110} suffix="+" />
            <p className="mt-2 text-white/70">组件</p>
          </div>
          <div className="text-center">
            <AnimatedCounter value={4} />
            <p className="mt-2 text-white/70">设计方向</p>
          </div>
          <div className="text-center">
            <AnimatedCounter value={100} suffix="%" />
            <p className="mt-2 text-white/70">免费</p>
          </div>
        </div>

        {/* Dock 栏 */}
        <Dock magnification={60} distance={140}>
          <DockItem label="GitHub" onClick={() => window.open('https://github.com')}>
            <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
              <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
            </svg>
          </DockItem>
          <DockItem label="Twitter" onClick={() => window.open('https://twitter.com')}>
            <svg width="24" height="24" viewBox="0 0 24 24" fill="currentColor">
              <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
            </svg>
          </DockItem>
          <DockItem label="Documentation" onClick={() => alert('打开文档')}>
            <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2">
              <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"></path>
              <polyline points="14 2 14 8 20 8"></polyline>
              <line x1="16" y1="13" x2="8" y2="13"></line>
              <line x1="16" y1="17" x2="8" y2="17"></line>
              <polyline points="10 9 9 9 8 9"></polyline>
            </svg>
          </DockItem>
        </Dock>

        <p className="text-white/50 text-sm mt-8">
          编辑 src/app/demo-page/page.tsx 查看热更新效果 ✨
        </p>
      </div>
    </AuroraBackground>
  );
}
```

### 步骤 2: 查看效果

1. 访问 <http://localhost:3000/demo-page>
2. 你会看到：
   - ✅ 流动的极光背景
   - ✅ 逐词显示的标题动画
   - ✅ 三个数字计数器动画
   - ✅ 底部的 macOS 风格 Dock 栏（鼠标悬停试试！）

### 步骤 3: 尝试修改

保持开发服务器运行，编辑 `demo-page/page.tsx`：

```tsx
// 修改标题文字
text="你好，Motion Primitives！"  // 改成中文试试

// 修改计数器的值
value={999}  // 改成更大的数字

// 保存后，浏览器会自动刷新！
```

---

## 📂 项目结构速览

了解关键目录的作用：

```
motion-primitives-website/
├── src/
│   ├── app/                    # Next.js 页面和路由
│   │   ├── page.tsx           # 首页
│   │   ├── layout.tsx         # 全局布局（主题、字体、平滑滚动）
│   │   ├── globals.css        # 全局样式 + 四种设计方向的 CSS 变量
│   │   └── _sections/         # 首页的各个区块（Hero, CTA 等）
│   │
│   ├── components/            # 🔥 所有可复用组件（115个）
│   │   ├── interactive/       # 交互组件（Dock, SpotlightCard 等）
│   │   ├── backgrounds/       # 背景效果（Aurora, Meteors 等）
│   │   ├── cards/             # 卡片组件（GlowCard, TiltCard 等）
│   │   ├── three/             # 3D 效果（Globe, NeuralNetwork 等）
│   │   ├── scroll/            # 滚动动画（ParallaxScroll 等）
│   │   ├── text/              # 文字动画（TextReveal 等）
│   │   ├── effects/           # 视觉特效（MagneticButton 等）
│   │   ├── transitions/       # 页面过渡（Preloader 等）
│   │   ├── layout/            # 布局组件
│   │   ├── navigation/        # 导航组件
│   │   ├── seo/               # SEO 组件
│   │   └── ...                # 更多分类
│   │
│   ├── hooks/                 # 自定义 Hooks（6个）
│   │   ├── use-mouse-position.ts    # 鼠标位置追踪
│   │   ├── use-gsap.ts              # GSAP 动画封装
│   │   ├── use-lenis.ts             # 平滑滚动
│   │   └── ...
│   │
│   ├── lib/                   # 工具函数和配置
│   │   ├── theme.tsx          # 🎨 主题系统（4种设计方向）
│   │   ├── fonts.ts           # 字体配置（Google Fonts）
│   │   ├── utils.ts           # cn() 类名合并工具
│   │   └── motion.ts          # Framer Motion 预设
│   │
│   └── types/                 # TypeScript 类型定义
│
├── public/                     # 静态资源（图片、图标等）
├── docs/                       # 项目文档（你在这里！）
├── tailwind.config.ts         # Tailwind CSS 配置
├── next.config.js             # Next.js 配置
└── package.json               # 项目元数据和依赖
```

### 关键文件说明

| 文件 | 作用 | 修改频率 |
| ------ | ------ | --------- |
| `src/app/layout.tsx` | 根布局，初始化主题、字体、平滑滚动 | 低（通常不需要改） |
| `src/app/globals.css` | 全局样式，定义4种设计的 CSS 变量 | 中（添加新方向时） |
| `src/lib/theme.tsx` | ThemeProvider，管理设计方向和明暗模式 | 中（扩展主题功能） |
| `tailwind.config.ts` | Tailwind 配置，自定义动画、颜色等 | 中（添加新工具类） |
| `package.json` | 依赖管理，脚本命令 | 低（添加新依赖时） |

---

## 💻 开发工作流

### 日常开发流程

```
1. 启动开发服务器
   └─ pnpm dev

2. 创建/编辑组件或页面
   └─ 编辑 src/components/... 或 src/app/...

3. 浏览器自动刷新（HMR）
   └─ 查看 http://localhost:3000

4. 保存并提交
   └─ git add . && git commit -m "描述"
```

### 推荐的 VS Code 扩展

提升开发体验的必备扩展：

| 扩展名 | 用途 | 安装 |
| -------- | ------ | ------ |
| **ESLint** | 代码质量检查 | 内置 |
| **Prettier** | 代码格式化 | 搜索 "Prettier" |
| **Tailwind CSS IntelliSense** | Tailwind 类名补全 | 搜索 "Tailwind" |
| **Auto Rename Tag** | 自动重命名标签 | 搜索 "Auto Rename Tag" |
| **Path Intellisense** | 路径自动补全 | 搜索 "Path Intellisense" |
| **Reactjs code snippets** | React 代码片段 | 搜索 "React" |

### 调试技巧

#### 1. React DevTools

安装后按 `F12` → 切换到 **Components** 标签：

- 查看组件树结构
- 检查 Props 和 State
- 测量渲染性能

#### 2. 浏览器开发者工具

```bash
# 查看元素样式
F12 → Elements 面板

# 查看控制台日志
F12 → Console 面板

# 性能分析
F12 → Performance 面板 → 点击 Record → 操作页面 → Stop

# 检查网络请求
F12 → Network 面板
```

#### 3. Three.js 调试

对于 3D 组件，可以使用 `@react-three/devtools`：

```tsx
import { Canvas } from '@react-three/fiber';
import { Stats, OrbitControls } from '@react-three/drei';

<Canvas>
  <Stats />  {/* 左上角显示 FPS */}
  <OrbitControls />  {/* 允许旋转/缩放场景 */}
  <Your3DComponent />
</Canvas>
```

---

## ❓ 常见问题

### Q1: 端口 3000 已被占用？

**错误信息**:

```
Port 3000 is in use, trying another port.
```

**解决方案**:

```bash
# 方法 1: 使用其他端口
pnpm dev -p 3001
# 访问 http://localhost:3001

# 方法 2: 关闭占用端口的进程
# Windows:
netstat -ano | findstr :3000
taskkill /PID <PID号> /F

# Mac/Linux:
lsof -i :3000
kill -9 <PID号>
```

---

### Q2: 样式没有生效？

**可能原因**:

1. **忘记导入 globals.css**

   ```tsx
   // 在 layout.tsx 中确保有这行
   import "./globals.css";
   ```

2. **Tailwind 类名拼写错误**

   ```css
   /* 错误 */
   class="bg-blue500"

   /* 正确 */
   className="bg-blue-500"
   ```

3. **CSS 变量未定义**

   ```css
   /* 确保在 globals.css 的 [data-direction] 中定义了变量 */
   [data-direction="luxury"] {
     --primary: 30 5% 8%;  /* HSL 格式 */
   }
   ```

---

### Q3: 组件找不到？

**错误信息**:

```
Module not found: Can't resolve '@/components/...'
```

**解决方案**:

1. **检查路径别名**
   确保 `tsconfig.json` 中配置了路径别名：

   ```json
   {
     "compilerOptions": {
       "paths": {
         "@/*": ["./src/*"]
       }
     }
   }
   ```

2. **检查文件是否存在**

   ```bash
   ls src/components/interactive/dock.tsx
   ```

3. **重启开发服务器**

   ```bash
   # 按 Ctrl+C 停止
   # 重新运行
   pnpm dev
   ```

---

### Q4: TypeScript 类型错误？

**常见错误**:

```
Property 'xxx' does not exist on type 'IntrinsicAttributes'
```

**解决方案**:

1. **检查 Props 接口**

   ```tsx
   interface MyProps {
     title: string;      // 必填
     count?: number;     // 可选
   }

   // 必须提供 title
   <MyComponent title="Hello" />
   ```

2. **使用类型断言（临时）**

   ```tsx
   // 如果确定类型正确但 TS 报错
   <Component {...props as any} />
   ```

3. **查看组件源码的类型定义**

   ```tsx
   // 在 IDE 中 Ctrl+Click 跳转到组件定义
   import { Dock } from '@/components/interactive';
   //        ^^^ Ctrl+Click 这里
   ```

---

### Q5: 动画卡顿/性能差？

**诊断步骤**:

1. **打开性能监控**

   ```
   F12 → Performance → Record → 操作页面 → Stop
   ```

   查找长任务（Long Tasks）

2. **检查 GPU 加速**

   ```css
   /* 确保动画属性是 GPU 友好的 */
   transform: translateX(100px);  /* ✅ 好 */
   left: 100px;                    /* ❌ 差，会触发重排 */
   ```

3. **减少粒子数量**（针对 Three.js 组件）

   ```tsx
   <Globe variant="dotted" radius={1.5} />  {/* 减小半径 */}
   <Particles quantity={30} />                 {/* 减少粒子数 */}
   ```

4. **启用减少动画模式**

   ```tsx
   const reduceMotion = useReducedMotion();
   // 当 reduceMotion=true 时跳过复杂动画
   ```

---

### Q6: 如何添加新的设计方向？

假设要添加 "Minimal" 方向：

**Step 1: 定义方向类型**

```tsx
// src/lib/theme.tsx
export type Direction = "luxury" | "cyberpunk" | "kinetic" | "freestyle" | "minimal";

export const DIRECTIONS: Direction[] = [
  "luxury", "cyberpunk", "kinetic", "freestyle", "minimal"
];

export const DIRECTION_META = {
  // ...existing directions
  minimal: {
    label: "Minimal",
    description: "Pure simplicity. Less is more.",
    accent: "#6B7280",
  },
};
```

**Step 2: 添加 CSS 变量**

```css
/* src/app/globals.css */

[data-direction="minimal"] {
  --background: 0 0% 100%;
  --foreground: 0 0% 9%;
  --primary: 0 0% 0%;
  /* ...其他变量 */
}

[data-direction="minimal"].dark {
  --background: 0 0% 7%;
  --foreground: 0 0% 95%;
  /* ...暗色模式变量 */
}
```

**Step 3: 配置字体**

```tsx
// src/lib/fonts.ts
import { Inter, Roboto } from "next/font/google";

export const minimalHeading = Inter({
  variable: "--font-minimal-heading",
  // ...
});

export const minimalBody = Roboto({
  variable: "--font-minimal-body",
  // ...
});
```

**Step 4: 添加 Tailwind 变体**

```js
// tailwind.config.ts
addVariant("minimal", '[data-direction="minimal"] &');
```

**Step 5: 使用新方向**

```tsx
<div className="minimal:bg-gray-100 minimal:text-gray-900">
  Minimal direction content
</div>
```

---

## 🎯 下一步学习

完成快速入门后，建议按以下顺序深入学习：

### 第 1 阶段：基础（1-2天）

- [ ] 阅读 [PROJECT_STRUCTURE.md](./PROJECT_STRUCTURE.md) - 了解完整项目结构
- [ ] 浏览所有组件分类，了解有哪些可用组件
- [ ] 尝试在首页 `_sections/` 中替换/添加区块

### 第 2 阶段：实践（3-5天）

- [ ] 阅读 [COMPONENT_GUIDE.md](./COMPONENT_GUIDE.md) - 学习组件 API
- [ ] 复制 5-10 个不同类型的组件到你的项目中
- [ ] 自定义组件的 props（颜色、大小、速度等）

### 第 3 阶段：进阶（1周+）

- [ ] 学习 [DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md) - 掌握四方向设计系统
- [ ] 阅读 [PERFORMANCE.md](./PERFORMANCE.md) - 性能优化技巧
- [ ] 尝试创建自己的动画组件并贡献给项目

### 第 4 阶段：贡献（持续）

- [ ] Fork 项目并创建特性分支
- [ ] 修复 Bug 或添加新组件
- [ ] 提交 Pull Request

---

## 📚 相关资源

### 官方文档

- **Next.js 文档**: <https://nextjs.org/docs>
- **React 文档**: <https://react.dev>
- **Framer Motion**: <https://www.framer.com/motion/>
- **GSAP**: <https://greensock.com/docs/>
- **Three.js**: <https://threejs.org/docs/>
- **Tailwind CSS**: <https://tailwindcss.com/docs>

### 社区资源

- **GitHub Issues**: <https://github.com/itsjwill/motion-primitives-website/issues>
- **Discussions**: <https://github.com/itsjwill/motion-primitives-website/discussions>
- **作者 Twitter**: [@itsjwill](https://x.com/itsjwill)

---

## ✅ 检查清单

在继续之前，确认你已完成以下步骤：

- [x] Node.js >= 18 已安装
- [x] pnpm >= 8 已安装
- [x] 项目已克隆/下载
- [x] `pnpm install` 成功执行
- [x] `pnpm dev` 能正常启动
- [x] 浏览器能访问 <http://localhost:3000>
- [x] 首页能正常显示动画效果
- [x] 创建了 demo-page 并能看到效果
- [x] 了解基本的项目结构

全部完成？🎉 恭喜你已经准备好开始使用 Motion Primitives 了！

---

## 🆘 获取帮助

如果遇到问题：

1. **查看文档** - 先搜索本文档和相关文档
2. **搜索 Issues** - 在 GitHub Issues 中查找类似问题
3. **提问** - 创建新 Issue，提供：
   - 操作系统和 Node.js 版本
   - 完整的错误信息和堆栈跟踪
   - 重现问题的最小代码示例
   - 你已经尝试过的解决方法

---

*最后更新: 2026-10-22*
*预计阅读时间: 15分钟*
*难度等级: 🟢 初学者友好*
