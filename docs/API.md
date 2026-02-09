# Origin API 文档

## 概述

Origin API 包含两部分：
1. **Tauri IPC Commands** - 桌面应用本地 API
2. **云端 REST API** - 可选的云端服务 API

**重要**: Origin 无任何速率限制，所有 API 调用无限制。

---

## Tauri IPC Commands

### 项目管理

#### 创建项目
```rust
invoke('create_project', {
  name: string,
  description?: string,
  framework: 'flutter' | 'react-native' | 'swiftui',
  platforms: string[]
}) => Promise<Project>
```

#### 获取项目列表
```rust
invoke('list_projects') => Promise<Project[]>
```

#### 获取项目详情
```rust
invoke('get_project', {
  id: string
}) => Promise<Project>
```

#### 保存项目
```rust
invoke('save_project', {
  project: Project
}) => Promise<void>
```

#### 删除项目
```rust
invoke('delete_project', {
  id: string
}) => Promise<void>
```

---

### 文档操作

#### 获取文档
```rust
invoke('get_document', {
  projectId: string
}) => Promise<UsdDocument>
```

#### 更新文档
```rust
invoke('update_document', {
  projectId: string,
  operations: DeltaOperation[]
}) => Promise<void>
```

#### 验证文档
```rust
invoke('validate_document', {
  dsl: string
}) => Promise<ValidationResult>
```

#### 格式化文档
```rust
invoke('format_document', {
  dsl: string
}) => Promise<string>
```

---

### DSL 操作

#### 解析 DSL
```rust
invoke('parse_dsl', {
  source: string
}) => Promise<UsdDocument>
```

#### 生成 DSL
```rust
invoke('generate_dsl', {
  document: UsdDocument
}) => Promise<string>
```

#### 获取 DSL 语法
```rust
invoke('get_dsl_grammar') => Promise<string>
```

---

### 代码生成

#### 生成代码
```rust
invoke('generate_code', {
  projectId: string,
  target: 'flutter' | 'react-native' | 'swiftui',
  options?: GenerationOptions
}) => Promise<GeneratedProject>
```

#### 获取生成历史
```rust
invoke('get_generation_history', {
  projectId: string
}) => Promise<Generation[]>
```

---

### OpenAPI 集成

#### 导入 OpenAPI
```rust
invoke('import_openapi', {
  projectId: string,
  source: 'url' | 'file' | 'content',
  content?: string
}) => Promise<OpenAPIImport>
```

#### 获取 API 列表
```rust
invoke('list_apis', {
  projectId: string
}) => Promise<ApiDefinition[]>
```

---

### AI 操作 (前端直接调用，非 IPC)

**注意**: AI 功能直接在前端实现，不通过 Tauri IPC。

#### AI 配置管理 (本地存储)

```typescript
// 前端 Zustand Store
interface AIConfigStore {
  config: AIConfig;
  setConfig: (config: AIConfig) => void;
  getProvider: () => AIService;
}

// 配置结构
interface AIConfig {
  provider: 'openai' | 'anthropic' | 'ollama' | 'custom';
  apiKey?: string;
  model: string;
  baseURL?: string;
  temperature?: number;
  maxTokens?: number;
}
```

#### AI 服务接口 (前端)

```typescript
interface AIService {
  // 配置
  configure(config: AIConfig): void;

  // 生成 DSL
  generateDSL(prompt: string, context: ProjectContext): Promise<string>;

  // 获取建议
  getSuggestions(document: UsdDocument, intent: string): Promise<Suggestion[]>;

  // 解释代码
  explainCode(code: string): Promise<string>;

  // 优化设计
  optimizeDesign(dsl: string): Promise<string>;
}
```

---

## 云端 REST API (可选)

### 基础 URL

```
本地开发: http://localhost:8080
生产环境: https://api.origin.app (自托管)
```

### 认证

```http
Authorization: Bearer <jwt_token>
```

---

### 项目管理

#### 创建项目
```http
POST /api/v1/projects
Content-Type: application/json

{
  "name": "My App",
  "description": "A mobile application",
  "framework": "flutter",
  "platforms": ["ios", "android", "web"]
}

Response: 201 Created
{
  "id": "uuid",
  "name": "My App",
  "createdAt": "2024-01-15T10:00:00Z",
  "updatedAt": "2024-01-15T10:00:00Z"
}
```

#### 获取项目列表
```http
GET /api/v1/projects

Response: 200 OK
{
  "projects": [...],
  "total": 10,
  "page": 1,
  "pageSize": 20
}
```

#### 获取项目详情
```http
GET /api/v1/projects/:id

Response: 200 OK
{
  "id": "uuid",
  "name": "My App",
  "document": {...},
  "createdAt": "2024-01-15T10:00:00Z"
}
```

#### 更新项目
```http
PUT /api/v1/projects/:id
Content-Type: application/json

{
  "name": "Updated Name"
}

Response: 200 OK
```

#### 删除项目
```http
DELETE /api/v1/projects/:id

Response: 204 No Content
```

---

### 文档操作

#### 获取文档
```http
GET /api/v1/projects/:projectId/document

Response: 200 OK
{
  "version": "2.0",
  "screens": [...],
  "components": [...],
  ...
}
```

#### 更新文档
```http
PATCH /api/v1/projects/:projectId/document
Content-Type: application/json

{
  "operations": [
    {
      "type": "add",
      "path": "/screens/0",
      "value": {...}
    }
  ]
}

Response: 200 OK
```

#### 导出文档
```http
GET /api/v1/projects/:projectId/export
?format=flutter&includeTests=true

Response: 200 OK
Content-Type: application/zip
```

---

### OpenAPI 集成

#### 导入 OpenAPI 规范
```http
POST /api/v1/projects/:projectId/openapi
Content-Type: application/json

{
  "url": "https://api.example.com/openapi.json",
  "spec": {...}
}

Response: 201 Created
{
  "id": "uuid",
  "endpoints": [...],
  "types": [...]
}
```

#### 获取 API 列表
```http
GET /api/v1/projects/:projectId/apis

Response: 200 OK
{
  "apis": [...]
}
```

---

### 代码生成

#### 生成代码
```http
POST /api/v1/projects/:projectId/generate
Content-Type: application/json

{
  "target": "flutter",
  "options": {
    "includeTests": true,
    "stateProvider": "riverpod",
    "router": "go_router"
  }
}

Response: 201 Created
{
  "id": "uuid",
  "downloadUrl": "https://...",
  "expiresAt": "2024-01-15T12:00:00Z"
}
```

#### 下载代码
```http
GET /api/v1/builds/:buildId/download

Response: 200 OK
Content-Type: application/zip
Content-Disposition: attachment; filename="myapp.zip"
```

---

### 协作功能

#### WebSocket 连接

```
ws://localhost:8080/ws?token=<jwt_token>
```

#### 消息格式

**客户端 → 服务器**

```json
{
  "type": "operation",
  "projectId": "uuid",
  "documentId": "uuid",
  "operation": {
    "type": "add",
    "path": "/screens/0/children/0",
    "value": {...}
  }
}
```

**服务器 → 客户端**

```json
{
  "type": "operation",
  "projectId": "uuid",
  "documentId": "uuid",
  "operation": {...},
  "userId": "user-uuid",
  "timestamp": 1234567890
}
```

#### 消息类型

| 类型 | 方向 | 描述 |
|------|------|------|
| `operation` | 双向 | 文档操作 |
| `selection` | 双向 | 选择状态同步 |
| `cursor` | 双向 | 光标位置 |
| `presence` | 服务器 | 用户在线状态 |
| `comment` | 双向 | 评论消息 |
| `error` | 服务器 | 错误信息 |

---

## 数据模型

### Project

```typescript
interface Project {
  id: string;
  name: string;
  description?: string;
  framework: 'flutter' | 'react-native' | 'swiftui';
  platforms: string[];
  document: UsdDocument;
  metadata: {
    createdAt: string;
    updatedAt: string;
    version: number;
  };
}
```

### UsdDocument

```typescript
interface UsdDocument {
  version: string;
  metadata: {
    name: string;
    description?: string;
  };
  imports: Import[];
  theme: Theme;
  entities: Entity[];
  apis: ApiDefinition[];
  screens: Screen[];
  components: Component[];
  globalState?: GlobalState;
  flows?: Flow[];
}
```

### AIConfig

```typescript
interface AIConfig {
  provider: 'openai' | 'anthropic' | 'ollama' | 'custom';
  apiKey?: string;
  model: string;
  baseURL?: string;
  temperature?: number;
  maxTokens?: number;
}
```

### GenerationOptions

```typescript
interface GenerationOptions {
  // 通用选项
  includeTests?: boolean;
  includeComments?: boolean;

  // Flutter 特定
  stateProvider?: 'riverpod' | 'bloc' | 'provider';
  router?: 'go_router' | 'auto_route';
  includeFreezed?: boolean;

  // React Native 特定
  navigationLibrary?: 'react-navigation' | 'react-router-native';
  stateLibrary?: 'zustand' | 'redux' | 'jotai';
  queryLibrary?: 'tanstack-query' | 'swr';

  // 格式化
  formatCode?: boolean;
  formatter?: 'prettier' | 'biome';
}
```

---

## 错误码

| 状态码 | 错误类型 | 描述 |
|--------|----------|------|
| 400 | Bad Request | 请求参数错误 |
| 401 | Unauthorized | 未认证 |
| 403 | Forbidden | 无权限 |
| 404 | Not Found | 资源不存在 |
| 409 | Conflict | 资源冲突 |
| 422 | Unprocessable Entity | 验证失败 |
| 500 | Internal Server Error | 服务器错误 |

### 错误响应格式

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input",
    "details": [
      {
        "field": "name",
        "message": "Name is required"
      }
    ]
  }
}
```

---

## 速率限制

**Origin 无速率限制**。

自托管部署时，如需添加速率限制（可选），可在配置中设置：

```toml
# config.toml
[rate_limiting]
enabled = false  # 默认关闭

# 可选配置
# enabled = true
# requests_per_minute = 10000
```

---

## 版本控制

API 使用语义化版本控制。

主版本变更时，会在 URL 中体现（如 `/v2/`）。

---

## SDK

### TypeScript/JavaScript

```bash
npm install @origin/sdk
```

```typescript
import { OriginClient } from '@origin/sdk';

const client = new OriginClient({
  endpoint: 'http://localhost:8080',
  token: 'your-jwt-token'  // 云端协作时需要
});

const project = await client.projects.create({
  name: 'My App',
  framework: 'flutter'
});
```

### Rust

```toml
[dependencies]
origin-sdk = "0.1.0"
```

```rust
use origin_sdk::Client;

let client = Client::new("http://localhost:8080", "your-jwt-token");
let project = client.create_project(CreateProjectRequest {
    name: "My App".to_string(),
    framework: Framework::Flutter,
}).await?;
```

### Python

```bash
pip install origin-sdk
```

```python
from origin_sdk import Client

client = Client(endpoint="http://localhost:8080", token="your-jwt-token")
project = client.projects.create(
    name="My App",
    framework="flutter"
)
```
