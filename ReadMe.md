# KaliLinux  学习笔记

## 1.安装
    kali2019.4版本是2019年度最后一个发行版本，采用了xface为默认的系统界面。
跟以往的版本一样安装，但安装完成后会出现几个问题：

1. 如果安装中文模式，安装完成后，一般都会出现中文乱码的情况。
2. 安装vmwaretools后，依然无法实现 1920x1080的分辨率。
3. 安装google拼音时，出现无法应用的情况。

###对应的解决方法：
###1.中文乱码解决
####0x01 更新源
`mousepad /etc/apt/sources.list`

在文档中写入国内卡里源：

\#清华大学
 
`deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free`
 
`deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free`

\#阿里云
 
`deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib`
 
`deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib`

\#浙大
 
`deb http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free`
 
`deb-src http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free`

\#中科大
 
`deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib`
 
`deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib`

保存后，在命令行输入：

`apt-get clean && apt-get update -y`

####0x02 安装字体
`sudo apt-get install ttf-wqy-microhei ttf-wqy-zenhei xfonts-intl-chinese`

####0x03 更改默认编码
在终端命令行输入 `dpkg-reconfigure locales`

进入图形化界面后，选中`len_US.UTF-8 UTF-81`和`zh_CN.UTF-8 UTF-8`(空格是选择，Tab是切换，*是选中)

确定后，将zh_CN.UTF-8 UTF-8选为默认(前面的按钮是[确定]键)。

####0x04 重启
输入`reboot`命令，重启kali后，一切显示正常。

###2. 安装vmwaretools后，无法显示1920x1080分辨率
####0x01 查询某个分辨率的有效扫描频率
`cvt  1920  1080`

####0x02 查询显示器名称
`xrandr`

####0x03 通过--newmode参数新建一种xrandr模式
`xrandr  --newmode  "1920x1080_60.00"   173.00  1920 2048 2248 2576  1080 1083 1088 1120  -hsync  +vsync`

####0x04 把新建的模式添加到当前的输出设备
`xrandr --addmode  Virtual1   1920x1080_60.00`

####0x05 将屏幕的分辨率更改为我们刚刚添加的分辨率
`xrandr  --output  VGA-1 --mode  1920x1080_60.00`

效果应该是马上生效的，如果不行可以点：设置 --> 显示  -->分辨率，可以看到多了一项：1920 x 1080 ，选中后应用即可。

####0x06 最后一步，添加到profile文件

`mousepad /etc/profile`

把下面两行命令添加到 /etc/profile 文件结尾，保存重启即可！
`xrandr  --newmode  "1920x1080_60.00"   173.00  1920 2048 2248 2576  1080 1083 1088 1120  -hsync  +vsync`

`xrandr --addmode  Virtual1   1920x1080_60.00`

`reboot`

重启后，如果分辨率依然无法到1920x1080，在终端命令行输入：

`source /etc/profile`

在点 设置 --> 显示  -->分辨率，可以看到多了一项：1920 x 1080 ，选中后应用即可。

###3. 安装拼音输入法
`apt-get install ibus ibus-pinyin`

`reboot`

重启系统后，通过 <b>WIN+space</b> 组合键，切换中英文。

至此，kali2019.4成功安装到VMware上，可以开始进行渗透测试！
