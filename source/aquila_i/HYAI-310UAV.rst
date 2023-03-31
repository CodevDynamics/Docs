机载电脑 - NX
=====================================

    .. image:: images/board_interface1.png

    .. image:: images/board_interface2.png

    
    ===========  ===========================  ===============================
    标记          接口                          描述
    ===========  ===========================  ===============================
    J1            NVIDIA NX / Nano             Nano or NX 266 PIN Connector
    J2            CAN                          GH125-2P-弯
    J3            SIP                          电源开关跳线
    J5            GPIO                         8 个可编程 GPIO
    J6            DEBUG                        TTL 电平
    J7            USB 2.0 Type C               REC 烧写端口
    J8            XT30 Power                   12~36V
    J9/J14/J18    USB 2.0                      GH125-4P
    J10           NVIDIA Gigabit Ethernet      RJ45 Gigabit Ethernet Connector (10/100/1000)
    J11/J13       UART                         TTL 电平
    J12           FAN CONNECT                  Picoblade Header
    J15/J16       USB 3.0 Type A               USB 3.0 Link 1 Type A Connector
    J17           HDMI                         Right Angle Vertical Connector
    J19/J21       CSI CONNECT                  FFC 30 CAMERA input
    J20           TF Card                      TF 卡扩展接口
    J22           Power Switch                 电源开关接口
    J23           Key                          Recover for Debug
    J24           RTC                          RTC 接口
    J25           Recover                      REC 接口
    ===========  ===========================  ===============================

基本信息
-------------
    Jetson Xavier NX(P3668-0001 module) - Ubuntu Desktop 18.04

软件支持列表
-------------
    ===========================  ===============================
     软件名称                       版本
    ===========================  ===============================
     GStreamer                     1.14.5
     Docker                        20.10.2
     OpenCV                        4.1.1
     TensorRT                      8.0.1.6-1
     CUDA                          10.2
     Python                        2.7/3.6
     ROS                           melodic-base
    ===========================  ===============================

网络信息
-------------
    * RJ45 有线网卡：192.168.2.116/24，无网关，用户不可更改。

    * USB-C 虚拟网卡：192.168.55.1，网关：192.168.55.100，网关为连接的Host电脑端虚拟网卡的IP，用户不可更改。

上网指南
-------------
    因为RJ45网卡已经被飞机使用，所以不能通过网线直接上网，我们推荐使用USB-C连接Ubuntu系统的电脑，开启转发使机载电脑上网，具体步骤如下

    * 使用“USB-C to USB-A”连接机载电脑和 Ubuntu 主机。

    * 在 Ubuntu 主机上通过命令确认可以上网的网卡名称以及连接机载电脑虚拟网卡名称，如图，**wlo1** 是可以连接外网的网卡，**enxc6c904244806** 是连接机载电脑的网卡。

        .. image:: images/ifconfig.png



    * 根据自己的网络环境获得两张网卡名称后，执行以下命令::

        echo "1" > /proc/sys/net/ipv4/ip_forward
        iptables -t nat -A POSTROUTING -o wlo1 -j MASQUERADE
        iptables -A FORWARD -i wlo1 -o enxc6c904244806 -m state --state RELATED,ESTABLISHED -j ACCEPT
        iptables -A FORWARD -i wlo1 -o enxc6c904244806 -j ACCEPT
        iptables-save > /etc/iptables.rules

    * 该流程在主机电脑重启后会失效，需要重新执行，建议写成脚本。*(注意：连接机载电脑的网卡的名称可能会发生改变)*

开发手册
-------------
    使用 MAVROS 与飞机和相机进行通信，使用 Codev ROS SDK 与地面站遥控器通信。

    .. toctree::
        :maxdepth: 2

        mavros.rst


    .. toctree::
        :maxdepth: 2

        codevrossdk.rst
