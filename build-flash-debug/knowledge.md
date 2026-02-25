# 经验知识库

> 由 build-flash-debug skill 自动维护。执行前读取用于快速诊断，执行后追加新发现。
> 按 编译 / 烧录 / 串口 / 代码调试 四个 topic 组织，快速定位对应阶段的问题。

---

## Topic 1: 编译

（暂无记录）

---

## Topic 2: 烧录

### 2-1. 串口设备号变化
- **现象**: /dev/ttyACM0 突然变成 /dev/ttyACM1 或其他
- **原因**: USB 重新枚举（拔插、DTR/RTS 翻转、烧录工具操作）
- **解决**: 每次操作前扫描 `ls /dev/ttyACM* /dev/ttyUSB*`，不硬编码
- **次数**: 0（预防性记录）

### 2-2. 串口被占用 (EBUSY)
- **现象**: cskburn 报 `Failed opening device: EBUSY`
- **原因**: python serial / picocom 进程未关闭
- **解决**: `fuser <设备>` → `kill <PID>` → 重试
- **次数**: 1（2026-02-25, lisa_wdt basic）

### 2-3. cskburn ETIMEDOUT
- **现象**: `Failed changing baud rate: ETIMEDOUT`，反复重试失败
- **原因**: 串口被占用或 USB 连接不稳定，设备无法进入更新模式
- **解决**: 先 `fuser` 释放占用 → 重新扫描设备号 → 重试
- **次数**: 1（2026-02-25, lisa_wdt basic）

---

## Topic 3: 串口

### 3-1. picocom 非 TTY 环境不可用
- **现象**: picocom 报 `STDIN is not a TTY`，立即退出
- **原因**: Claude Code Bash 工具无 TTY
- **解决**: 用 pyserial (`import serial`) 替代
- **次数**: 1（2026-02-25, lisa_wdt basic）

### 3-2. 串口乱码
- **现象**: 刚打开串口或复位瞬间读到乱码（如 `<���<`）
- **原因**: bootloader 使用不同波特率；串口状态未稳定
- **解决**: ① `ser.reset_input_buffer()` 清空 ② sleep 1-2 秒后重新打开 ③ 等板子完全启动后再读
- **次数**: 2（2026-02-25, lisa_wdt basic）

### 3-3. DTR/RTS 复位导致双重启动
- **现象**: DTR/RTS 翻转后启动日志出现两次，之后稳定运行
- **原因**: 信号序列 (False→True→False) 产生两个复位边沿，硬件正常行为
- **判定**: 非代码问题，以第二次启动后的稳定性为准
- **次数**: 2（2026-02-25, lisa_wdt basic）

### 3-4. USB CDC 设备断连
- **现象**: `SerialException: device disconnected`
- **原因**: DTR/RTS 翻转导致 USB 重新枚举
- **解决**: 关闭连接 → 等 2-3 秒 → 扫描 `/dev/ttyACM*` → 用新路径重连
- **次数**: 1（2026-02-25, lisa_wdt basic）

---

## Topic 4: 代码调试

### 4-1. 双重启动 ≠ WDT 超时
- **现象**: 启动日志出现两次，易误判为看门狗超时复位
- **区分**:
  - WDT 超时 → 持续重启（每隔固定时间循环）
  - DTR/RTS 信号 → 恰好两次，之后稳定
  - 不做 DTR/RTS 翻转时仍重启 → 才是 WDT 问题
- **验证**: 第二次启动后监控 10-15 秒，0 次额外重启 → 排除 WDT
- **次数**: 1（2026-02-25, lisa_wdt basic）
