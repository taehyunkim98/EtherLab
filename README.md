# EtherLab

6.3 CPU Isolation 설정 (Xenomai 및 RT-Preempt 동일)
$
sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash i915.enable_rc6=0 i915.enable_dc=0
noapic xeno_nucleus.xenomai_gid=4390 xenomai.allowed_group=4390 isolcpus=0,1
xenomai.supported_cpus=0x3”
$
sudo update-grub

7.1 IgH EtherLAB
-Xenomai
$ cd /usr/src/realtime/
$ sudo -s
# git clone https://github.com/ribalda/ethercat.git ethercat_xenomai -> 오래 된거라 최신 gitLab꺼 사용해야 할 듯
# cd ethercat_xenomai
# ./bootstrap
# ./configure --with-xenomai-dir=/usr/xenomai --enable-cycles=yes --enable-rtdm=yes
--enable-hrtimer=yes --enable-8139too=no --enable-igb=yes --enable-e1000=yes --
enable-e1000e=yes --enable-r8169=yes --prefix=/opt/etherlab_xenomai  
# make
# make modules
# make modules_install
# make install

-RT_Preempt
$ cd /usr/src/realtime/
$ sudo -s
# git clone https://github.com/ribalda/ethercat.git
# cd ethercat
# ./bootstrap
# ./configure --enable-cycles=yes --enable-hrtimer=yes --enable-8139too=no --enable-
r8169=yes --enable-igb=yes --prefix=/opt/etherlab_rt
make
# make modules
# make modules_install
# make install

7.2 Ethernet 장치를 eth* 변경
# sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0"   -> 하면 네트워크 초기화 되는듯 인터넷 ip새로 입력해야함.
# update-grub
# reboot

7.3 IgH 사용법 (Xenomai 및 RT-Preempt 동일)
# cd /opt/etherlab_xenomai
# ln -s /opt/etherlab_xenomai/etc/ethercat.conf /etc/ethercat.conf
# ln -s /opt/etherlab_xenomai/bin/ethercat /usr/bin/
# ln -s /opt/etherlab_xenomai/sbin/ethercatctl /usr/sbin/
# ifconfig  -> 추가된 네트워크 주소
# dmesg | grep “c4:00:ad:2b:37:0a”
# vi /etc/ethercat.conf
MASTER0_DEVICE="c4:00:ad:2b:37:0a"
DEVICE_MODULES="generic"
# ethercatctl start
# ethercat slaves
# ethercatctl stop
