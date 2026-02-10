# Origin - 技术栈详解 (纯 Web 版本)

## 技术选型原则

1. **性能优先**：关键路径使用 WASM
2. **开发效率**：成熟的 React 生态
3. **零部署门槛**：纯静态部署，无需后端维护
4. **渐进增强**：可选自托管云服务
5. **开源优先**：避免商业依赖

## 纯 Web 技术栈

### 核心框架

| 技术 | 版本 | 用途 |
|------|------|------|
| **Next.js** | 14+ | 全栈框架 (可选 API Routes) |
| **React** | 18.2+ | UI 框架 |
| **TypeScript** | 5.0+ | 类型系统 |

**为什么选择 Next.js？**

| 对比项 | Vite (纯 SPA) | Next.js |
|--------|--------------|---------|
| 部署 | 静态托管 | 静态托管 / Node.js |
| API | 需要单独后端 | 内置 API Routes |
| SEO | 差 | 好 |
| 路由 | React Router | App Router |
| 服务端渲染 | 无 | 支持 |

> **注**: Origin 可以作为纯静态应用部署，Next.js 主要提供更好的开发体验和可选的服务端能力。

### UI 组件库 - shadcn/ui

**shadcn/ui 是 Origin 的主要 UI 组件库**

| 技术 | 版本 | 用途 |
|------|------|------|
| **shadcn/ui** | - | 基础组件 (复制到项目中) |
| **Tailwind CSS** | 3.3+ | 样式系统 |
| **Radix UI** | - | 无障碍组件底层 |
| **Lucide React** | - | 图标库 |
| **class-variance-authority** | - | CVA 变体管理 |
| **tailwind-merge** | - | Tailwind 类合并 |

**为什么选择 shadcn/ui？**

| 特性 | shadcn/ui | MUI | Ant Design | Chakra UI |
|------|-----------|-----|------------|-----------|
| 运行时依赖 | 无 (复制代码) | 有 | 有 | 有 |
| 包体积 | 最小 | 大 | 大 | 中 |
| 可定制性 | 完全控制 | 有限 | 有限 | 中等 |
| TypeScript | 原生支持 | 支持 | 支持 | 支持 |
| 无障碍 | Radix 原生 | 好 | 好 | 好 |
| 样式系统 | Tailwind | 自有 | 自有 | 自有 |

**shadcn/ui 的优势：**
1. **无运行时依赖** - 组件代码直接复制到项目中，完全控制
2. **Tailwind 原生** - 与项目 Tailwind 配置无缝集成
3. **Radix UI 底层** - 完善的无障碍支持和键盘导航
4. **高度可定制** - 可以直接修改组件代码
5. **渐进式采用** - 只复制需要的组件

详细的 UI 组件设计请参考 [UI_COMPONENTS.md](docs/UI_COMPONENTS.md)

### 画布渲染

| 技术 | 版本 | 用途 |
|------|------|------|
| **Fabric.js** | 5.3+ | 主画布渲染 |
| **Konva.js** | - | 备选方案 |

### DSL 编辑器

| 技术 | 版本 | 用途 |
|------|------|------|
| **Monaco Editor** | 0.44+ | 代码编辑器 |

### 状态管理

| 技术 | 版本 | 用途 |
|------|------|------|
| **Zustand** | 4.4+ | 全局状态 |
| **Immer** | 10.0+ | 不可变更新 |
| **Yjs** | 13.6+ | CRDT 同步 |

### WASM DSL 解析器

| 技术 | 版本 | 用途 |
|------|------|------|
| **Rust** | 1.75+ | Parser 实现 |
| **wasm-bindgen** | - | Rust ↔ JS 绑定 |
| **wasm-pack** | - | WASM 打包 |

### 本地存储

| 技术 | 用途 |
|------|------|
| **IndexedDB** | 项目数据缓存 |
| **Origin Private File System** | 本地文件持久化 |
| **localStorage** | 用户配置 |

### 构建工具

| 技术 | 版本 | 用途 |
|------|------|------|
| **Next.js** | 14+ | 前端构建 |
| **pnpm** | 8.0+ | 包管理器 |
| **Turbopack** | - | 增量构建 |

### 前端依赖结构

```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "next": "^14.0.0",

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
    "@radix-ui/react-popover": "^1.0.0",
    "@radix-ui/react-slider": "^1.1.0",
    "@radix-ui/react-switch": "^1.0.0",
    "@radix-ui/react-toggle": "^1.0.0",
    "@radix-ui/react-scroll-area": "^1.0.0",
    "@radix-ui/react-separator": "^1.0.0",
    "@radix-ui/react-slot": "^1.0.0",
    "@radix-ui/react-label": "^2.0.0",
    "@radix-ui/react-icons": "^1.3.0",

    "tailwindcss": "^3.3.0",
    "class-variance-authority": "^0.7.0",
    "tailwind-merge": "^2.0.0",
    "clsx": "^2.0.0",
    "tailwindcss-animate": "^1.0.7",

    "lucide-react": "^0.300.0",

    "openai": "^4.20.0",
    "@anthropic-ai/sdk": "^0.17.0",

    "idb": "^8.0.0",
    "monaco-editor-textmate": "^4.0.0"
  },
  "devDependencies": {
    "@types/react": "^18.2.0",
    "@types/react-dom": "^18.2.0",
    "typescript": "^5.3.0",
    "tailwindcss": "^3.3.0",
    "autoprefixer": "^10.4.0",
    "postcss": "^8.4.0",
    "eslint": "^8.55.0",
    "prettier": "^3.1.0"
  }
}
```

---

## WASM DSL Parser

### Rust 项目结构

```toml
# usd-parser-wasm/Cargo.toml
[package]
name = "usd-parser-wasm"
version = "0.1.0"
edition = "2021"

[lib]
crate-type = ["cdylib", "rlib"]

[dependencies]
wasm-bindgen = "0.2"
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
pest = "2.7"
pest_derive = "2.7"

[dependencies.web-sys]
version = "0.3"
features = ["console"]
```

### WASM 接口

```rust
// src/lib.rs
use wasm_bindgen::prelude::*;

#[wasm_bindgen]
pub struct UsdParser {
    // Parser 状态
}

#[wasm_bindgen]
impl UsdParser {
    #[wasm_bindgen(constructor)]
    pub fn new() -> Self {
        Self {
            // 初始化
        }
    }

    #[wasm_bindgen]
    pub fn parse(&self, source: &str) -> Result<JsValue, JsValue> {
        // 解析 DSL
        let ast = self.parse_internal(source)?;
        Ok(serde_wasm_bindgen::to_value(&ast)?)
    }

    #[wasm_bindgen]
    pub fn generate(&self, document: JsValue) -> Result<String, JsValue> {
        // 生成 DSL
        let doc: UsdDocument = serde_wasm_bindgen::from_value(document)?;
        Ok(self.generate_internal(doc)?)
    }

    #[wasm_bindgen]
    pub fn validate(&self, source: &str) -> Result<JsValue, JsValue> {
        // 验证 DSL
        let errors = self.validate_internal(source)?;
        Ok(serde_wasm_bindgen::to_value(&errors)?)
    }

    #[wasm_bindgen]
    pub fn format(&self, source: &str) -> String {
        // 格式化 DSL
        self.format_internal(source)
    }
}
```

### JavaScript 使用

```typescript
// src/lib/wasm/parser.ts
import initWasm, { UsdParser } from 'usd-parser-wasm';

let parser: UsdParser | null = null;

export async function initParser() {
  await initWasm();
  parser = new UsdParser();
}

export async function parseDSL(source: string): Promise<UsdDocument> {
  if (!parser) await initParser();
  return parser!.parse(source);
}

export async function generateDSL(document: UsdDocument): Promise<string> {
  if (!parser) await initParser();
  return parser!.generate(document);
}

export async function validateDSL(source: string): Promise<ValidationError[]> {
  if (!parser) await initParser();
  return parser!.validate(source);
}

export function formatDSL(source: string): string {
  if (!parser) initParser();
  return parser!.format(source);
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
# Rust WASM 构建检查
cd usd-parser-wasm && wasm-pack build --dev

# TypeScript 格式化和检查
cd src && npm run format:check && npm run lint

# 运行测试
npm test
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

## 部署方式

### 静态部署 (推荐)

```bash
# 构建
pnpm build

# 部署到 Vercel
vercel deploy

# 或部署到 Netlify
netlify deploy --prod

# 或自托管 Nginx
cp -r out/* /var/www/origin/
```

### Docker 部署

```dockerfile
# Dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json pnpm-lock.yaml ./
RUN npm install -g pnpm && pnpm install
COPY . .
RUN pnpm build

FROM nginx:alpine
COPY --from=builder /app/out /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
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
| **WASM** | 高性能解析 |

### 后端优化

| 技术 | 用途 |
|------|------|
| **Tokio** | 异步 I/O |
| **连接池** | 数据库连接复用 |
| **缓存** | Redis 热数据缓存 |
| **增量生成** | 只生成变更部分 |

---

## 安全考虑

### Web 应用安全

- CSP (Content Security Policy)
- 输入验证与清理
- API Key 加密存储
- HTTPS 强制

### AI API Key 安全

- API Key 加密存储在 localStorage (使用用户密码)
- 不经过任何服务器
- 用户完全控制 Key 使用
- 支持自定义 Endpoint

### 云端服务安全

- JWT 认证
- RBAC 权限控制
- 输入验证与清理

---

## 技术决策记录 (ADR)

### ADR-001: 选择纯 Web 而非 Tauri

**状态**: 已接受

**背景**: 需要构建跨平台设计工具

**决策**：使用纯 Web + TypeScript

**理由**：
1. 部署更简单 (静态托管)
2. 更新更快 (即时生效)
3. 用户门槛更低 (无需下载)
4. 避免等待 Tauri 2.0 稳定
5. 现代 Web API 功能足够

**后果**：
- 需要云存储实现协作
- 本地文件操作依赖 File System Access API
- 可后续添加桌面版本

### ADR-002: 选择 Zustand 而非 Redux

**状态**: 已接受

**背景**: 需要全局状态管理

**决策**：使用 Zustand

**理由**：
1. API 简单直观
2. 无需 Provider 包裹
3. 包体积小
4. TypeScript 支持好

### ADR-003: 选择 WASM 而非 JS Parser

**状态**: 已接受

**背景**: 需要 DSL Parser

**决策**：使用 Rust + WASM

**理由**：
1. Rust 生态成熟 (Pest)
2. WASM 性能接近原生
3. 可在浏览器中运行
4. 未来可复用 Parser 代码

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

### ADR-005: 选择 shadcn/ui 而非其他组件库

**状态**: 已接受

**背景**: UI 组件库选择

**决策**：使用 shadcn/ui

**理由**：
1. 无运行时依赖 (组件代码直接复制到项目)
2. Tailwind 原生集成
3. 基于 Radix UI，无障碍支持完善
4. 完全可定制，可以直接修改组件代码
5. 渐进式采用，只复制需要的组件

**后果**：
- 需要手动管理组件代码
- 初期设置需要更多配置
- 需要团队熟悉 Tailwind CSS
