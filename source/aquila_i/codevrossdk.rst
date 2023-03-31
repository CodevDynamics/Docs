Codev ROS SDK
=====================================
    主要提供给使用 Codev Aquila I 飞行平台的开发者使用，通过该 SDK 可以与我们的 FlyDynamic 2.0 地面站进行通信与交互。

跟踪交互
-------------
    接口不提供跟踪算法，只是提供用于跟踪算法输入/输出的软件接口

    .. uml:: puml/Tracking.puml
    
Bash 快速测试
^^^^^^^^^^^^^^^
    ::

        rostopic pub -1 /codevqtros/tracking/Object codevqtros_msgs/TrackingObject -- 0.45 0.45 0.1 0.1 1
        # 发送 0 0 0 0 0 可以立刻隐藏对象框
        rostopic pub -1 /codevqtros/tracking/Object codevqtros_msgs/TrackingObject -- 0 0 0 0 0


C++ Demo
^^^^^^^^^^^^^^^
    .. literalinclude:: code/qtros/src/tracking_node.cpp
        :language: c

Python Demo
^^^^^^^^^^^^^^^
    .. literalinclude:: code/qtros/scripts/tracking_node.py


识别交互
-------------
    接口不提供识别算法，只是提供用于识别算法输入/输出的软件接口

    .. uml:: puml/Detect.puml

Bash 快速测试
^^^^^^^^^^^^^^^
    ::

        rostopic pub -1 /codevqtros/detect/Objects codevqtros_msgs/DetectObject -- 0.45 0.45 0.1 0.1 'apple' \'\#f000f0\'
        # 发送 0 0 0 0 0 可以立刻清除所有对象框
        rostopic pub -1 /codevqtros/detect/Objects codevqtros_msgs/DetectObject -- 0 0 0 0 'apple' \'\#f000f0\'


C++ Demo
^^^^^^^^^^^^^^^
    .. literalinclude:: code/qtros/src/detect_node.cpp
        :language: c

Python Demo
^^^^^^^^^^^^^^^
    .. literalinclude:: code/qtros/scripts/detect_node.py
