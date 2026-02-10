# Origin

> AI-First Design-to-Code Platform - 双向驱动的应用生成平台

Origin 是一个基于 **统一语义 DSL (USD)** 的下一代应用开发平台。通过 **可视化编辑**、**DSL/代码编辑**、**AI 自然语言** 三种方式的双向同步，实现真正的"设计即代码"。

## 核心理念

```
┌─────────────────────────────────────────────────────────────────┐
│                    用户交互层（多模态输入）                      │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐ │
│  │ 可视化画布   │  │ 代码/DSL    │  │ 自然语言助手            │ │
│  │ (Design)    │←→│ (Code)      │←→│ (AI Assistant)          │ │
│  │ 设计师主导   │  │ 开发者主导   │  │ 产品经理/业务主导        │ │
│  └──────┬──────┘  └──────┬──────┘  └───────────┬─────────────┘ │
│         └─────────────────┼─────────────────────┘               │
│                           ↓                                     │
│              ┌─────────────────────┐                            │
│              │   统一语义 DSL 层    │ ← 核心抽象，双向同步        │
│              │  (Single Source of   │   任何改动实时反映到其他视图  │
│              │       Truth)         │                            │
│              └──────────┬──────────┘                            │
│                         ↓                                       │
│              ┌─────────────────────┐                            │
│              │   多目标生成引擎     │                            │
│              │  Flutter/React/小程序 │                            │
│              └─────────────────────┘                            │
└─────────────────────────────────────────────────────────────────┘
```

## 主要特性

### 三种编辑方式，完全同步
- **可视化编辑器**：类 Penpot/Figma 的画布，拖拽设计
- **DSL 编辑器**：强类型语义语言，精确控制
- **AI 助手**：自然语言描述，自动生成

### 统一语义 DSL (USD)
- **8 大语义**：结构、状态、数据、交互、主题、能力、路由、业务
- **声明式语法**：描述"是什么"而非"怎么做"
- **双向实时同步**：任何视图修改，其他视图实时更新

### 多目标代码生成
- **Flutter** (Dart) - 优先支持
- **React Native** (TypeScript)
- **SwiftUI** (Swift)
- **小程序** (多平台)

### AI 深度集成
- 前端直接调用 LLM，用户完全控制
- 支持 OpenAI、Anthropic、Ollama 等
- 意图理解与 DSL 生成
- 智能组件推荐
- 设计建议与优化

### 无使用限制
- 无速率限制
- 无项目数量限制
- 无协作用户数限制
- 完全开源，自主部署

## 技术栈

### 纯 Web 前端
- **Next.js 14** + **React 18** + **TypeScript**
- **shadcn/ui** - UI 组件库 (基于 Radix UI + Tailwind CSS)
- **Fabric.js** - 画布渲染
- **Monaco Editor** - DSL 编辑器
- **Zustand** - 状态管理
- **Yjs** - CRDT 同步
- **IndexedDB** - 本地存储

### WASM DSL Parser
- **Rust** + **wasm-bindgen**
- **Pest** - PEG 解析器
- 浏览器内高性能解析

### AI 集成 (前端)
- **openai** - OpenAI SDK
- **@anthropic-ai/sdk** - Anthropic SDK
- **ollama-js** - 本地 LLM 支持

### 云端服务 (可选，自托管)
- **Rust** + **Actix-web** / **Axum**
- **PostgreSQL** - 主数据库
- **Redis** - 缓存和会话
- **WebSocket** - 实时协作

## 项目状态

**开发阶段**: 架构设计完成，开发中

## 文档

- [项目概述](docs/PROJECT_OVERVIEW.md) - 项目愿景和核心特性
- [系统架构](docs/ARCHITECTURE.md) - 技术架构和模块设计
- [统一语义 DSL 规范](docs/DSL_SPEC_V2.md) - USD v2 完整语法
- [工作流程](docs/WORKFLOW.md) - 三种编辑方式的使用流程
- [技术栈](docs/TECH_STACK_WEB.md) - 技术选型详解
- [UI 组件](docs/UI_COMPONENTS.md) - shadcn/ui 组件系统
- [API 文档](docs/API.md) - API 规范
- [任务清单](docs/TODO.md) - 开发任务和里程碑

## 快速开始

### 环境要求

- Node.js 20+
- pnpm 8+
- Rust 1.75+ (仅用于 WASM Parser 编译)

### 本地开发

```bash
# 克隆项目
git clone https://github.com/yourusername/origin.git
cd origin

# 安装依赖
pnpm install

# 构建 WASM Parser
cd usd-parser-wasm
wasm-pack build --target web --out-dir ../src/lib/wasm/pkg
cd ..

# 启动开发环境
pnpm dev
```

访问 http://localhost:3000

### 构建生产版本

```bash
# 构建静态文件
pnpm build

# 预览构建结果
pnpm preview
```

### 部署

```bash
# 部署到 Vercel (推荐)
vercel deploy

# 或部署到 Netlify
netlify deploy --prod

# 或自托管 Nginx
cp -r out/* /var/www/origin/
```

### 自托管后端 (可选，用于协作)

```bash
# 启动后端服务
docker-compose up -d

# 或使用 Rust 直接运行
cd backend && cargo run
```

## 路线图

### Phase 1: 语义核心 (8 周)
- ✅ USD v2 语法设计
- ⏳ WASM DSL Parser (Rust)
- ⏳ 可视化编辑器基础
- ⏳ 双向同步机制
- ⏳ Flutter 生成器 MVP

### Phase 2: 完整设计体验 (6 周)
- ⏳ 矢量编辑
- ⏳ 自动布局
- ⏳ 组件系统
- ⏳ 多人协作

### Phase 3: 语义增强 (6 周)
- ⏳ 状态可视化
- ⏳ 数据绑定
- ⏳ 交互设计
- ⏳ AI 集成

### Phase 4: 生成与部署 (4 周)
- ⏳ 多目标生成器
- ⏳ CI/CD 集成
- ⏳ 一键部署

## 贡献

欢迎贡献！请查看 [贡献指南](docs/CONTRIBUTING.md)。

## 对比传统方案

| 特性 | Figma | Penpot | FlutterFlow | Origin |
|------|-------|--------|------------|--------|
| 可视化设计 | ✅ | ✅ | ✅ | ✅ |
| DSL/代码编辑 | ❌ | ❌ | ❌ | ✅ |
| AI 自然语言 | ❌ | ❌ | ⚠️ | ✅ |
| 代码质量 | 手写 | 手写 | 平台锁定 | 可维护 |
| 多框架支持 | ❌ | ❌ | ❌ | ✅ |
| 自托管 | ❌ | ✅ | ❌ | ✅ |
| 使用限制 | 有 | ❌ | 有 | ❌ |
| 部署方式 | 桌面/Web | Web | Web | 纯 Web |

## 许可证

MIT License - 详见 [LICENSE](LICENSE)

---

**Origin** - 让设计与代码无缝融合
