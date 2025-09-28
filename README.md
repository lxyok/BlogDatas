## Blog Datas

# 我恢复甲骨文云的笔记  
### 显示灰色  无法在正常实例中挂载附加盘  可以到  甲骨文控制台 存储——引导卷——点击问题引导卷——选择附加到实例——通过粘贴正常实例OCID来挂载
通过实例去挂载附加卷显示灰色，无法挂载。   通过卷去找实例进行挂载
## 附加到实例  然后查看磁盘
lsblk        fdisk -l
dd系统前挂载到正常实例的命令
### 连接iSCSI
sudo iscsiadm -m node -o new -T iqn.2015-02.oracle.boot:uefi -p 169.254.2.2:3260
sudo iscsiadm -m node -o update -T iqn.2015-02.oracle.boot:uefi -n node.startup -v automatic
sudo iscsiadm -m node -T iqn.2015-02.oracle.boot:uefi -p 169.254.2.2:3260 -l

## 说明：为了防止你在SSH连接在恢复数据中途中断导致失败，建议使用 srceen 后台窗口运行以下命令
screen -S huifu  （名称为huifu）

## 打开新ssh 运行下面的命令查看进度
watch -n 5 pkill -USR1 ^dd$

gzip -dc '救援包完整路径' | dd of='引导卷加载路径'

例：
使用 Ubuntu 20.04 ARM 官方原版完整救援包，命令如下：
gzip -dc /root/Ubuntu20.04.arm.img.gz | dd of=/dev/sdb

使用 Ubuntu 20.04 AMD 官方原版完整救援包，命令如下：
gzip -dc /root/Ubuntu20.04.amd.img.gz | dd of=/dev/sdb

使用 debian10 ARM 网络精简救援包，命令如下：
gzip -dc /root/dabian10.arm.img.gz | dd of=/dev/sdb




## DD完成后解挂 引导卷
### 断开iSCSI
sudo iscsiadm -m node -T iqn.2015-02.oracle.boot:uefi -p 169.254.2.2:3260 -u
sudo iscsiadm -m node -o delete -T iqn.2015-02.oracle.boot:uefi -p 169.254.2.2:3260

## 把引导卷分离后  重新挂到原来的实例  启动实例 用密钥登录

# 参考  
### https://cnboy.org/2074
### https://blog.csdn.net/gba3455/article/details/131146366
# CNBoy 四海部落：https://cnboy.org 
