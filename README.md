# SKILLs（我的技能包合集）

这里整理我自用的一组 **Skill 技能包**（以目录形式保存 `SKILL.md`、模板、示例等），用于在支持“技能/工作流”的 AI 编程工具里复用固定流程与产出格式。

## 包含的技能

### 1) `skills/arcs-dev-tools/`

- **名称**：`arcs-dev-tools`
- **用途**：ARCS SDK 工具链层能力：**拉取仓库、安装开发环境、编译、烧录、运行、日志读取**（代码开发/理解交由模型本体）
- **入口文件**：`skills/arcs-dev-tools/SKILL.md`
- **分发包**：`arcs-dev-tools.zip`
- **状态**：开发中，欢迎反馈使用过程中的 bug / 兼容性问题，并欢迎补充更好的用法示例
- **使用示例**：见 `docs/arcs-dev-tools-usage.md`

## 使用方式（通用）

- **作为源码使用**：直接在工具中引用/加载对应目录下的 `SKILL.md`（保持目录结构不变）。
- **作为压缩包分发**：使用仓库根目录的 `*.zip`（同样需要保持目录结构不变）。

## 用 `npx skills` 安装（推荐）

本仓库已按 `skills/<skill-name>/SKILL.md` 的结构组织，可直接用 `vercel-labs/skills` 的 CLI 安装。

### 1) 查看仓库里有哪些 skill

```bash
npx skills add https://github.com/MkmCv/SKILLs --list
```

### 2) 安装 `arcs-dev-tools`（项目级，安装到 Cursor）

> Windows 环境如遇到 symlink 权限/策略问题，建议加 `--copy`。

```bash
npx skills add https://github.com/MkmCv/SKILLs --skill arcs-dev-tools -a cursor --copy -y
```

### 3) 直接从某个分支安装（用于试用未合并的更新）

把 URL 指向 `tree/<branch>/...` 即可（示例分支名见下方命令）：

```bash
npx skills add https://github.com/MkmCv/SKILLs/tree/feat/arcs-dev-tools-npx-skills-install/skills/arcs-dev-tools -a cursor --copy -y
```

## 约定

- 每个技能以“同名目录”承载，目录内必须有 `SKILL.md`。
- 模板、示例、资源文件放在技能目录内部（例如 `templates/`、`examples/`），避免跨目录依赖，方便单独打包分发。

