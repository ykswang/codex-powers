# Codex Usage Rules

## Required Parameters

### cwd (Mandatory)

- **Must be the actual absolute path of current workspace** - never fabricate or guess
- **Must be an existing directory**

❌ Prohibited: `/path/to/project`, `./src`, omitting cwd

### sandbox (Mandatory)

| User Intent | sandbox Value |
|-------------|---------------|
| Advice, review, analysis | `read-only` |
| Modify/create files | `danger-full-access` |

❌ Prohibited: omitting sandbox, using wrong mode for task type

## File Paths in Prompt

- Every file must have a **clear, unique path** relative to cwd
- **No ambiguity** - must match exactly one file
- For multiple files, **list each explicitly**

❌ Prohibited: `"modify index.ts"`, `"fix the config file"`, `"update all test files"`

✅ Correct: `"modify src/components/UserList.tsx"`, `"fix src/config/app.config.ts"`

## Spec Workflow Integration

### Design Review with Codex

在 Kiro Spec 流程中，**Design 阶段完成后**，建议使用 Codex 进行 review：

1. Design 文档完成后，调用 Codex 进行设计评审
2. 使用 `read-only` sandbox 模式
3. 让 Codex 检查设计的合理性、潜在问题、改进建议

**示例：**
```json
{
  "prompt": "Review the design document at .kiro/specs/2025-01-01-feature-name/design.md, check for potential issues, suggest improvements",
  "sandbox": "read-only",
  "cwd": "/absolute/path/to/workspace"
}
```

**Review 要点：**
- 架构设计是否合理
- 是否有遗漏的边界情况
- 性能和安全性考虑
- 与现有代码的兼容性
