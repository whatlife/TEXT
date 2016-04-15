
# 安装

直接到官网下载，完整包包括git，vim等其他工具。我这里只下载了mini版本

# 设置
### 解决中文乱码问题

把一下几行代码添加到config/aliases文件末尾即可解决中文乱码问题：

> l=ls --show-control-chars 
la=ls -aF --show-control-chars 
ll=ls -alF --show-control-chars
ls=ls --show-control-chars -F

### 解决文字重叠问题

Win + Ait + P 唤出设置界面 > mian > font > monospce 的勾勾去掉(点两下).

![enter image description here](http://ww1.sinaimg.cn/mw690/602cf753gw1f2xc674af4j20b405n3zq.jpg)


### 配置其在win+r中打开

把根目录加到系统环境的path变量中即可。
![enter image description here](http://ww4.sinaimg.cn/large/602cf753gw1f2xc41mpbjj209q03r74l.jpg)

### 右键打开
在 Cmder 目录直接运行 

>cmder /register user 

或者 

>cmder /register all 




