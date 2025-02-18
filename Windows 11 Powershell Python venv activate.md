# 解决powershell无法运行Python venv中activate.ps1脚本的问题

在配置Python虚拟环境的时候，无法运行`activate.ps1`。

其中一个简单的解决方案是，将终端改为`cmd.exe`，运行`activate.bat`，避免了Powershell的一堆破事。

由于powershell的安全策略，将未知的ps1视为了不安全脚本，不允许执行。只需要放开权限就行。

在Windows 11中打开设置-系统-开发者选项-Powershell，启用“允许本地脚本执行”。

或者直接通过管理员权限运行powershell，然后输入命令`Set-ExecutionPolicy RemoteSigned`；

这个问题还在安装 scoop 的时候见过，官方也给出了解决方案——就不多说了。
