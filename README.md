参考：https://weibo.com/ttarticle/p/show?id=2309405328137356705848<br>
核心结论：这些画面由不同组件控制<br>

|显示阶段|控制者|本机解决方法|
|---|---|---|
|GPD BIOS Logo|UEFI 固件|操作系统无法控制|
|Limine 菜单|Limine|interface_rotation: 90|
|UKI Arch Linux Logo|systemd-stub / UKI splash|取消 --splash|
|启动文字与 LUKS 提示|Linux framebuffer console|fbcon=rotate:1|
|Plasma 登录界面|plasma-login-manager 的独立 KWin 会话|同步正确的 kwinoutputconfig.json|
|登录后的桌面|当前用户的 KWin 输出配置|在 Plasma 显示设置中调整|
|桌面自动旋转|mxc4005(这个是P4的硬件) + iio-sensor-proxy + KWin|启用 KWin autoRotatePolicy|



# 一、GPDMicroPC2BootRotate
## 在安装了limine启动器的GPD Micro PC 2上旋转屏幕方向<br>
只适用于Limine启动器<br>

环境：CachyOS+Limine+原生竖屏的显示器<br>
增加内核参数，使竖屏显示的界面改为横屏，方便操作<br>
## 第一种方法（这种更新后可能会掉设置）：
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

# 三、Dolphin网络里面看不到NAS文件夹<br>
kio_smb只列出匿名共享的文件夹，可以用这个验证：<br>
```
smbclient -L <nas_ip> -U username
```
如果能看到全部共享就说明是这个问题<br>
然后先安装kio-extras samba kdenetwork-filesharing 尝试一下<br>
```
sudo pacman -S kio-extras samba kdenetwork-filesharing
```
可能的解决方式：<br>
1.直接挂载到文件夹名<br>
2.装smb4k<br>
3.cifs挂载<br>


# 四、登录界面的旋转
先在设置-登录屏幕里面应用一下（可能就同步过去了），不行再尝试下面的方法<br>
~/.config/kwinoutputconfig.json 对应登录后的界面，里面有transform的字段设置旋转<br>
/var/lib/plasmalogin/.config/kwinoutputconfig.json    对应登录界面<br>





