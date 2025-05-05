---
title: 如何修复 Old登 弄坏的Wifi
description: 修完开冰点 (DeepFreeze) 谢谢
date: 2025-05-05
categories:
    - System
---

# 这只是个标题

### **一、检查组策略设置 (这个试过了估计没用)**

1. **打开组策略编辑器**：
   - 按 `Win + R` 输入 `gpedit.msc` 回车。
   - 导航至：
     `计算机配置 → 管理模板 → 网络 → 网络连接`
     检查右侧是否有 **“禁止访问LAN连接属性”** 或 **“禁用无线设备”** 的策略，将其设置为 **“未配置”**。
2. **设备安装限制**：
   - 进入路径：
     `计算机配置 → 管理模板 → 系统 → 设备安装 → 设备安装限制`
     确保没有启用 **“禁止安装可移动设备”** 或 **“阻止使用与下列设备安装程序类相匹配的驱动程序安装设备”**。

------

### **二、检查计划任务 (感觉这个最大可能)**

1. 按 `Win + R` 输入 `taskschd.msc` 打开任务计划程序。
2. 在 **任务计划程序库** 中，查找近期创建的任务，尤其是触发条件为“重复任务”或“系统启动”的任务。
3. 检查任务操作中是否有禁用无线网卡的命令（如 `devcon disable` 或 `netsh` 命令）。

------

### **三、检查服务状态 (这个我也试过了)**

1. 按 `Win + R` 输入 `services.msc`。
2. 找到 **WLAN AutoConfig** 服务，确保其状态为 **“正在运行”**，启动类型为 **“自动”**。

------

### **四、注册表检查 (谨慎操作 建议等我来)**

1. 按 `Win + R` 输入 `regedit`。
2. 导航至：
   `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NDIS`
   检查右侧是否有异常项，或 `Start` 值是否为 `3`（手动启动）。
3. **警告**：修改注册表前建议备份，误操作可能导致系统不稳定。

------

### **五、命令行尝试强制启用**

1. 以管理员身份打开命令提示符（CMD）或 PowerShell。

2. 输入以下命令查看无线网卡硬件ID：

   ```
   powershell "Get-NetAdapter | Where-Object {$_.PhysicalMediaType -eq 'Native 802.11'}"
   ```

3. 使用硬件ID启用设备（替换 `<ID>`）：

   ```
   devcon enable "<ID>"
   ```

   （若未安装 `devcon`，可从微软官网下载 [Windows Driver Kit](https://docs.microsoft.com/en-us/windows-hardware/drivers/download-the-wdk) 获取工具）