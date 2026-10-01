# 一、GPDMicroPC2BootRotate
## 在安装了limine启动器的GPD Micro PC 2上旋转屏幕方向<br>
只适用于Limine启动器<br>

环境：CachyOS+Limine+原生竖屏的显示器<br>
增加内核参数，使竖屏显示的界面改为横屏，方便操作<br>
## 第一种方法，这种更新后会掉设置：
在limine.conf中增加如下内容：<br>
1.在文件开始部分加入：interface_rotation:90<br>
2：在cmdline行中增加 fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up,可以用定义的方式：<br>
--定义，这个也放到开始位置：${addedconf}=fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up<br>
--使用，在对应的cmdline行后面加上：${addedconf}<br>

## 第二种方法：<br>
参考https://wiki.archlinux.org.cn/title/Limine 第6.1.1配置<br>
~~首先查看/proc/cmdline的内核参数，确认原始信息<br>
在/etc/kernel/cmdline中增加上述横屏内容：<br>
+=fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up~~<br>
在limine的cmdline中增加参数，增加内核参数的位置应该有两个：<br>
/etc/kernel/cmdline（这个刚开始可能没有）或者/etc/default/limine
所以可以把参数加到/etc/default/limine里面<br>
/proc/cmdline 为当前的mcdline,可以先查看参考<br>

注意： /etc/default/limine 具有最高优先级，并覆盖所有嵌入配置。因此，在附加内核参数时，建议使用 +=。示例：<br>

```python
KERNEL_CMDLINE[default]+=rw root=UUID=... 
KERNEL_CMDLINE[default]+=quiet splash initrd=/amd-ucode.img
```

举例：<br>
/etc/default/limine 里面显示的是：<br>
```python
ESP_PATH="/boot"
KERNEL_CMDLINE[default]+="quiet nowatchdog splash rw rootflags=subvol=/@ root=UUID=9ceb7b71-f0ad-410a-b0b5-035a859fcf91"
BOOT_ORDER="*, *lts, *fallback, Snapshots"
```
增加后的内容是:<br>
```python
ESP_PATH="/boot"
KERNEL_CMDLINE[default]+="quiet nowatchdog splash rw rootflags=subvol=/@ root=UUID=9ceb7b71-f0ad-410a-b0b5-035a859fcf91 fbcon=rotate:1 video=DSI-1:panel_orientation=right_side_up"
BOOT_ORDER="*, *lts, *fallback, Snapshots"
```



查找文件位置<br>
sudo find /boot -maxdepth 4 -type f -name limine.conf -print<br>



# 二、中文输入法安装<br>
```
sudo pacman -S fcitx5 fcitx5-chinese-addons fcitx5-configtool fcitx5-gtk fcitx5-qt
```
<br>
编辑：/etc/environment<br>
增加```XMODIFIERS=@im=fcitx``` for XWayland application<br>
其他的按照：https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland#KDE_Plasma的说法，不要动<br>
