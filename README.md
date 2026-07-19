# 仓库/项目简介
  Amlogic USB Burning Tool 可烧录/刷机的玩客云3/赚钱宝3 Armbian官方镜像，官方镜像默认写入USB/SD卡，现在可以直接写入eMMC启动 <br>
  Generate OneCloud (WS1608) Armbian images for Amlogic USB Burning Tool <br>

  <img alt="OneCloud running Armbian Linux 6 6 43-current-meson--Ubuntu stable (noble)" src="https://raw.githubusercontent.com/CopyPasteArtisan/armbian-official-onecloud-usb-burn-images/refs/heads/main/img/Xshell_Zz0XSVYmAb-1.png" /> <br>

## 线刷包报错问题
  * 如果AMD平台笔记本/台式机用 Amlogic USB Burning Tool 刷入镜像时在1%处报错，无论插USB 2.0 USB 3.0端口。换Intel平台的笔记本/台式机再刷机。这是USB口兼容性问题，或者是USB控制器的差异 <br>
    ```
    1% [0x10102002]Romcode/初始化DDR/初始化寄存器/USB控制命令出错
    [HUBx-x][Err]--[0x10102002]Romcode/初始化DDR/初始化寄存器/USB控制命令出错
    ```
    <img src=https://raw.githubusercontent.com/CopyPasteArtisan/armbian-official-onecloud-usb-burn-images/refs/heads/main/img/Amlogic%20USB%20Burning%20Tool%20v2.2.4_AMD_USB2.0_2026-07-19_19-21-19.png />
  * 我没试过用USB 2.0转接器/拓展坞能不能解决这个问题，在没有可替代的笔记本/台式机时你可以尝试 <br>
    
## Armbian的相关信息
* 当前镜像站：[https://docs.armbian.com/Mirrors/#current-mirrors](https://docs.armbian.com/Mirrors/#current-mirrors) ，如果不可用使用其他镜像站代替
* 各分支URL地址
  |Content |URL |Description |
  |---|---|---|
  |Current images |https://rsync.armbian.com/dl |当前主流版本 |
  |Archived images |https://rsync.armbian.com/archive |上一正式版本存档 |
  |Very old images |https://rsync.armbian.com/oldarchive |更老的正式版本存档 |
  |Armbian community |https://github.com/armbian/community/releases |社区版本，内核较新，支持老设备 |
* Actions默认官方镜像：Armbian_24.11.1_Onecloud_bookworm_current_6.6.43_minimal.img.xz
* Actions工作流程参考了 https://github.com/hzyitc/armbian-onecloud 项目，感谢@hzyitc
* 目前有以下官方img.xz打包的线刷包，并且跟随Armbian官方更新

  ```
  Armbian_23.11.1_Onecloud_jammy_current_6.1.63_minimal.img.xz
  Armbian_23.11.1_Onecloud_bookworm_current_6.1.63_minimal.img.xz
  Armbian_24.8.1_Onecloud_noble_current_6.6.43_minimal.img.xz
  Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_minimal.img.xz
  Armbian_24.11.1_Onecloud_noble_current_6.6.43_minimal.img.xz
  Armbian_24.11.1_Onecloud_bookworm_current_6.6.43_minimal.img.xz
  Armbian_community_26.2.0-trunk.904_Onecloud *
  Armbian_community_26.8.0-trunk.121_Onecloud *
  Armbian_community_26.8.0-trunk.170_Onecloud *
  Armbian_community_26.8.0-trunk.359_Onecloud *
  Armbian_community_26.8.0-trunk.413_Onecloud *
  ```

* 默认用户名/密码：root/1234

* 修改主机名，包括上游DHCP列表中的名字：
  ```shell
  // 先查看你的hosts
  cat /etc/hosts
  
  // 供参考
  hostnamectl set-hostname WS1608-006220 && sudo sed -i 's/\bonecloud\b/WS1608-006220/g' /etc/hosts && grep WS1608 /etc/hosts
  ```

* 如果SSH登录后没有MOTD (Message of the Day) ，Shell执行下面命令，然后重新登录:
  ```shell
  // 原因是 /etc/update-motd.d/* 文件没有执行权限，默认644
  chmod 755 /etc/update-motd.d/*

  // 检查修改结果
  ls -lh /etc/update-motd.d/
  ```
  
* 默认红色LED灯，改为绿色:
  ```shell
  // none为关闭，default-on为常亮
  echo "none" > /sys/class/leds/onecloud\:blue\:alive/trigger && echo "default-on" > /sys/class/leds/onecloud\:green\:alive/trigger && echo "none" > /sys/class/leds/onecloud\:red\:alive/trigger
  ```

* 恢复原厂主板标签上的MAC地址，注意格式，MACAddress=[你的设备MAC地址]:
  ```shell
  // 适用于Armbian_24.11.1，Armbian_23.11.1不适用以下步骤，它使用nmcli管理
  tee /etc/systemd/network/10-eth0.link <<-'EOF'
  [Match]
  OriginalName=eth0
  
  [Link]
  MACAddress=00:22:6D:65:E2:DD
  MACAddressPolicy=none
  EOF
  
  // 重启生效
  init 6
  
  // 或
  reboot
  

  // 适用于Armbian_23.11.1，nmcli查看网络详情
  nmcli connection show
  
  NAME                UUID                                  TYPE      DEVICE 
  Wired connection 1  91d281cd-c62b-33ff-935d-7ca79f27ff40  ethernet  eth0
  
  // 修改成你的MAC地址
  nmcli connection modify "Wired connection 1" 802-3-ethernet.cloned-mac-address 00:22:6D:65:E2:DD
  
  // 查看配置文件
  cat /etc/NetworkManager/system-connections/Wired\ connection\ 1.nmconnection
  
  [connection]
  id=Wired connection 1
  uuid=b0a941f8-9771-30ab-92b5-7e77882e34e6
  type=ethernet
  autoconnect-priority=-999
  interface-name=eth0
  timestamp=1701306574
  
  [ethernet]
  cloned-mac-address=00:22:6D:65:E2:DD
  
  [ipv4]
  method=auto
  
  [ipv6]
  addr-gen-mode=default
  method=auto
  
  [proxy]
  
  
  // 重启生效
  init 6
  
  // 或
  reboot

  ```
* 其他信息：

  ```shell
  // free -h
                 total        used        free      shared  buff/cache   available
  Mem:           988Mi        90Mi       784Mi       3.7Mi       140Mi       898Mi
  Swap:          494Mi          0B       494Mi
  ```
  
  ```shell
  // df -h
  Filesystem      Size  Used Avail Use% Mounted on
  tmpfs            99M  3.7M   96M   4% /run
  /dev/mmcblk1p2  6.9G  816M  6.0G  12% /
  tmpfs           495M     0  495M   0% /dev/shm
  tmpfs           5.0M     0  5.0M   0% /run/lock
  tmpfs           495M     0  495M   0% /tmp
  /dev/mmcblk1p1  256M   28M  229M  11% /boot
  /dev/zram1       47M  1.2M   43M   3% /var/log
  tmpfs            99M  4.0K   99M   1% /run/user/0
  ```
  
  ```shell
  // lsblk
  NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
  mmcblk1      179:0    0   7.3G  0 disk 
  ├─mmcblk1p1  179:1    0   256M  0 part /boot
  └─mmcblk1p2  179:2    0     7G  0 part /var/log.hdd
                                         /
  mmcblk1boot0 179:16   0     4M  1 disk 
  mmcblk1boot1 179:32   0     4M  1 disk 
  zram0        253:0    0 494.2M  0 disk [SWAP]
  zram1        253:1    0    50M  0 disk /var/log
  zram2        253:2    0     0B  0 disk 
  
  ```
  
  ```shell
  // mmc extcsd read /dev/mmcblk1 | grep -iE 'life|eof'
  eMMC Life Time Estimation A [EXT_CSD_DEVICE_LIFE_TIME_EST_TYP_A]: 0x01
  eMMC Life Time Estimation B [EXT_CSD_DEVICE_LIFE_TIME_EST_TYP_B]: 0x01
  ```
  
  ```shell
  // dpkg --print-architecture
  armhf
  ```
  
  ```shell
  // uname -a
  Linux WS1608-006220 6.6.43-current-meson #1 SMP Sat Jul 27 09:34:11 UTC 2024 armv7l armv7l armv7l GNU/Linux
  ```
  
  ```shell
  // cat /etc/os-release 
  PRETTY_NAME="Armbian 24.11.1 noble"
  NAME="Ubuntu"
  VERSION_ID="24.04"
  VERSION="24.04 LTS (Noble Numbat)"
  VERSION_CODENAME=noble
  ID=ubuntu
  ID_LIKE=debian
  HOME_URL="https://www.armbian.com"
  SUPPORT_URL="https://forum.armbian.com"
  BUG_REPORT_URL="https://www.armbian.com/bugs"
  PRIVACY_POLICY_URL="https://www.armbian.com"
  UBUNTU_CODENAME=noble
  LOGO="armbian-logo"
  ARMBIAN_PRETTY_NAME="Armbian 24.11.1 noble"
  
  ```
  
  ```shell
  // lscpu
  Architecture:             armv7l
    Byte Order:             Little Endian
  CPU(s):                   4
    On-line CPU(s) list:    0-3
  Vendor ID:                ARM
    Model name:             Cortex-A5
      Model:                1
      Thread(s) per core:   1
      Core(s) per socket:   4
      Socket(s):            1
      Stepping:             r0p1
      CPU(s) scaling MHz:   33%
      CPU max MHz:          1536.0000
      CPU min MHz:          96.0000
      BogoMIPS:             1.27
      Flags:                half thumb fastmult vfp edsp thumbee neon vfpv3 tls vfpv4 vfpd32
  Vulnerabilities:          
    Gather data sampling:   Not affected
    Itlb multihit:          Not affected
    L1tf:                   Not affected
    Mds:                    Not affected
    Meltdown:               Not affected
    Mmio stale data:        Not affected
    Reg file data sampling: Not affected
    Retbleed:               Not affected
    Spec rstack overflow:   Not affected
    Spec store bypass:      Not affected
    Spectre v1:             Mitigation; __user pointer sanitization
    Spectre v2:             Not affected
    Srbds:                  Not affected
    Tsx async abort:        Not affected
  
  ```
  
  ```shell
  // armbianmonitor -m
  Stop monitoring using [ctrl]-[c]
  Time        CPU    load %cpu %sys %usr %nice %io %irq   Tcpu  C.St.
  
  15:47:17  1536 MHz  0.00   0%   0%   0%   0%   0%   0%  46.2 °C  0/4
  15:47:23   504 MHz  0.00   0%   0%   0%   0%   0%   0%  46.9 °C  0/4
  15:47:28   504 MHz  0.00   0%   0%   0%   0%   0%   0%  46.2 °C  0/4
  15:47:33   504 MHz  0.00   0%   0%   0%   0%   0%   0%  47.2 °C  0/4
  15:47:38   504 MHz  0.00   0%   0%   0%   0%   0%   0%  45.9 °C  0/4
  15:47:43   504 MHz  0.00   0%   0%   0%   0%   0%   0%  46.6 °C  0/4
  15:47:48   504 MHz  0.00   0%   0%   0%   0%   0%   0%  46.6 °C  0/4
  15:47:53   504 MHz  0.00   0%   0%   0%   0%   0%   0%  47.5 °C  0/4
  15:47:59   504 MHz  0.00   0%   0%   0%   0%   0%   0%  46.2 °C  0/4
  15:48:04   504 MHz  0.00   0%   0%   0%   0%   0%   0%  46.6 °C  0/4
  15:48:09   504 MHz  0.00   0%   0%   0%   0%   0%   0%  46.9 °C  0/4
  15:48:14   504 MHz  0.07   0%   0%   0%   0%   0%   0%  45.6 °C  0/4
  15:48:19   504 MHz  0.07   0%   0%   0%   0%   0%   0%  44.7 °C  0/4
  15:48:24   504 MHz  0.06   0%   0%   0%   0%   0%   0%  46.6 °C  0/4
  
  ```

