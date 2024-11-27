
## 简介

诺基亚贝尔 AX3000 (Nokia EA0326GMP) 刷最新版 OpenWrt | OpenClash和DDNS-GO可用

1. **登录路由器管理后台**：`192.168.10.1`

2. **破解 SSH**：
   - 找到系统管理 -> 备份和恢复 -> 选择文件
   - 将“破解 SSH/EA0326GMP_SSH.tar.gz”文件恢复备份，完成后自动重启

3. **重启之后用 SSH 工具登录**：
   - 建议用 Mobaxterm，方便上传文件
   - **Host**：`192.168.10.1`
   - **账号**：`root`
   - **密码**：第一次登录密码为空，（另外管理后台密码和 WiFi 密码变为 `password`）

4. **刷入 U-Boot**：
   - **指令 1**：
     - 查看 mtd 分区情况，可以用命令备份每个分区(例如：dd if=/dev/mtd1 of=/tmp/firmware_backup.bin)
     ```bash
     cat /proc/mtd
     ```
   - **指令 2**：
     - 将 `刷入 U-Boot 分区/mt7981_nokia_ea0326gmp-fip-fixed-parts.bin` 上传或拖入到 `/tmp` 目录
     ```bash
     mtd write /tmp/mt7981_nokia_ea0326gmp-fip-fixed-parts.bin FIP
     ```
     - 一定要注意 FIP 大小写

   - 没有报错就是成功了，如果报错说分区锁定，解决方法如下：
     - `kmod-mtd-rw` 这个软件通常是内核编译的时候带的，不要试图去安装，这是个坑，有能力就去编译源码生成固件，可以把这个模块编译进去，或者 TTL

   - 有这个模块之后执行指令解锁分区，解锁完继续写入 U-Boot 镜像，也可以写入别的 U-Boot 镜像
     ```bash
     insmod mtd-rw i_want_a_brick=1
     ```

5. **刷入 OpenWrt 镜像**：
   - 将 Internet 里面改成静态 IP 地址，`192.168.1.5`，掩码 `255.255.255.0`，网关 `192.168.1.1`，DNS：`192.168.1.1`
   - 然后网页打开 `192.168.1.1`，点击选择按钮选镜像，将“基于 LEDE 编译镜像/openwrt-mediatek-filogic-nokia_ea0326gmp-squashfs-sysupgrade.bin”上传点击 upload，等待上传完成，点击 update，更新完成后设备自动重启

6. **重启之后将 Internet 设置恢复成 DHCP**，然后打开 `192.168.1.1`，用户名 `root`，密码 `password`
   - 登录进去后建议先修改网络接口 IP 地址，将 `192.168.1.1` 改成 `192.168.5.1` 避免和光猫等设备冲突
   - 改完之后连上光猫网线就可以联网了

7. **编译集成 OpenClash 组件可以直接用**

## 附录

固件基于 LEDE 源码编译：[https://github.com/coolsnowwolf/lede](https://github.com/coolsnowwolf/lede)
内核版本：6.6.63
源版本：24.10
