# notebooklm-video-scripter（Claude Skill）

本 Skill 用于把一份 Markdown 文档，转换成三类视频脚本产物：

- **A｜产品/商务 Feature 概览**：不讲实现，只讲变化与价值
- **B｜技术原理讲解**：动机→直觉→机制→术语映射
- **C｜版本发布视频**：新Feature + 问题修复 + 下一版本预告（用形象图示解释技术术语）

产出包含：
- `notebooklm_prompt.md`：**可直接粘贴给 NotebookLM** 的 Prompt（引导其基于 Sources 生成视频讲解）

可选（建议生成，但不强制）：
- `assets_checklist.md`：截图/图片/示意图建议清单（P0/P1/P2），由你和 AI 商榷要不要补

## 如何在 Claude 中使用（通用思路）

1. 将整个 `notebooklm-video-scripter/` 文件夹打包成 zip（保持目录结构不变）
2. 在 Claude 的 Skills 管理处上传/启用该 Skill（不同产品入口略有差异）
3. 在对话中提供 md 内容，并说明要 A、B 还是 C、目标受众与节奏

## 最小调用示例

### A：产品/商务（2 分钟）

你可以这样对 Claude 说：

> 下面是我的 md 文档，请用 **视频类型 A（产品/商务 Feature）** 做一个 **120 秒** 的新功能介绍视频。  
> 配图：不配图 / 只补P0（推荐） / 尽量多图（我稍后决定）。  
> 输出：`assets_checklist.md（建议） + notebooklm_prompt.md`。  
> 观众：产品经理 + 销售。  
> 注意：不要讲实现原理、不要出现代码/命令/参数。

### B：技术原理（3 分钟）

> 下面是我的 md 文档，请用 **视频类型 B（技术原理讲解）** 做一个 **180 秒** 的教学视频。  
> 配图：不确定，你先给 `assets_checklist.md` 我再决定补哪些/不补。  
> 输出：`assets_checklist.md（建议） + notebooklm_prompt.md`。  
> 观众：后端工程师。  
> 重点：先动机与直觉类比，再用流程图逐步展开机制，最后把类比映射回术语定义。

### C：版本发布视频（5-6 分钟紧凑）

> 下面是我的 CHANGELOG.md，请用 **视频类型 C（版本发布）** 做一个版本发布视频。  
> 节奏：紧凑（5-6 分钟）。  
> 配图：你先给 `assets_checklist.md`，告诉我哪些技术术语需要形象化图示（如 UVC → USB 摄像头图、MQTT → 消息传递图）。  
> 输出：`assets_checklist.md（建议） + notebooklm_prompt.md`。  
> 观众：开发者和技术用户。  
> 重点：结构固定为"新Feature（重要的展开讲，只用 CHANGELOG 原文） → 问题修复总结 → 下一版本预告"。

## 文件说明

- `SKILL.md`：Skill 元数据与工作流（Claude 触发与执行依据）
- `templates/`：A/B/C 三种模板与公共视觉规则（渐进披露，便于迭代）
  - `template_a_feature.md`：产品/商务 Feature 概览
  - `template_b_tech.md`：技术原理讲解
  - `template_c_release.md`：版本发布视频（新增）
  - `visual_rules.md`：通用视觉规则
- `examples/`：示例输入输出


