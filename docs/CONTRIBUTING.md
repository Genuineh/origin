# 贡献指南

感谢你对 Origin 项目的关注！

## 如何贡献

### 报告问题

在创建 Issue 之前，请先搜索是否已有类似问题。

创建 Issue 时请包含：
- 清晰的标题
- 复现步骤
- 期望行为 vs 实际行为
- 环境信息（操作系统、版本号等）
- 相关日志或截图

### 提交代码

1. Fork 项目
2. 创建特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 创建 Pull Request

### 代码规范

#### Rust 代码
- 使用 `cargo fmt` 格式化代码
- 使用 `cargo clippy` 检查代码质量
- 添加适当的文档注释
- 编写单元测试

#### TypeScript 代码
- 使用 `npm run lint` 检查代码
- 使用 `npm run format` 格式化代码
- 遵循 ESLint 规则
- 编写类型定义

#### Commit 信息
遵循约定式提交规范：

```
feat: 添加新功能
fix: 修复 Bug
docs: 文档更新
style: 代码格式调整
refactor: 重构代码
test: 测试相关
chore: 构建/工具相关
```

## 开发指南

### 设置开发环境

详见 [技术栈](docs/TECH_STACK.md)。

### 项目结构

```
origin/
├── backend/          # Rust 后端
├── frontend/         # React 前端
├── docs/            # 项目文档
└── templates/       # 代码生成模板
```

### 获取帮助

- 查看 [TODO](docs/TODO.md) 了解当前任务
- 在 Issues 中标记 `good first issue` 的任务适合新手
- 在 Discord/讨论区提问

## 许可

提交代码即表示你同意将代码以 MIT 许可证发布。
