# Origin - 系统架构设计

## 整体架构

```
┌─────────────────────────────────────────────────────────────────┐
│                         Frontend (Web)                          │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │  Canvas View │  │ Properties   │  │  Component   │          │
│  │              │  │  Panel       │  │  Library     │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
└─────────────────────────────────────────────────────────────────┘
                              ↓ WebSocket
┌─────────────────────────────────────────────────────────────────┐
│                      Backend (Rust)                             │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    API Layer                              │  │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐         │  │
│  │  │ Project │ │ Design  │ │  DSL    │ │ Export  │         │  │
│  │  │   API   │ │   API   │ │   API   │ │   API   │         │  │
│  │  └─────────┘ └─────────┘ └─────────┘ └─────────┘         │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    Core Services                          │  │
│  │  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐   │  │
│  │  │   Document    │ │    Render     │ │    History    │   │  │
│  │  │   Engine      │ │    Engine     │ │    Manager    │   │  │
│  │  └───────────────┘ └───────────────┘ └───────────────┘   │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    DSL & Generator                        │  │
│  │  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐   │  │
│  │  │   DSL Parser  │ │   Model       │ │   Code        │   │  │
│  │  │               │ │   Analyzer    │ │   Generator   │   │  │
│  │  └───────────────┘ └───────────────┘ └───────────────┘   │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    AI Services                            │  │
│  │  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐   │  │
│  │  │  Design       │ │  Component    │ │  Layout       │   │  │
│  │  │  Assistant    │ │  Recommender  │ │  Optimizer    │   │  │
│  │  └───────────────┘ └───────────────┘ └───────────────┘   │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    Integrations                           │  │
│  │  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐   │  │
│  │  │   OpenAPI     │ │  Asset        │ │  Git          │   │  │
│  │  │   Importer    │ │  Manager      │ │  Integration  │   │  │
│  │  └───────────────┘ └───────────────┘ └───────────────┘   │  │
│  └──────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────┐
│                      Storage Layer                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ PostgreSQL   │  │    Redis     │  │   S3/MinIO   │          │
│  │ (Projects)   │  │   (Cache)    │  │   (Assets)   │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
└─────────────────────────────────────────────────────────────────┘
```

## 核心模块

### 1. Document Engine (文档引擎)
负责设计文档的存储、序列化、协作编辑。

**主要功能：**
- 文档树结构管理
- 增量更新
- 操作历史
- 冲突解决

**数据结构：**
```rust
struct Document {
    id: Uuid,
    version: u64,
    pages: Vec<Page>,
    components: Vec<Component>,
    assets: Vec<Asset>,
    metadata: DocumentMetadata,
}

struct Page {
    id: Uuid,
    name: String,
    children: Vec<Layer>,
    viewport: Viewport,
}
```

### 2. Render Engine (渲染引擎)
负责画布渲染、图层管理、布局计算。

**主要功能：**
- 图形绘制（矩形、圆形、路径、文本等）
- 图层变换（位置、旋转、缩放）
- 布局约束系统
- 样式计算

### 3. DSL System (领域特定语言)
定义中间表示语言，用于描述 UI 和行为。

**DSL 设计示例：**
```
Component LoginScreen {
    State {
        username: String
        password: String
        isLoading: Boolean
        error: String?
    }

    Layout {
        Container {
            direction: column
            padding: 24

            TextField("Username") bind username
            TextField("Password") bind password secret
            Button("Login") onClick loginAction
        }
    }

    Action loginAction {
        api: auth.login
        request: { username, password }
        onSuccess: navigateTo(HomeScreen)
        onError: error = message
    }
}
```

### 4. Model Analyzer (模型分析器)
分析设计文档，提取状态、行为、数据流信息。

**分析内容：**
- 状态识别（输入框、选择器等）
- 事件流分析（点击、导航等）
- API 调用识别
- 页面导航关系

### 5. Code Generator (代码生成器)
将 DSL 转换为目标框架代码。

**目标框架：**
- Flutter (Dart) - 优先
- React Native (TypeScript)
- SwiftUI (Swift)

**生成策略：**
- 模板化生成
- 自定义代码片段
- 项目结构管理

### 6. OpenAPI Integration
导入和管理后端 API 定义。

**功能：**
- OpenAPI JSON 解析
- API 类型生成
- 请求/响应建模
- API 调用代码生成

### 7. AI Services
提供 AI 辅助功能。

**功能：**
- 自然语言转 UI
- 布局优化建议
- 组件推荐
- 设计规范检查

## 技术选型

### Backend
- **语言**: Rust 1.75+
- **Web 框架**: Actix-web / Axum
- **WebSocket**: actix-ws / tokio-tungstenite
- **数据库**: PostgreSQL (sqlx)
- **缓存**: Redis
- **异步运行时**: Tokio
- **序列化**: Serde

### Frontend
- **框架**: React + TypeScript
- **构建工具**: Vite
- **画布**: Fabric.js / Konva.js
- **状态管理**: Zustand / Jotai
- **UI 组件**: shadcn/ui

### AI 集成
- **LLM**: OpenAI API / Anthropic Claude API
- **向量搜索**: (未来) pgvector

## 项目结构

```
origin/
├── backend/                 # Rust 后端
│   ├── Cargo.toml
│   ├── src/
│   │   ├── main.rs
│   │   ├── api/            # API handlers
│   │   ├── core/           # 核心服务
│   │   │   ├── document/   # 文档引擎
│   │   │   ├── render/     # 渲染引擎
│   │   │   └── history/    # 历史管理
│   │   ├── dsl/            # DSL 系统
│   │   │   ├── parser.rs
│   │   │   ├── analyzer.rs
│   │   │   └── generator/  # 代码生成器
│   │   ├── ai/             # AI 服务
│   │   ├── integration/    # 外部集成
│   │   │   ├── openapi.rs
│   │   │   └── git.rs
│   │   └── db/             # 数据库层
│   └── tests/
├── frontend/               # React 前端
│   ├── package.json
│   ├── src/
│   │   ├── main.tsx
│   │   ├── App.tsx
│   │   ├── components/     # UI 组件
│   │   ├── canvas/         # 画布相关
│   │   ├── panels/         # 属性面板
│   │   ├── stores/         # 状态管理
│   │   └── api/            # API 客户端
│   └── public/
├── docs/                   # 文档
│   ├── PROJECT_OVERVIEW.md
│   ├── ARCHITECTURE.md
│   ├── WORKFLOW.md
│   ├── TODO.md
│   └── TECH_STACK.md
└── templates/              # 代码生成模板
    ├── flutter/
    ├── react-native/
    └── swiftui/
```

## 数据流

### 设计到代码流程

```
1. 用户在画布上创建设计
   ↓
2. Document Engine 保存设计数据
   ↓
3. 用户配置建模信息（状态、API调用、导航）
   ↓
4. Model Analyzer 分析设计，提取模型信息
   ↓
5. DSL Parser 生成 DSL 表示
   ↓
6. Code Generator 读取 DSL 和模板
   ↓
7. 生成目标框架代码
```

### AI 辅助流程

```
1. 用户输入自然语言描述
   ↓
2. AI Service 调用 LLM
   ↓
3. 返回结构化设计建议
   ↓
4. 应用到画布
   ↓
5. 用户可继续调整
```
