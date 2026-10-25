# Round Estimate and Native Toast Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 排除大厅与开局倒计时，修复离线防抖，并去掉通知的 PowerShell 子进程。

**Architecture:** Monitor 内维护逐服估算状态，现有设置对话框配置人数门槛及秒数；后台通知线程直接调用 WinRT，保留协议按钮。

**Tech Stack:** Python、PySide6、PyWinRT 3.2.1、PyInstaller。

---

### Task 1: 计时状态与回归

Files: monitor.py、config.py、tests/verify_round_timer.py、tests/verify_logic.py。

1. 添加可控时钟测试，先复现大厅计时与失败次数未清零。
2. 实现等待、倒计时、进行中；成功时清零失败次数。
3. 更新显示文案和旧计时兼容用例，运行计时与纯逻辑测试。

### Task 2: 设置与显示

Files: alert_dialog.py、sl_restart.py、tools/gui_check.py。

1. 添加人数和秒数设置，确认变更后重置估算状态，取消保持原值。
2. 显示等待、倒计时及估算标识，保留颜色和逐服显隐。
3. 运行离屏界面和拖动回归。

### Task 3: 进程内通知

Files: alerts.py、requirements.txt、tests/verify_alerts.py。

1. 安装固定版本 WinRT 依赖，读取本机 API 签名。
2. 替换子进程发送器，测试 apartment 清理、系统禁用和 XML/按钮。
3. 确认通知路径不存在脚本及子进程调用，实测本机通知。

### Task 4: 文档与发布

Files: README.md、build.bat（若打包依赖需要）。

1. 记录估算限制与配置方法，删去已失效的 PowerShell 说明。
2. 运行全部现有自检，打包并验证原生通知依赖。
3. 提交、推送并创建后续 PR；交付明确的新 EXE 路径。
