---
name: dev-habits
description: 开发习惯助手 - 版本管理和冲突解决。当使用 openSpec 或 oh my openCode 完成每个 change/case 后自动触发，负责生成修改内容、提交版本管理（SVN 提交或 Git 提交并推送）、处理代码冲突合并。
allowedTools:
  - Bash
  - Edit
  - Write
  - Read
  - Glob
  - Grep
auto-allow-cmd: true
---

# Dev Habits - 开发习惯助手

这个技能帮助你在开发过程中保持良好的版本管理习惯。

## 触发时机

**必须在使用此技能 whenever:**
- 使用 openSpec 完成一个 change 或 case 后
- 使用 oh my openCode 完成一个 change 或 case 后
- 需要提交代码变更到版本控制系统时
- 遇到代码冲突需要合并时

## 核心功能

### 1. 版本管理提交

每当完成一个 change/case 后，执行以下步骤：

#### 识别版本控制系统
1. 检查项目使用 SVN 还是 Git（查找 `.git` 或 `.svn` 目录）
2. 根据检测到的系统执行相应操作

#### Git 工作流
```bash
# 1. 查看变更状态
git status

# 2. 添加变更的文件（使用具体文件路径，避免 git add -A）
git add <files>

# 3. 生成有意义的提交信息
git commit -m "$(cat <<'EOF'
<提交信息>

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
EOF
)"

# 4. 推送到远程
git push
```

#### SVN 工作流
```bash
# 1. 查看变更状态
svn status

# 2. 添加新文件（如有）
svn add <new-files>

# 3. 提交变更
svn commit -m "<提交信息>"
```

### 2. 冲突解决策略

当遇到合并冲突时，按以下原则处理：

| 代码来源 | 处理策略 |
|---------|---------|
| **你实现的代码** | 你负责全部合并工作，确保逻辑正确 |
| **他人提交的代码** | 快速合并能确定的部分，不确定部分标记出来由用户手动处理 |
| **非你实现的代码** | 同上，保守处理，优先保证代码安全 |

#### 冲突处理流程
1. 运行 `git status` 识别冲突文件
2. 逐个读取冲突文件，分析冲突内容
3. 根据上述策略决定合并方式
4. 编辑文件解决冲突
5. 标记冲突已解决：`git add <resolved-file>`
6. 完成合并提交

**重要原则：**
- 不要使用 `--no-verify` 跳过 hooks
- 不要强制推送（除非用户明确要求）
- 不要重置或丢弃变更（除非用户明确要求）
- 遇到不确定的情况先询问用户

## 提交信息规范

遵循以下格式生成提交信息：

```
<type>(<scope>): <subject>

<body>

Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
```

常用 type：
- `feat`: 新功能
- `fix`: 修复 bug
- `docs`: 文档变更
- `style`: 代码格式（不影响功能）
- `refactor`: 重构
- `test`: 测试相关
- `chore`: 构建/工具/配置

## 示例

### 示例 1：完成功能后提交
用户："这个用户登录功能写完了"

你应该：
1. 运行 `git status` 查看变更
2. 读取修改的文件确认变更内容
3. 生成提交：`feat(auth): 实现用户登录功能`
4. 提交并推送

### 示例 2：遇到冲突
用户："合并时好像有冲突"

你应该：
1. 运行 `git status` 确认冲突文件
2. 读取冲突文件，分析冲突区域
3. 根据代码来源决定合并策略
4. 解决冲突并完成提交

---

## 注意事项

1. **不要创建空提交** - 如果没有变更，不要提交
2. **不要过度提交** - 确保每个提交有实际意义
3. **保护敏感文件** - 提交前检查是否包含 .env、credentials 等敏感文件
4. **保持原子提交** - 每个提交应该是一个完整的变更单元
