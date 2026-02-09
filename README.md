# Origin

> AI-First Design-to-Code Platform

Origin 是一个类似 Penpot 的设计工具，但核心理念完全不同——**为 AI 辅助的人机协作而生**。

## 核心理念

```
人类 → AI → UI设计稿 + UX设计 + 前端建模 + 前端系统外部能力 → DSL → 生成器 → Code
```

## 主要特性

- **AI-First 设计**: 自然语言交互生成界面
- **统一前端框架**: 优先支持 Flutter，一次设计多平台运行
- **OpenAPI 对接**: 导入 API 规范，自动生成调用代码
- **可视化建模**: 状态、行为、数据流可视化建模
- **设计到代码**: 从设计稿直接生成可运行应用

## 项目状态

**开发阶段**: 项目规划中

## 文档

- [项目概述](docs/PROJECT_OVERVIEW.md) - 项目愿景和核心特性
- [系统架构](docs/ARCHITECTURE.md) - 技术架构和模块设计
- [工作流程](docs/WORKFLOW.md) - 使用流程和核心概念
- [技术栈](docs/TECH_STACK.md) - 技术选型详解
- [任务清单](docs/TODO.md) - 开发任务和里程碑

## 快速开始

### 环境要求

- Rust 1.75+
- Node.js 20+
- PostgreSQL 15+
- Redis 7+

### 安装

```bash
# 克隆项目
git clone https://github.com/yourusername/origin.git
cd origin

# 启动开发环境
docker-compose up -d

# 启动后端
cd backend && cargo run

# 启动前端
cd frontend && npm install && npm run dev
```

## 贡献

欢迎贡献！请查看 [CONTRIBUTING.md](docs/CONTRIBUTING.md) 了解详情。

## 许可证

MIT License - 详见 [LICENSE](LICENSE)

---

**Origin** - 让设计与代码无缝融合
