# API 文档

## 概述

Origin API 采用 RESTful 风格，基于 WebSocket 进行实时协作。

## 基础 URL

```
开发环境: http://localhost:8080/api/v1
生产环境: https://api.origin.app/v1
```

## 认证

使用 Bearer Token 认证：

```http
Authorization: Bearer <token>
```

## REST API

### 项目管理

#### 创建项目
```http
POST /projects
Content-Type: application/json

{
  "name": "My App",
  "description": "A mobile application",
  "framework": "flutter",
  "platforms": ["ios", "android", "web"]
}
```

#### 获取项目列表
```http
GET /projects
```

#### 获取项目详情
```http
GET /projects/:id
```

#### 更新项目
```http
PUT /projects/:id
Content-Type: application/json

{
  "name": "Updated Name"
}
```

#### 删除项目
```http
DELETE /projects/:id
```

### 文档操作

#### 获取文档
```http
GET /projects/:projectId/documents/:documentId
```

#### 更新文档
```http
PUT /projects/:projectId/documents/:documentId
Content-Type: application/json

{
  "operations": [
    {
      "type": "add",
      "path": "/pages/0/children",
      "value": { ... }
    }
  ]
}
```

#### 导出文档
```http
GET /projects/:projectId/documents/:documentId/export
?format=flutter&includeTests=true
```

### OpenAPI 集成

#### 导入 OpenAPI 规范
```http
POST /projects/:projectId/openapi
Content-Type: application/json

{
  "url": "https://api.example.com/openapi.json",
  "spec": {...}  // 或直接提供规范
}
```

#### 获取 API 列表
```http
GET /projects/:projectId/apis
```

### 代码生成

#### 生成代码
```http
POST /projects/:projectId/generate
Content-Type: application/json

{
  "target": "flutter",
  "options": {
    "includeTests": true,
    "stateProvider": "riverpod",
    "router": "go_router"
  }
}
```

#### 下载代码
```http
GET /projects/:projectId/builds/:buildId/download
```

### AI 功能

#### 设计建议
```http
POST /ai/suggestions
Content-Type: application/json

{
  "prompt": "创建一个登录页面",
  "context": {
    "framework": "flutter",
    "screenSize": "mobile"
  }
}
```

## WebSocket API

### 连接

```
ws://localhost:8080/ws?token=<token>&projectId=<projectId>
```

### 消息格式

#### 客户端 → 服务器

```json
{
  "type": "operation",
  "documentId": "uuid",
  "operation": {
    "type": "add",
    "path": "/pages/0/children/0",
    "value": {...}
  }
}
```

#### 服务器 → 客户端

```json
{
  "type": "operation",
  "documentId": "uuid",
  "operation": {...},
  "userId": "user-uuid",
  "timestamp": 1234567890
}
```

### 消息类型

| 类型 | 方向 | 描述 |
|------|------|------|
| operation | 双向 | 文档操作 |
| selection | 双向 | 选择状态同步 |
| cursor | 双向 | 光标位置 |
| presence | 服务器 | 用户在线状态 |
| error | 服务器 | 错误信息 |

## 数据模型

### Document

```typescript
interface Document {
  id: string;
  projectId: string;
  version: number;
  pages: Page[];
  components: Component[];
  assets: Asset[];
  metadata: DocumentMetadata;
}

interface Page {
  id: string;
  name: string;
  children: Layer[];
  viewport: Viewport;
}

interface Layer {
  id: string;
  type: 'frame' | 'rectangle' | 'ellipse' | 'text' | 'image' | 'component';
  name?: string;
  x: number;
  y: number;
  width: number;
  height: number;
  rotation?: number;
  style?: Style;
  children?: Layer[];
  // 类型特定属性
}
```

### DSL (简化)

```
Component <Name> {
    State {
        <variable>: <Type>
        ...
    }

    Layout {
        <layout definition>
    }

    Action <name> {
        <action definition>
    }
}
```

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

## 速率限制

- 认证用户: 1000 req/hour
- 未认证用户: 100 req/hour

## 版本控制

API 使用语义化版本控制。主版本号变更会在 API URL 中体现（如 `/v2/`）。
