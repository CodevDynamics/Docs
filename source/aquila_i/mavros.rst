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
    努力开发中