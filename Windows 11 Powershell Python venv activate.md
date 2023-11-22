---
title: Windows 11 Powershell Python venv activate
date: 2023-03-03 22:01:50
tags:
- 技术
- Python
- Powershell
- Windows
- venv
categories:
- 技术
---

# 解决powershell无法运行Python venv中activate.ps1脚本的问题

在配置Python虚拟环境的时候，无法运行`activate.ps1`。

其中一个简单的解决方案是，将终端改为`cmd.exe`，运行`activate.bat`，避免了Powershell的一堆破事。

由于powershell的安全策略，将未知的ps1视为了不安全脚本，不允许执行。只需要放开权限就行。

通过管理员权限运行powershell，然后输入命令` set-ExecutionPolicy RemoteSigned`。

由于可能存在安全性问题，结束后可以用`set-ExecutionPolicy Default`恢复。

这个问题还在安装 scoop 的时候见过，官方也给出了解决方案——就不多说了。
