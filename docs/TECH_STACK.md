# Origin - 技术栈详解

## 后端技术栈

### 核心框架
| 技术 | 版本 | 用途 |
|------|------|------|
| Rust | 1.75+ | 主要开发语言 |
| Tokio | 1.35+ | 异步运行时 |
| Actix-web | 4.4+ | HTTP 服务器 (备选: Axum 0.7+) |
| actix-ws | - | WebSocket 支持 |

### 数据库
| 技术 | 版本 | 用途 |
|------|------|------|
| PostgreSQL | 15+ | 主数据库 |
| sqlx | 0.7+ | 数据库访问 |
| Redis | 7+ | 缓存和会话 |

### 序列化
| 技术 | 版本 | 用途 |
|------|------|------|
| serde | 1.0+ | 序列化/反序列化 |
| serde_json | 1.0+ | JSON 支持 |

### AI 集成
| 技术 | 版本 | 用途 |
|------|------|------|
| reqwest | 0.11+ | HTTP 客户端 (调用 LLM API) |
| async-openai | - | OpenAI SDK (备选) |

### OpenAPI 解析
| 技术 | 版本 | 用途 |
|------|------|------|
| serde | - | OpenAPI JSON/YAML 解析 |
| anyhow | - | 错误处理 |

### 代码质量
| 技术 | 版本 | 用途 |
|------|------|------|
| clippy | - | Linting |
| rustfmt | - | 代码格式化 |
| cargo-test | - | 测试框架 |

### 后端依赖结构
```toml
# Cargo.toml 预览

[dependencies]
# Web 框架
actix-web = "4.4"
actix-cors = "0.7"
actix-ws = "0.3"

# 异步运行时
tokio = { version = "1.35", features = ["full"] }

# 数据库
sqlx = { version = "0.7", features = ["runtime-tokio", "postgres", "uuid", "chrono", "json"] }
redis = { version = "0.24", features = ["tokio-comp", "connection-manager"] }

# 序列化
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"

# 工具
uuid = { version = "1.6", features = ["v4", "serde"] }
chrono = { version = "0.4", features = ["serde"] }
anyhow = "1.0"
thiserror = "1.0"

# AI 集成
reqwest = { version = "0.11", features = ["json"] }

# 代码生成
handlebars = "5.0"  # 模板引擎

# 日志
tracing = "0.1"
tracing-subscriber = { version = "0.3", features = ["env-filter"] }

# 配置
config = "0.14"
dotenv = "0.15"
```

---

## 前端技术栈

### 核心框架
| 技术 | 版本 | 用途 |
|------|------|------|
| React | 18.2+ | UI 框架 |
| TypeScript | 5.0+ | 类型系统 |
| Vite | 5.0+ | 构建工具 |

### 画布渲染
| 技术 | 版本 | 用途 |
|------|------|------|
| Fabric.js | 5.3+ | 主选项: 画布渲染 |
| 备选: Konva.js | - | 替代方案 |

### 状态管理
| 技术 | 版本 | 用途 |
|------|------|------|
| Zustand | 4.4+ | 全局状态 (主选项) |
| 备选: Jotai | - | 原子化状态 (备选) |

### UI 组件
| 技术 | 版本 | 用途 |
|------|------|------|
| shadcn/ui | - | 基础组件库 |
| Tailwind CSS | 3.3+ | 样式系统 |
| class-variance-authority | - | 样式变体 |
| clsx / cn | - | 类名合并 |

### 路由
| 技术 | 版本 | 用途 |
|------|------|------|
| React Router | 6.20+ | 页面路由 |

### 网络请求
| 技术 | 版本 | 用途 |
|------|------|------|
| axios | 1.6+ | HTTP 客户端 |
| 或: ky | - | 轻量级替代 |

### WebSocket
| 技术 | 版本 | 用途 |
|------|------|------|
| WebSocket API | 原生 | 实时通信 |
| 或: socket.io-client | - | 更高级的抽象 |

### 前端依赖结构
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.20.0",

    "fabric": "^5.3.0",

    "zustand": "^4.4.0",

    "@radix-ui/react-dialog": "^1.0.5",
    "@radix-ui/react-dropdown-menu": "^2.0.6",
    "@radix-ui/react-select": "^2.0.0",
    "@radix-ui/react-slider": "^1.1.2",
    "@radix-ui/react-tabs": "^1.0.4",
    "@radix-ui/react-tooltip": "^1.0.7",

    "tailwindcss": "^3.3.0",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.0.0",
    "tailwind-merge": "^2.2.0",

    "axios": "^1.6.0",
    "socket.io-client": "^4.6.0",

    "lucide-react": "^0.300.0"
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
    "prettier": "^3.1.0"
  }
}
```

---

## 代码生成技术栈

### 模板引擎
| 技术 | 用途 |
|------|------|
| Handlebars (Rust) | 服务端模板 |
| Tera (Rust) | 备选方案 |

### 生成目标
| 框架 | 语言 | 优先级 |
|------|------|--------|
| Flutter | Dart | P0 |
| React Native | TypeScript | P2 |
| SwiftUI | Swift | P3 |

### Flutter 依赖 (生成项目)
```yaml
# 生成项目的 pubspec.yaml
dependencies:
  flutter:
    sdk: flutter

  # 状态管理
  flutter_riverpod: ^2.4.0
  # 备选: bloc, provider

  # 路由
  go_router: ^13.0.0

  # 网络请求
  dio: ^5.4.0
  json_annotation: ^4.8.0

  # 本地存储
  shared_preferences: ^2.2.0

  # UI 组件
  cached_network_image: ^3.3.0

dev_dependencies:
  flutter_test:
    sdk: flutter

  build_runner: ^2.4.0
  json_serializable: ^6.7.0
  flutter_lints: ^3.0.0
```

---

## 开发工具

### 代码编辑器
- **推荐**: VS Code / IntelliJ IDEA + Rust 插件

### VS Code 插件
```json
{
  "recommendations": [
    "rust-lang.rust-analyzer",
    "tamasfe.even-better-toml",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "usernamehw.errorlens",
    "yoavbls.pretty-ts-errors"
  ]
}
```

### 测试工具
| 技术 | 用途 |
|------|------|
| cargo test | Rust 单元测试 |
| rustdoc | 文档测试 |
| Vitest | React 单元测试 |
| Playwright | E2E 测试 (未来) |

### 容器化
```yaml
# docker-compose.yml 开发环境
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: origin_dev
      POSTGRES_USER: origin
      POSTGRES_PASSWORD: password
    ports:
      - "5432:5432"

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
```

---

## 部署技术栈

### 后端部署
| 技术 | 用途 |
|------|------|
| Docker | 容器化 |
| Kubernetes (未来) | 编排 |
| Nginx | 反向代理 |

### 前端部署
| 技术 | 用途 |
|------|------|
| Vite Build | 静态资源打包 |
| Nginx / CDN | 静态资源服务 |

### 监控 (未来)
| 技术 | 用途 |
|------|------|
| Prometheus | 指标收集 |
| Grafana | 监控面板 |
| Sentry | 错误追踪 |

---

## 技术决策记录

### 为什么选择 Rust?
- 性能: 零成本抽象，内存安全
- 并发: Tokio 异步运行时成熟
- 类型系统: 强类型减少运行时错误
- WebAssembly: 前端未来优化可能

### 为什么选择 Actix-web (备选 Axum)?
- Actix-web: 成熟稳定，性能优秀
- Axum: Tokio 生态，类型安全路由
- 决策: 先用 Actix-web，评估 Axum

### 为什么选择 Fabric.js?
- 功能丰富，文档完善
- 性能优秀
- 社区活跃

### 为什么选择 Zustand?
- 简单直观，API 清晰
- 无需 Provider 包裹
- TypeScript 支持好
- 包体积小

### 为什么优先支持 Flutter?
- 跨平台能力最强
- 性能接近原生
- Google 生态支持
- UI 一致性好

### 为什么选择 Riverpod?
- Provider v2，类型安全
- 编译时安全
- 自动依赖追踪
- 测试友好
