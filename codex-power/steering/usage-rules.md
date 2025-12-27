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
