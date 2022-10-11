# numa使用

## 关闭numa

```shell
#关闭的方式，证明可行的 【redhat 7.4】
vi /etc/default/grub
#在 GRUB_CMDLINE_LINUX 参数的末尾增加 ： numa=off
#例如：GRUB_CMDLINE_LINUX="crashkernel=auto rd.lvm.lv=vg_root/root rd.lvm.lv=vg_root/swap rhgb quiet numa=off"

#重建grub 配置文件
#MBR 分区表
grub2-mkconfig -o /etc/grub2.cfg

#efi 引导模式， efi + GPT分区表
grub2-mkconfig -o /etc/grub2-efi.cfg

#重启机器，再来检查numa 是否被关闭即可
```

