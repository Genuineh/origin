# Origin - 技术栈详解

## 技术选型原则

1. **性能优先**：关键路径使用 Rust
2. **开发效率**：UI 层使用 React 生态
3. **跨平台**：Tauri 实现桌面应用
4. **可维护**：强类型语言为主
5. **开源优先**：避免商业依赖

## 桌面客户端技术栈

### 核心框架

| 技术 | 版本 | 用途 |
|------|------|------|
| **Tauri** | 2.0+ | 桌面应用框架 (替代 Electron) |
| **React** | 18.2+ | UI 框架 |
| **TypeScript** | 5.0+ | 类型系统 |

**为什么选择 Tauri 而不是 Electron？**

| 对比项 | Electron | Tauri |
|--------|----------|-------|
| 包大小 | ~150MB | ~10MB |
| 内存占用 | 高 | 低 |
| 后端语言 | Node.js | Rust |
| 安全性 | 较低 | 更高 |
| 启动速度 | 慢 | 快 |

### UI 组件库

| 技术 | 版本 | 用途 |
|------|------|------|
| **shadcn/ui** | - | 基础组件 (无运行时依赖) |
| **Tailwind CSS** | 3.3+ | 样式系统 |
| **Radix UI** | - | 无障碍组件底层 |
| **Lucide React** | - | 图标库 |

### 画布渲染

| 技术 | 版本 | 用途 |
|------|------|------|
| **Fabric.js** | 5.3+ | 主画布渲染 |
| **Konva.js** | - | 备选方案 |

### DSL 编辑器

| 技术 | 版本 | 用途 |
|------|------|------|
| **Monaco Editor** | 0.44+ | 代码编辑器 |
| **WebAssembly** | - | DSL Parser (未来) |

### 状态管理

| 技术 | 版本 | 用途 |
|------|------|------|
| **Zustand** | 4.4+ | 全局状态 |
| **Immer** | 10.0+ | 不可变更新 |
| **Yjs** | 13.6+ | CRDT 同步 |

### 构建工具

| 技术 | 版本 | 用途 |
|------|------|------|
| **Vite** | 5.0+ | 前端构建 |
| **pnpm** | 8.0+ | 包管理器 |

### 前端依赖结构

```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",

    "fabric": "^5.3.0",
    "monaco-editor": "^0.44.0",

    "zustand": "^4.4.0",
    "immer": "^10.0.0",
    "yjs": "^13.6.0",

    "@radix-ui/react-dialog": "^1.0.0",
    "@radix-ui/react-dropdown-menu": "^2.0.0",
    "@radix-ui/react-select": "^2.0.0",
    "@radix-ui/react-tabs": "^1.0.0",
    "@radix-ui/react-tooltip": "^1.0.0",

    "tailwindcss": "^3.3.0",
    "class-variance-authority": "^0.7.0",
    "tailwind-merge": "^2.0.0",
    "clsx": "^2.0.0",

    "lucide-react": "^0.300.0",

    "@tauri-apps/api": "^2.0.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "@vitejs/plugin-react": "^4.2.0",
    "typescript": "^5.3.0",
    "vite": "^5.0.0",
    "tailwindcss": "^3.3.0",
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "eslint": "^8.55.0",
    "prettier": "^3.1.0",
    "@tauri-apps/cli": "^2.0.0"
  }
}
```

---

## AI 集成技术栈 (前端直接接入)

### AI SDK

| 技术 | 版本 | 用途 |
|------|------|------|
| **openai** | 4.20+ | OpenAI API (GPT-4) |
| **@anthropic-ai/sdk** | 0.17+ | Anthropic API (Claude) |
| **ollama-js** | - | 本地 LLM (可选) |

### AI 依赖结构

```json
{
  "dependencies": {
    "openai": "^4.20.0",
    "@anthropic-ai/sdk": "^0.17.0"
  }
}
```

### AI 服务实现

```typescript
// AI 服务接口
interface AIService {
  configure(config: AIConfig): void;
  generateDSL(prompt: string, context: ProjectContext): Promise<string>;
  getSuggestions(document: UsdDocument, intent: string): Promise<Suggestion[]>;
  explainCode(code: string): Promise<string>;
  optimizeDesign(dsl: string): Promise<string>;
}

// Provider 配置
interface AIConfig {
  provider: 'openai' | 'anthropic' | 'ollama' | 'custom';
  apiKey?: string;
  model: string;
  baseURL?: string;
  temperature?: number;
  maxTokens?: number;
}

// OpenAI 实现
import OpenAI from 'openai';

class OpenAIService implements AIService {
  private client: OpenAI;
  private config: AIConfig;

  constructor(config: AIConfig) {
    this.config = config;
    this.client = new OpenAI({
      apiKey: config.apiKey,
      baseURL: config.baseURL,
      dangerouslyAllowBrowser: true
    });
  }

  async generateDSL(prompt: string, context: ProjectContext): Promise<string> {
    const response = await this.client.chat.completions.create({
      model: this.config.model,
      messages: [
        { role: 'system', content: this.buildSystemPrompt(context) },
        { role: 'user', content: prompt }
      ],
      temperature: this.config.temperature ?? 0.7,
      max_tokens: this.config.maxTokens ?? 4096
    });

    return response.choices[0].message.content ?? '';
  }
}

// Anthropic 实现
import Anthropic from '@anthropic-ai/sdk';

class AnthropicService implements AIService {
  private client: Anthropic;
  private config: AIConfig;

  constructor(config: AIConfig) {
    this.config = config;
    this.client = new Anthropic({
      apiKey: config.apiKey,
      baseURL: config.baseURL,
      dangerouslyAllowBrowser: true
    });
  }

  async generateDSL(prompt: string, context: ProjectContext): Promise<string> {
    const response = await this.client.messages.create({
      model: this.config.model,
      system: this.buildSystemPrompt(context),
      messages: [{ role: 'user', content: prompt }],
      max_tokens: this.config.maxTokens ?? 4096
    });

    return response.content[0].type === 'text' ? response.content[0].text : '';
  }
}
```

---

## Tauri 后端技术栈

### 核心框架

| 技术 | 版本 | 用途 |
|------|------|------|
| **Rust** | 1.75+ | 后端语言 |
| **Tauri** | 2.0+ | 桌面框架后端 |
| **Tokio** | 1.35+ | 异步运行时 |

### DSL 解析

| 技术 | 版本 | 用途 |
|------|------|------|
| **Pest** | 2.7+ | PEG 解析器 (主选) |
| **Nom** | 7.1+ | 组合子解析器 (备选) |

### 代码生成

| 技术 | 版本 | 用途 |
|------|------|------|
| **Handlebars** | 5.0+ | 模板引擎 |
| **Tera** | 1.19+ | 备选模板引擎 |

### 本地存储

| 技术 | 版本 | 用途 |
|------|------|------|
| **SQLite** | (rusqlite) | 本地数据库 |
| **serde_json** | 1.0+ | JSON 序列化 |
| **dirs** | 5.0+ | 系统目录 |

### 后端依赖结构

```toml
[dependencies]
# Tauri
tauri = { version = "2.0", features = ["shell-open"] }
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"

# 异步运行时
tokio = { version = "1.35", features = ["full"] }

# DSL 解析
pest = "2.7"
pest_derive = "2.7"

# 代码生成
handlebars = "5.0"

# 本地存储
rusqlite = { version = "0.30", features = ["bundled"] }
dirs = "5.0"

# 日志
tracing = "0.1"
tracing-subscriber = { version = "0.3", features = ["env-filter"] }

# 工具
anyhow = "1.0"
thiserror = "1.0"
uuid = { version = "1.6", features = ["v4", "serde"] }
```

---

## 云端服务技术栈 (自托管 - 可选)

### API 网关

| 技术 | 版本 | 用途 |
|------|------|------|
| **Rust** | 1.75+ | 后端语言 |
| **Actix-web** | 4.4+ | Web 框架 |
| **Axum** | 0.7+ | 备选框架 |

### 数据库

| 技术 | 版本 | 用途 |
|------|------|------|
| **PostgreSQL** | 15+ | 主数据库 |
| **sqlx** | 0.7+ | 数据库访问 |
| **Redis** | 7+ | 缓存和会话 |

### 存储服务

| 技术 | 用途 |
|------|------|
| **MinIO** | S3 兼容对象存储 |
| **PostgreSQL** | 结构化数据 |

### 云端依赖结构

```toml
# Rust API 服务
[dependencies]
actix-web = "4.4"
actix-cors = "0.7"
actix-ws = "0.3"

sqlx = { version = "0.7", features = ["runtime-tokio", "postgres", "uuid", "chrono", "json"] }
redis = { version = "0.24", features = ["tokio-comp", "connection-manager"] }

# 认证
jsonwebtoken = "9.2"
argon2 = "0.5"

# WebSocket
futures-util = "0.3"

# 日志
tracing = "0.1"
tracing-subscriber = "0.3"
```

---

## 代码生成目标技术栈

### Flutter

```yaml
# pubspec.yaml
dependencies:
  flutter:
    sdk: flutter

  # 状态管理
  flutter_riverpod: ^2.4.0

  # 路由
  go_router: ^13.0.0

  # 网络
  dio: ^5.4.0
  json_annotation: ^4.8.0

  # 本地存储
  shared_preferences: ^2.2.0
  hive_flutter: ^1.1.0

dev_dependencies:
  build_runner: ^2.4.0
  json_serializable: ^6.7.0
  flutter_lints: ^3.0.0
```

### React Native

```json
{
  "dependencies": {
    "react": "18.2.0",
    "react-native": "^0.72.0",

    "@react-navigation/native": "^6.1.0",
    "@react-navigation/native-stack": "^6.9.0",

    "zustand": "^4.4.0",
    "@tanstack/react-query": "^5.0.0",

    "axios": "^1.6.0",
    "zustand": "^4.4.0"
  }
}
```

### SwiftUI

```swift
// Package.swift
dependencies: [
    .package(url: "https://github.com/apple/swift-syntax", from: "509.0.0"),
    .package(url: "https://github.com/pointfreeco/swift-composable-architecture", from: "1.9.0")
]
```

---

## 开发工具

### 代码编辑器
- **推荐**：VS Code + rust-analyzer

### VS Code 插件

```json
{
  "recommendations": [
    "rust-lang.rust-analyzer",
    "tauri-apps.tauri-vscode",
    "usernamehw.errorlens",
    "yoavbls.pretty-ts-errors",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "orta.vscode-jest"
  ]
}
```

### Git Hooks

```bash
# .git/hooks/pre-commit
#!/bin/bash
# Rust 格式化和检查
cd src-tauri && cargo fmt --check && cargo clippy -- -D warnings

# TypeScript 格式化和检查
cd src && npm run format:check && npm run lint

# 运行测试
cargo test && npm test
```

### Docker 开发环境

```yaml
# docker-compose.dev.yml
services:
  postgres:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: origin_dev
      POSTGRES_USER: origin
      POSTGRES_PASSWORD: origin
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

---

## 性能优化策略

### 前端优化

| 技术 | 用途 |
|------|------|
| **React.memo** | 组件记忆化 |
| **useMemo/useCallback** | 值/函数记忆化 |
| **虚拟滚动** | 长列表优化 |
| **Web Worker** | DSL 解析 (未来) |
| **WASM** | 高性能计算 (未来) |

### 后端优化

| 技术 | 用途 |
|------|------|
| **Tokio** | 异步 I/O |
| **连接池** | 数据库连接复用 |
| **缓存** | Redis 热数据缓存 |
| **增量生成** | 只生成变更部分 |

---

## 安全考虑

### 桌面应用安全

- Tauri CSP (Content Security Policy)
- IPC 通信验证
- 本地数据加密

### AI API Key 安全

- API Key 加密存储在本地
- 不经过任何服务器
- 用户完全控制 Key 使用
- 支持自定义 Endpoint

### 云端服务安全

- JWT 认证
- RBAC 权限控制
- 输入验证与清理

---

## 技术决策记录 (ADR)

### ADR-001: 选择 Tauri 而非 Electron

**状态**: 已接受

**背景**: 需要构建跨平台桌面应用

**决策**：使用 Tauri 2.0

**理由**：
1. 包体积小 10 倍
2. 内存占用低
3. Rust 后端性能更好
4. 安全性更高

**后果**：
- 前端需要适配 Tauri API
- 需要学习 Rust (后端)

### ADR-002: 选择 Zustand 而非 Redux

**状态**: 已接受

**背景**: 需要全局状态管理

**决策**：使用 Zustand

**理由**：
1. API 简单直观
2. 无需 Provider 包裹
3. 包体积小
4. TypeScript 支持好

### ADR-003: 选择 Pest 而非 Nom

**状态**: 已接受

**背景**: 需要 DSL Parser

**决策**：使用 Pest (PEG)

**理由**：
1. 语法更易读
2. 错误信息更好
3. 自动生成 Parser

**备选**：Nom (性能更好，但学习曲线陡)

### ADR-004: AI 服务前端接入

**状态**: 已接受

**背景**: AI 功能实现方式

**决策**：AI 服务直接在前端调用 LLM API

**理由**：
1. 用户完全控制 API Key
2. 降低服务器成本
3. 更灵活的 Provider 选择
4. 支持本地 LLM (Ollama)

**后果**：
- 需要安全存储 API Key
- API 调用受用户网络影响
- 需要支持多种 Provider
