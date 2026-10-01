# GPDMicroPC2BootRotate
# 在安装了limine启动器的GPD Micro PC 2上旋转屏幕方向
# 只适用于Limine启动器

环境：CachyOS+Limine+原生竖屏的显示器\n
增加内核参数，使竖屏显示的界面改为横屏，方便操作\n
在limine.conf中增加如下内容：\n
1：interface_rotation:90\n
2：在cmdline中增加 fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up\n
--定义：${addedconf}=fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up\n
--使用：${addedconf}\n

# 第二种方法：
参考https://wiki.archlinux.org.cn/title/Limine\n
首先查看/proc/cmdline的内核参数，确认原始信息\n
在/etc/kernel/cmdline中增加上述横屏内容\n
