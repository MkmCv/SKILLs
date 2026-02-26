# arcs-dev-tools 使用示例（可复制粘贴）

这里存放 `arcs-dev-tools` 的“触发语句/使用方式”示例，方便快速验证工具链能力。

> 约定：**使用示例不放在 skill 包内**（不放进 `skills/arcs-dev-tools/` 目录），统一维护在 `docs/` 中，避免打包分发时夹带大量文档内容。

## 安装（npx skills）

本 skill 可通过 `vercel-labs/skills` 的 CLI 安装。

### 从 main 分支安装

```bash
npx skills add https://github.com/MkmCv/SKILLs --skill arcs-dev-tools -a cursor --copy -y
```

### 从指定分支安装（试用未合并更新）

```bash
npx skills add https://github.com/MkmCv/SKILLs/tree/feat/arcs-dev-tools-npx-skills-install/skills/arcs-dev-tools -a cursor --copy -y
```

## 示例 1：运行某个 sample（自动查找 + 编译烧录 + 成功/失败判定）

```
帮我运行 lisa_wdt basic：你自己去仓库里找到它的路径，阅读 README，编译、烧录到板子上并读取串口日志，最后告诉我成功还是失败（如果失败，把关键错误日志贴出来）。
```

## 示例 2：运行 helloworld（快速验证板子能跑通）

```
帮我运行 helloworld，并根据串口输出判断是否启动成功。
```

## 示例 3：批量验证多个模块（逐个串行）

```
批量验证 lisa_gpio lisa_uart lisa_pwm：逐个编译→烧录→读日志→出每个模块的结论，最后给一份汇总。
```

## 示例 4：首次环境拉起（拉取仓库 + 安装工具链 + 跑一个 sample）

```
我的机器上还没有 arcs-sdk，请先拉取仓库并初始化 submodule，然后安装开发环境（缺什么装什么），最后跑通 lisa_wdt basic。
```

## 示例 5：UI 显示一行字（写代码 → 烧录运行）

```
请你在 arcs-sdk 里写一个最小示例：UI 上显示一行文字 "Hello"。你自己选一个最合适的 UI 框架/示例工程作为起点（优先参考仓库已有 UI 示例），然后编译、烧录并用日志证明程序运行起来了。
```

## 示例 6：UI 文本迭代（改字 → 重新烧录 → 确认生效）

```
把上一个 UI 示例里的文字从 "Hello" 改成 "Hello ARCS"，重新编译烧录并确认修改已生效（说明你用什么方式确认：屏幕观察/日志等）。
```

## 反馈 bug（建议格式）

```text
【环境】OS/发行版：；Python：；板型：；串口设备：；仓库版本（commit/tag）：；
【触发语句】（你对 skill 说的话）
【期望结果】…
【实际结果】…
【命令输出/日志】（贴关键片段，必要时附完整）
```

