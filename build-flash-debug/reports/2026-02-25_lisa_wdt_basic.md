# 执行报告：lisa_wdt basic

**日期**: 2026-02-25
**操作者环境**: /dev/ttyACM0, BOARD=arcs_evb
**最终状态**: 成功

## 执行摘要

编译、烧录 lisa_wdt basic 示例并通过串口验证运行正常。WDT 看门狗基础功能（配置、启动、喂狗）全部符合预期。过程中遇到串口相关的环境问题，均已解决。

## 遇到的问题

### [串口通信] picocom 在非 TTY 环境不可用
- **现象**: picocom 报 `STDIN is not a TTY`，立即退出
- **是否偶发**: 否（必现，因 Claude Code Bash 无 TTY）
- **根因**: picocom 是交互式终端程序，需要 TTY 环境
- **解决方案**: 改用 pyserial (`import serial`) 读取串口
- **耗时**: 1 轮

### [串口通信] 首次读取乱码
- **现象**: `stty` + `cat` 方式读取时全部乱码
- **是否偶发**: 否（stty 方式必现乱码）
- **根因**: `stty` 方式缺少 DTR/RTS 控制，串口状态不正确
- **解决方案**: 改用 pyserial 并控制 DTR/RTS 信号
- **耗时**: 1 轮

### [串口通信] DTR/RTS 翻转导致双重启动
- **现象**: 板子启动日志出现两次
- **是否偶发**: 否（两次验证一致复现，但不是代码问题）
- **根因**: DTR/RTS 信号序列产生两个复位边沿，属硬件信号层面的正常行为
- **解决方案**: 以第二次启动后的稳定性为判定依据。验证第二次启动后 13+ 秒无重启
- **耗时**: 2 轮验证

### [串口通信] USB CDC 断连
- **现象**: 串口读取时报 `SerialException: device disconnected`
- **是否偶发**: 是（仅在特定 DTR/RTS 翻转时序下触发）
- **根因**: 过激的 DTR/RTS 翻转导致 USB 设备重新枚举
- **解决方案**: catch 异常 → 等待 2-3 秒 → 重新扫描设备 → 重连
- **耗时**: 1 轮

### [烧录] cskburn ETIMEDOUT / EBUSY
- **现象**: 重新烧录时报 ETIMEDOUT，随后报 EBUSY
- **是否偶发**: 否（前一个 serial 进程未释放时必现）
- **根因**: 之前的 python serial 进程仍占用串口
- **解决方案**: `fuser /dev/ttyACM0` 找到 PID → `kill PID` → 重试
- **耗时**: 2 轮

## 关键数据

- 编译时间: ~30 秒（Clean + Release 两次）
- 编译产物大小: 134,016 bytes (arcs.bin)
- 烧录用时: ~5 秒
- 串口验证时长: 15 秒 × 2 轮

## 给后续使用者的建议

1. **不要用 picocom**：在 Claude Code 环境下用 pyserial 替代，picocom 需要 TTY
2. **串口读取前先清缓冲区**：`ser.reset_input_buffer()` 避免读到残留数据
3. **烧录前确认串口空闲**：先 `fuser` 检查，否则 cskburn 会报 EBUSY
4. **双重启动不是 bug**：DTR/RTS 复位序列会产生两次启动，以第二次后的稳定性为准
5. **喂狗循环无可见输出**：feed 函数只有 LOGD 级别日志，持续运行时串口看不到任何输出是正常的
