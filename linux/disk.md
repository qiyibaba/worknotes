# 磁盘分区划分

```shell
#1）分区
# 查看挂载的新磁盘信息：
# fdisk -l|grep ^"Disk /dev"
显示4块裸盘，假如大小都是 3276 GB
#fdisk -l nvme0n1
#fdisk -l nvme0n2
#fdisk -l nvme0n3
#fdisk -l nvme0n4

2）对4个裸盘，创建PV 
#pvdisplay
#pvs

创建PV
#pvcreate /dev/nvme0n1
#pvcreate /dev/nvme0n2
#pvcreate /dev/nvme0n3
#pvcreate /dev/nvme0n4

#pvdisplay
#pvs

3）创建VG
#vgdisplay
#vgs
vgcreate vg_ob_data /dev/nvme0n1 /dev/nvme0n2 /dev/nvme0n3 /dev/nvme0n4
#vgdisplay
#vgs

4）创建LV
#lvdisplay
#lvs
lvcreate -L 3000G -n ob_log1 vg_ob_data --stripes=4 --stripesize=128
lvcreate -L 6000G -n ob_data ob_vg --stripes=4 --stripesize=128
#lvdisplay
#lvs

5）创建目录
mkdir -p /data/log1
mkdir -p /data/1

6）格式化磁盘
mkfs.ext4 /dev/vg_ob_data/ob_log1
mkfs.ext4 /dev/vg_ob_data/ob_data

7）挂载磁盘
mount /dev/vg_ob_data/ob_log1 /data/log1
mount /dev/vg_ob_data/ob_data /data/1

8）编辑fstab配置文件
vi /etc/fstab -- 编辑fstab文件添加如下内容
/dev/vg_ob_data/ob_log1 /data/log1 ext4 defaults,noatime,nodiratime,nodelalloc,barrier=0 0 0
/dev/vg_ob_data/ob_data /data/1 ext4 defaults,noatime,nodiratime,nodelalloc,barrier=0 0 0
```

