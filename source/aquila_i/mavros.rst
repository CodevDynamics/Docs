MAVROS
=====================================
    该文档包含两部分：MAVROS 官方文档和 Codev 认证的 MAVROS 接口。

MAVROS 官方文档
-----------------
    用于 ROS 的 MAVLink 通信节点。`官方文档地址 <http://wiki.ros.org/mavros/>`_

Codev 认证的 MAVROS 接口
--------------------------
    我们不会对 MAVROS 的代码接口进行任何修改，但是我们会对我们认为可能会存在问题的关键命令进行测试，并通过修改飞控的方式来支持 MAVROS，我们会在这里将我们测试过的或者通过修改飞控支持过的命令列举出来。

MAVROS Offboard 控制
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    以下是一个通过飞机 Offboard 模式起飞的 C++ 例子：

    .. literalinclude:: code/offboard/src/offb_node.cpp
        :language: c

    以下是一个通过飞机 Offboard 模式起飞的 Python 例子：

    .. literalinclude:: code/offboard/scripts/offb_node.py


MAVROS 云台控制
^^^^^^^^^^^^^^^
    命令由飞机转发给云台，所以在此之前一定要确认飞机参数 'SYS_GIMBAL_TYPE' 为 6::

        # 读取飞机参数，如果为6，则跳过下一条命令
        rosrun mavros mavparam get SYS_GIMBAL_TYPE
        # 设置飞机参数 SYS_GIMBAL_TYPE=6
        # SYS_GIMBAL_TYPE=6 机载电脑控制模式
        # SYS_GIMBAL_TYPE=1 地面站控制模式
        rosrun mavros mavparam set SYS_GIMBAL_TYPE 6


Bash 快速测试
"""""""""""""
    ::

        # 使用 MAVROS 发送 Mavlink Command: MAV_CMD_DO_MOUNT_CONTROL(#205)
        # 参数1: ROS header
        # 参数2: 2 角度模式 1 角速度模式，当前角度模式 2
        # 参数3: Pitch = -90 垂直向下
        # 参数4: Roll = 0 水平保持，一般不可控
        # 参数5: Yaw = 0 偏航角
        # 参数6、7、8 保持0，无含义
        rostopic pub -1 /mavros/mount_control/command mavros_msgs/MountControl -- '[0, 0, ""]' 2 -90 0 10 0 0 0
        # 获得 Gimbal 姿态
        rostopic echo /mavros/mount_control/orientation


C++ Demo
"""""""""""""
    .. literalinclude:: code/gimbal/src/gimbal_node.cpp
        :language: c

Python Demo
"""""""""""""
    .. literalinclude:: code/gimbal/scripts/gimbal_node.py


Demo 仓库地址
^^^^^^^^^^^^^^^
    `Github 仓库地址 <https://github.com/CodevDynamics/ROSDemo>`_
