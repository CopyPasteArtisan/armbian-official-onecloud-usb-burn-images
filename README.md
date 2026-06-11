Amlogic USB Burning Tool 可烧录/刷机的玩客云3/赚钱宝3 Armbian官方镜像，官方镜像默认写入USB/SD卡，现在可以直接写入eMMC启动 <br>
Generate OneCloud (WS1608) Armbian images for Amlogic USB Burning Tool <br>


* 官方镜像站：https://archive.armbian.com/onecloud/archive/ <br>

* 默认官方镜像：Armbian_24.11.1_Onecloud_bookworm_current_6.6.43_minimal.img.xz <br>

  ```
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43.img.txt	19565 bytes	2024-08-27 05:09:39
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43.img.xz	412498228 bytes	2024-08-27 05:10:17
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43.img.xz.asc	833 bytes	2024-08-27 05:10:17
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43.img.xz.sha	178 bytes	2024-08-27 05:10:17
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43.img.xz.torrent	39577 bytes	2024-08-27 05:10:17
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_minimal.img.txt	19613 bytes	2024-08-27 05:10:17
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_minimal.img.xz	224204276 bytes	2024-08-27 05:11:05
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_minimal.img.xz.asc	833 bytes	2024-08-27 05:11:05
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_minimal.img.xz.sha	186 bytes	2024-08-27 05:11:05
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_minimal.img.xz.torrent	25536 bytes	2024-08-27 05:11:05
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_xfce_desktop.img.txt	19643 bytes	2024-08-27 05:38:56
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_xfce_desktop.img.xz	1194535204 bytes	2024-08-27 05:40:48
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_xfce_desktop.img.xz.asc	833 bytes	2024-08-27 05:40:48
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_xfce_desktop.img.xz.sha	191 bytes	2024-08-27 05:40:48
  📄 Armbian_24.8.1_Onecloud_bookworm_current_6.6.43_xfce_desktop.img.xz.torrent	99765 bytes	2024-08-27 05:40:48
  📄 Armbian_24.8.1_Onecloud_jammy_current_6.6.43.img.txt	19547 bytes	2024-08-27 05:14:02
  📄 Armbian_24.8.1_Onecloud_jammy_current_6.6.43.img.xz	424950180 bytes	2024-08-27 05:15:02
  📄 Armbian_24.8.1_Onecloud_jammy_current_6.6.43.img.xz.asc	833 bytes	2024-08-27 05:15:02
  📄 Armbian_24.8.1_Onecloud_jammy_current_6.6.43.img.xz.sha	175 bytes	2024-08-27 05:15:02
  📄 Armbian_24.8.1_Onecloud_jammy_current_6.6.43.img.xz.torrent	40405 bytes	2024-08-27 05:15:02
  📄 Armbian_24.8.1_Onecloud_noble_current_6.6.43_minimal.img.txt	19595 bytes	2024-08-27 05:10:23
  📄 Armbian_24.8.1_Onecloud_noble_current_6.6.43_minimal.img.xz	214209416 bytes	2024-08-27 05:11:28
  📄 Armbian_24.8.1_Onecloud_noble_current_6.6.43_minimal.img.xz.asc	833 bytes	2024-08-27 05:11:28
  📄 Armbian_24.8.1_Onecloud_noble_current_6.6.43_minimal.img.xz.sha	183 bytes	2024-08-27 05:11:28
  📄 Armbian_24.8.1_Onecloud_noble_current_6.6.43_minimal.img.xz.torrent	24658 bytes	2024-08-27 05:11:28
  ```

* 默认用户名/密码：root/1234 <br>

* 修改主机名，包括上游DHCP列表中的名字：
  ```shell
  // 先查看你的hosts
  cat /etc/hosts
  
  // 供参考
  hostnamectl set-hostname WS1608-006220 && sudo sed -i 's/\bonecloud\b/WS1608-006220/g' /etc/hosts && grep WS1608 /etc/hosts
  ```

* 如果SSH登录后没有MOTD(Message of the day)，SSH执行下面命令，然后重新登录:
  ```shell
  // 原因是 /etc/update-motd.d/* 文件没有执行权限，默认644
  chmod 755 -R /etc/update-motd.d/

  // 检查修改结果
  ls -lh /etc/update-motd.d/
  ```
  <img width="792" height="503" alt="OneCloud running Armbian Linux 6 6 43-current-meson--Ubuntu stable (noble)" src="https://github.com/user-attachments/assets/6815ef7d-2e68-4ec4-b76b-2e2798051eef" /> <br>

* 默认红色LED灯，改为绿色:
  ```shell
  // none为关闭，default-on为常亮
  echo "none" > /sys/class/leds/onecloud\:blue\:alive/trigger && echo "default-on" > /sys/class/leds/onecloud\:green\:alive/trigger && echo "none" > /sys/class/leds/onecloud\:red\:alive/trigger
  ```

* 固定原厂机身MAC地址，MACAddress=[你的设备MAC地址]:
  ```shell
  // Netplan
  tee /etc/systemd/network/10-eth0.link <<-'EOF'
  [Match]
  OriginalName=eth0
  
  [Link]
  MACAddress=00:22:6D:65:E2:DD
  MACAddressPolicy=none
  EOF
  
  // 
  udevadm control --reload && udevadm trigger
  
  // 如果没有生效，一般重启生效
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

