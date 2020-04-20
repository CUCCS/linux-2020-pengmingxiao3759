＃ Systemd开机自启动项管理
零。实验背景：

init缺点：启动时间长，启动脚本复杂。

系统优点：Systemd使用配置文件的方式更加方便的对自启动项进行管理，而原来的脚本（init.d）方式既不安全也不便于用户进行配置。
一。实验环境：

    Ubuntu的18.04服务器版 -配置文件Systemd -辅助记录器 -bandicam

二。实验步骤：
A.命令篇：

** 由于重启系统等操作无法通过asciinema录像上传，所以采用了Bandciam录制视频上传到微博上

** (https://weibo.com/7093262981/profile?rightmod=1&wvr=6&mod=personnumber&is_all=1)

    Unit

    Unit 的配置文件

    Target

    日志管理 没有采用分段录频

** https://asciinema.org/a/321936

asciicast
三。自查清单：

    ** 1。如何添加一个用户并转换为sudo执行程序的权限？**

A：使用命令来添加一个拥有sudo执行权限的用户：

`` sudo adduser用户名 sudo usermod -aG sudo 测试 ＃删除用户： userdel -r


！[ 1Users ]（img / 1Users.PNG）

-  ** 2。如何将一个用户添加到一个用户组？**

A：使用命令来添加到某一个用户组：

``
sudo usermod -aG组用户名

    ** 3。如何查看当前系统的分区表和文件系统详细信息？**

一种：

查看当前系统的分区表：

`` 须藤fdisk -l

查看文件系统信息：

``
 df -a [文件]

    ** 4。如何实现开机自动挂载Virtualbox的共享目录分区？**

一种：

安装增强功能，在VB界面不选择自动挂载；

在Windows中新建文件夹/ share；

在linux中新建文件夹：

`` mkdir / mnt /共享


挂载共享文件夹：

``
挂载-t vboxsf共享/ mnt / shared

** 5。基于LVM（逻辑分卷管理）的分区如何实现动态扩容和插入容量？**

一种：

查看系统中的逻辑卷相关信息/扫描逻辑卷：

`` lvdisplay / lvscan


！[ lvm ]（img / lvm.PNG）

动态扩容：

``
 lvextend -L <逻辑卷增量>  <逻辑卷全路径>

动态减容：

`` lvreduce -L <逻辑卷减量> <逻辑卷全路径>


-  ** 6。如何通过systemd设置实现在网络互连时运行一个指定脚本，在网络切换时运行另一个脚本？**

答：更改/etc/systemd/system/network-online.target.wants/networking.service配置文件下的[Service]部分属性：

【参考：<https://github.com/CUCCS/linux-2019-zzskin/blob/lab3/lab3/Sytemd%E5%85%A5%E9%97%A8%E5%AE%9E%E9%AA% 8C.md>】

``
＃在service段添加ExecStartPost，ExecStopPost
ExecStartPost = / bin / sh -c “回显”
ExecStopPost = / bin / sh -c “回声”

＃重载
systemctl守护程序重新加载

    ** 7。如何通过systemd设置实现一个脚本在任何情况下被杀死之后会立即重新启动？实现杀不死？**

A：重新启动定义在某处服务退出后重新启动的方式，如果将始终设置为总是，则总是重新启动。
四。参考资料：

http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html

http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html
