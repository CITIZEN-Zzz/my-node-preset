# Contributing to my-node-preset

感谢你对 my-node-preset 的关注！以下是参与贡献的指南。

## 提交 Issue

在提交 Issue 之前，请先检查是否已有相同或类似的 Issue。

### Bug 报告

请包含以下信息：

- 操作系统版本
- nvm-windows 版本（`nvm version`）
- Node.js 版本（`node -v`）
- 使用的 AI Agent 平台及版本
- 复现步骤
- 预期行为 vs 实际行为

### 功能建议

请说明：

- 你想解决的问题场景
- 建议的解决方式
- 是否愿意提交 PR 来实现

## 提交 Pull Request

### SKILL.md 修改规范

`SKILL.md` 是这个仓库的核心文件，修改时请注意：

1. **不要随意修改 front matter**（`---` 之间的元数据），版本号变更需在 PR 描述中说明理由
2. **新增意图触发词**应添加到 "Use this skill for" 章节
3. **行为变更**必须同步更新对应的规则章节
4. **保持输出风格**与现有示例一致：简洁、操作性强、明确

### 文件同步规则

- 修改 `SKILL.md` 的行为契约时，需同步更新 `README.md` 中的相关描述
- 任何功能变更都需要在 `CHANGELOG.md` 中记录
- 版本号遵循 [Semantic Versioning](https://semver.org/)

### PR 流程

1. Fork 本仓库
2. 基于 `main` 分支创建特性分支：`feat/your-feature` 或 `fix/your-fix`
3. 提交修改，commit message 参考：
   - `feat: 新增 xxx 功能`
   - `fix: 修复 xxx 问题`
   - `docs: 更新 xxx 文档`
   - `chore: 维护性变更`
4. 提交 PR，描述清楚变更内容和动机
5. 等待 review

## 开发理念

这个项目遵循"最小可行 + 逐步演进"的设计哲学：

- v1 是纯声明式的，不包含辅助脚本
- 优先验证交互模型的正确性
- 保持 Skill 定义的清晰和可预测性
- 不追求大而全，先把核心链路做扎实

如果你的 PR 引入了较大的架构变化，请先开 Issue 讨论。

## 许可证

提交贡献即表示你同意以 [MIT 许可证](LICENSE) 发布你的贡献代码。
