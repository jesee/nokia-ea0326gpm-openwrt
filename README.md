# ea0325gpm-openwrt
简介
诺基亚贝尔ax3000刷最新版openwrt | 基于lede源码手动编译 | 纯净版集成科学

1、登录路由器管理后台：192.168.10.1
2、破解ssh：
找到系统管理->备份和恢复->选择文件
将“破解ssh/EA0326GMP_SSH.tar.gz”文件恢复备份，完成后自动重启
3、重启之后用ssh工具登录，建议用mobaxterm，方便上传文件
host：
    192.168.10.1
账号：
    root
密码：
    第一次登录密码为空，（另外管理后台密码和wifi密码变为password）

4、刷入uboot
   指令1
   cat /proc/mtd  #查看mtd分区情况，可以用命令备份每个分区(例如：dd if=/dev/mtd1 of=/tmp/firmware_backup.bin)
   指令2  #将 刷入uboot分区/mt7981_nokia_ea0326gmp-fip-fixed-parts.bin上传或拖入到/tmp目录
   mtd write /tmp/mt7981_nokia_ea0326gmp-fip-fixed-parts.bin FIP
   一定要注意FIP大小写

   没有报错就是成功了，如果报错说分区锁定，解决方法如下
   kmod-mtd-rw这个软件通常是内核编译的时候带的，不要试图去安装，这是个坑，有能力就去编译源码生成固件，可以把这个模块编译进去，或者ttl

   有这个模块之后执行指令解锁分区，解锁完继续写入uboot镜像，也可以写入别的uboot镜像
   insmod mtd-rw i_want_a_brick=1
5、刷入openwrt镜像
   将internet里面改成静态ip地址，192.168.1.5，掩码255.255.255.0，网关192.168.1.1，dns：192.168.1.1
   然后网页打开192.168.1.1，点击选择按钮选镜像，将“基于lede编译镜像/openwrt-mediatek-filogic-nokia_ea0326gmp-squashfs-sysupgrade.bin”
   上传点击upload，等待上传完成，点击update，更新完成后设备自动重启
6、重启之后将internet设置恢复成dhcp，然后打开192.168.1.1，用户名root，密码password
   登录进去后建议先修改网络接口ip地址，将192.168.1.1改成192.168.5.1避免和光猫等设备冲突
   改完之后连上光猫网线就可以联网了
7、继承了openClash组件可以直接用

附录：
    固件基于lede源码编译：https://github.com/coolsnowwolf/lede
    
   
   
   
