# Origin - UI 组件设计 (shadcn/ui)

## 概述

Origin 使用 **shadcn/ui** 作为主要 UI 组件库。与传统的 npm 包不同，shadcn/ui 采用"复制到项目"的方式，让开发者拥有完全的控制权。

## 为什么选择 shadcn/ui

### 对比其他组件库

| 特性 | shadcn/ui | MUI | Ant Design | Chakra UI |
|------|-----------|-----|------------|-----------|
| **运行时依赖** | 无 (复制代码) | ✅ npm 包 | ✅ npm 包 | ✅ npm 包 |
| **包体积** | 最小 | ~300KB | ~500KB | ~200KB |
| **可定制性** | 完全控制 | 有限 | 有限 | 中等 |
| **TypeScript** | ✅ 原生 | ✅ 支持 | ✅ 支持 | ✅ 支持 |
| **无障碍** | ✅ Radix 原生 | ✅ 好 | ✅ 好 | ✅ 好 |
| **样式系统** | Tailwind | 自有 (JSS) | 自有 (CSS-in-JS) | Emotion |
| **主题切换** | ✅ 简单 | ⚠️ 复杂 | ⚠️ 复杂 | ✅ 支持 |
| **Tree Shaking** | ✅ 完美 | ⚠️ 部分 | ⚠️ 部分 | ✅ 支持 |

### shadcn/ui 的核心优势

1. **无运行时依赖**
   - 组件代码直接复制到 `src/components/ui/`
   - 没有额外的 npm 包依赖
   - Tree shaking 100% 有效

2. **完全控制**
   - 可以直接修改组件代码
   - 不受上游更新影响
   - 完全自定义行为

3. **Tailwind 原生**
   - 与项目 Tailwind 配置无缝集成
   - 使用 `cn()` 工具函数合并类名
   - 支持所有 Tailwind 特性

4. **Radix UI 底层**
   - 完善的无障碍支持 (ARIA)
   - 键盘导航开箱即用
   - 屏幕阅读器友好

5. **渐进式采用**
   - 只复制需要的组件
   - 按需添加，不臃肿

## 项目设置

### 安装依赖

```bash
# 安装核心依赖
pnpm add tailwindcss class-variance-authority tailwind-merge clsx tailwindcss-animate

# 安装 Radix UI 依赖 (按需)
pnpm add @radix-ui/react-dialog
pnpm add @radix-ui/react-dropdown-menu
pnpm add @radix-ui/react-select
pnpm add @radix-ui/react-tabs
pnpm add @radix-ui/react-tooltip
pnpm add @radix-ui/react-popover
pnpm add @radix-ui/react-slider
pnpm add @radix-ui/react-switch
pnpm add @radix-ui/react-toggle
pnpm add @radix-ui/react-scroll-area
pnpm add @radix-ui/react-separator
pnpm add @radix-ui/react-slot
pnpm add @radix-ui/react-label

# 安装图标库
pnpm add lucide-react
```

### Tailwind 配置

```javascript
// tailwind.config.js
const { fontFamily } = require("tailwindcss/defaultTheme")

/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: ["class"],
  content: [
    './pages/**/*.{ts,tsx}',
    './components/**/*.{ts,tsx}',
    './app/**/*.{ts,tsx}',
    './src/**/*.{ts,tsx}',
  ],
  theme: {
    container: {
      center: true,
      padding: "2rem",
      screens: {
        "2xl": "1400px",
      },
    },
    extend: {
      colors: {
        border: "hsl(var(--border))",
        input: "hsl(var(--input))",
        ring: "hsl(var(--ring))",
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))",
        },
        secondary: {
          DEFAULT: "hsl(var(--secondary))",
          foreground: "hsl(var(--secondary-foreground))",
        },
        destructive: {
          DEFAULT: "hsl(var(--destructive))",
          foreground: "hsl(var(--destructive-foreground))",
        },
        muted: {
          DEFAULT: "hsl(var(--muted))",
          foreground: "hsl(var(--muted-foreground))",
        },
        accent: {
          DEFAULT: "hsl(var(--accent))",
          foreground: "hsl(var(--accent-foreground))",
        },
        popover: {
          DEFAULT: "hsl(var(--popover))",
          foreground: "hsl(var(--popover-foreground))",
        },
        card: {
          DEFAULT: "hsl(var(--card))",
          foreground: "hsl(var(--card-foreground))",
        },
      },
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },
      fontFamily: {
        sans: ["var(--font-sans)", ...fontFamily.sans],
        mono: ["var(--font-mono)", ...fontFamily.mono],
      },
      keyframes: {
        "accordion-down": {
          from: { height: 0 },
          to: { height: "var(--radix-accordion-content-height)" },
        },
        "accordion-up": {
          from: { height: "var(--radix-accordion-content-height)" },
          to: { height: 0 },
        },
      },
      animation: {
        "accordion-down": "accordion-down 0.2s ease-out",
        "accordion-up": "accordion-up 0.2s ease-out",
      },
    },
  },
  plugins: [require("tailwindcss-animate")],
}
```

### CSS 变量

```css
/* app/globals.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 222.2 84% 4.9%;
    --card: 0 0% 100%;
    --card-foreground: 222.2 84% 4.9%;
    --popover: 0 0% 100%;
    --popover-foreground: 222.2 84% 4.9%;
    --primary: 221.2 83.2% 53.3%;
    --primary-foreground: 210 40% 98%;
    --secondary: 210 40% 96.1%;
    --secondary-foreground: 222.2 47.4% 11.2%;
    --muted: 210 40% 96.1%;
    --muted-foreground: 215.4 16.3% 46.9%;
    --accent: 210 40% 96.1%;
    --accent-foreground: 222.2 47.4% 11.2%;
    --destructive: 0 84.2% 60.2%;
    --destructive-foreground: 210 40% 98%;
    --border: 214.3 31.8% 91.4%;
    --input: 214.3 31.8% 91.4%;
    --ring: 221.2 83.2% 53.3%;
    --radius: 0.5rem;
  }

  .dark {
    --background: 222.2 84% 4.9%;
    --foreground: 210 40% 98%;
    --card: 222.2 84% 4.9%;
    --card-foreground: 210 40% 98%;
    --popover: 222.2 84% 4.9%;
    --popover-foreground: 210 40% 98%;
    --primary: 217.2 91.2% 59.8%;
    --primary-foreground: 222.2 47.4% 11.2%;
    --secondary: 217.2 32.6% 17.5%;
    --secondary-foreground: 210 40% 98%;
    --muted: 217.2 32.6% 17.5%;
    --muted-foreground: 215 20.2% 65.1%;
    --accent: 217.2 32.6% 17.5%;
    --accent-foreground: 210 40% 98%;
    --destructive: 0 62.8% 30.6%;
    --destructive-foreground: 210 40% 98%;
    --border: 217.2 32.6% 17.5%;
    --input: 217.2 32.6% 17.5%;
    --ring: 224.3 76.3% 48%;
  }
}

@layer base {
  * {
    @apply border-border;
  }
  body {
    @apply bg-background text-foreground;
  }
}
```

### 工具函数

```typescript
// src/lib/utils.ts
import { type ClassValue, clsx } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

## Origin 使用的 shadcn/ui 组件

### 布局组件

```
src/components/ui/
├── accordion/          # 折叠面板
├── aspect-ratio/       # 宽高比容器
├── divider/            # 分割线
├── scroll-area/        # 滚动区域
└── separator/          # 分隔符
```

### 表单组件

```
src/components/ui/
├── button/             # 按钮
├── checkbox/           # 复选框
├── form/               # 表单容器
├── input/              # 输入框
├── label/              # 标签
├── radio-group/        # 单选组
├── select/             # 下拉选择
├── slider/             # 滑块
├── switch/             # 开关
├── textarea/           # 文本域
└── toggle/             # 切换按钮
```

### 反馈组件

```
src/components/ui/
├── alert/              # 警告提示
├── badge/              # 徽章
├── progress/           # 进度条
├── sonner/             # 通知提示
└── toast/              # 消息提示
```

### 展示组件

```
src/components/ui/
├── avatar/             # 头像
├── card/               # 卡片
├── collapsible/        # 可折叠
├── dialog/             # 对话框
├── drawer/             # 抽屉
├── popover/            # 弹出框
├── sheet/              # 侧边栏
├── table/              # 表格
├── tabs/               # 标签页
└── tooltip/            # 提示框
```

### 导航组件

```
src/components/ui/
├── breadcrumb/         # 面包屑
├── dropdown-menu/      # 下拉菜单
├── navigation-menu/    # 导航菜单
├── pagination/         # 分页
└── context-menu/       # 右键菜单
```

### 数据展示

```
src/components/ui/
├── calendar/           # 日历
├── chart/              # 图表
├── data-table/         # 数据表格
└── hover-card/         # 悬停卡片
```

## 组件使用示例

### Button 组件

```typescript
// src/components/ui/button/button.tsx
import * as React from "react"
import { Slot } from "@radix-ui/react-slot"
import { cva, type VariantProps } from "class-variance-authority"

import { cn } from "@/lib/utils"

const buttonVariants = cva(
  "inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium ring-offset-background transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive:
          "bg-destructive text-destructive-foreground hover:bg-destructive/90",
        outline:
          "border border-input bg-background hover:bg-accent hover:text-accent-foreground",
        secondary:
          "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost: "hover:bg-accent hover:text-accent-foreground",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        default: "h-10 px-4 py-2",
        sm: "h-9 rounded-md px-3",
        lg: "h-11 rounded-md px-8",
        icon: "h-10 w-10",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
)

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ className, variant, size, asChild = false, ...props }, ref) => {
    const Comp = asChild ? Slot : "button"
    return (
      <Comp
        className={cn(buttonVariants({ variant, size, className }))}
        ref={ref}
        {...props}
      />
    )
  }
)
Button.displayName = "Button"

export { Button, buttonVariants }
```

### 使用示例

```tsx
import { Button } from "@/components/ui/button"

function MyComponent() {
  return (
    <div className="flex gap-2">
      <Button>默认按钮</Button>
      <Button variant="secondary">次要按钮</Button>
      <Button variant="outline">轮廓按钮</Button>
      <Button variant="ghost">幽灵按钮</Button>
      <Button variant="destructive">危险按钮</Button>
      <Button variant="link">链接按钮</Button>
    </div>
  )
}
```

### Card 组件

```tsx
import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from "@/components/ui/card"

function ProjectCard() {
  return (
    <Card className="w-[350px]">
      <CardHeader>
        <CardTitle>项目名称</CardTitle>
        <CardDescription>项目描述信息</CardDescription>
      </CardHeader>
      <CardContent>
        <p>卡片内容...</p>
      </CardContent>
      <CardFooter className="flex justify-between">
        <Button variant="outline">取消</Button>
        <Button>确认</Button>
      </CardFooter>
    </Card>
  )
}
```

### Dialog 组件

```tsx
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogFooter,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog"

function SettingsDialog() {
  return (
    <Dialog>
      <DialogTrigger asChild>
        <Button>打开设置</Button>
      </DialogTrigger>
      <DialogContent>
        <DialogHeader>
          <DialogTitle>设置</DialogTitle>
          <DialogDescription>
            在这里配置应用程序的设置。
          </DialogDescription>
        </DialogHeader>
        <div className="py-4">
          {/* 设置表单 */}
        </div>
        <DialogFooter>
          <Button type="submit">保存更改</Button>
        </DialogFooter>
      </DialogContent>
    </Dialog>
  )
}
```

## 主题定制

### 颜色定制

Origin 使用一套品牌色系统，可以在 CSS 变量中定制：

```css
:root {
  /* 品牌色 - 蓝色系 */
  --primary: 221.2 83.2% 53.3%;
  --primary-foreground: 210 40% 98%;

  /* 成功色 */
  --success: 142.1 76.2% 36.3%;
  --success-foreground: 355.7 100% 97.3%;

  /* 警告色 */
  --warning: 32.6 94.6% 43.7%;
  --warning-foreground: 210 40% 98%;

  /* 错误色 */
  --error: 0 72.2% 50.6%;
  --error-foreground: 210 40% 98%;
}
```

### 深色模式

```tsx
import { useTheme } from "@/components/theme-provider"

function ThemeToggle() {
  const { theme, setTheme } = useTheme()

  return (
    <Button
      variant="ghost"
      size="icon"
      onClick={() => setTheme(theme === "dark" ? "light" : "dark")}
    >
      {theme === "dark" ? <SunIcon /> : <MoonIcon />}
    </Button>
  )
}
```

## 响应式设计

shadcn/ui 组件天然支持响应式，结合 Tailwind 的断点系统：

```tsx
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  <Card>项目 1</Card>
  <Card>项目 2</Card>
  <Card>项目 3</Card>
</div>
```

## 无障碍支持

所有 shadcn/ui 组件基于 Radix UI，内置完整的无障碍支持：

- **键盘导航** - Tab、Enter、Escape 等按键支持
- **ARIA 属性** - 完整的 aria-label、aria-describedby 等
- **屏幕阅读器** - 与 NVDA、JAWS 等兼容
- **焦点管理** - 自动焦点陷阱和恢复

## 性能优化

### 按需导入

```tsx
// ✅ 推荐 - 按需导入
import { Button } from "@/components/ui/button"

// ❌ 避免 - 从 barrell 文件导入
import { Button } from "@/components/ui"
```

### 代码分割

```tsx
// 动态导入大型组件
const HeavyChart = dynamic(() => import("@/components/ui/chart"), {
  loading: () => <Skeleton className="h-[200px] w-full" />,
})
```

## 最佳实践

1. **保持组件简单** - 每个 UI 组件职责单一
2. **使用 CVA** - 用 class-variance-authority 管理变体
3. **组合优于继承** - 使用组合模式构建复杂 UI
4. **TypeScript 优先** - 所有组件都有完整类型定义
5. **测试无障碍** - 定期用键盘和屏幕阅读器测试

## 添加新组件

```bash
# 方式 1: 使用 CLI (如果安装了 shadcn-ui CLI)
npx shadcn-ui@latest add [component-name]

# 方式 2: 手动复制
# 从 https://ui.shadcn.com/docs/components 复制组件代码
# 到 src/components/ui/[component-name]/
```

## 参考资源

- [shadcn/ui 官方文档](https://ui.shadcn.com)
- [Radix UI 文档](https://www.radix-ui.com)
- [Tailwind CSS 文档](https://tailwindcss.com)
- [CVA 文档](https://cva.style)
