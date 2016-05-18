[TOC]

# 在vs code里用cmder替换cmd
vs code在资源文件上右有一项是`Open in Command Prompt` 默认打开的是windows的cmd，由于vs code强大的自定义属性，使得其可以更改为[cmder](http://cmder.net/)打开

## 配置文件
菜单里打开Workspace settings配置选项，复制`externalTerminal.windowsExec` 到右侧settings.json的用户自定义文件里。这个文件可以覆盖系统的设置。
## 写到新的配置文件
设置`externalTerminal.windowsExec` 为cmder的路径如：
`D:\\cmder\\Cmder.exe`
OR

`D:/cmder/Cmder.exe`

## 测试
命令可以被执行，但只是打开cmder，而不能定位到当前文件所在的目录。。。估计是哪个环境变量整错了。
哦，是bug，这个issue有说明[#6466](https://github.com/Microsoft/vscode/issues/6466)

> Written with [StackEdit](https://stackedit.io/).