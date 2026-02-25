# SKILLs（我的技能包合集）

这里整理我自用的一组 **Skill 技能包**（以目录形式保存 `SKILL.md`、模板、示例等），用于在支持“技能/工作流”的 AI 编程工具里复用固定流程与产出格式。

## 包含的技能

### 1) `build-flash-debug/`

- **名称**：`build-flash-debug`
- **用途**：面向 ARCS SDK 示例程序的「编译 → 烧录 → 串口验证」全自动工作流（含失败时的自动修复与验证报告）
- **入口文件**：`build-flash-debug/SKILL.md`
- **分发包**：`build-flash-debug.zip`

### 2) `notebooklm-video-scripter/`

- **名称**：`notebooklm-video-scripter`
- **用途**：把一份 Markdown 文档转成可直接粘贴到 NotebookLM 的“视频讲解 Steering Prompt”，并可选生成素材/截图建议清单
- **入口文件**：`notebooklm-video-scripter/SKILL.md`
- **说明文档**：`notebooklm-video-scripter/README.md`
- **分发包**：`notebooklm-video-scripter.zip`

## 使用方式（通用）

- **作为源码使用**：直接在工具中引用/加载对应目录下的 `SKILL.md`（保持目录结构不变）。
- **作为压缩包分发**：使用仓库根目录的 `*.zip`（同样需要保持目录结构不变）。

## 约定

- 每个技能以“同名目录”承载，目录内必须有 `SKILL.md`。
- 模板、示例、资源文件放在技能目录内部（例如 `templates/`、`examples/`），避免跨目录依赖，方便单独打包分发。

