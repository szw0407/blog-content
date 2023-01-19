# 解决powershell无法运行ps1脚本的问题

在配置Python虚拟环境的时候，无法运行activate.ps1。

其中一个简单的解决方案是，将终端改为cmd.exe，运行activate.bat，避免了Powershell的一堆破事。

至于如何调教powershell：

参考：https://zhuanlan.zhihu.com/p/493496089

power shell的安全策略，将 nrm 命令视为了不安全脚本，不允许执行。只需要放开权限就行。

通过管理员权限运行powershell，然后输入命令` set-ExecutionPolicy RemoteSigned`

由于可能存在安全性问题，需要用`set-ExecutionPolicy Default`恢复。
