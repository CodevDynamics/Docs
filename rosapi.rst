ROS2 API
=====================================
    运行在机载设备上的 ROS2 节点，可用于控制飞机、机库、相机以及一些其他工具。

Message
-----------------------

* :ros:msg:`codev_msgs/msg/AirportStatus`
* :ros:msg:`codev_msgs/msg/AutopilotTelemetry`
* :ros:msg:`codev_msgs/msg/CameraTelemetry`
* :ros:msg:`codev_msgs/msg/ComputerStatus`
* :ros:msg:`codev_msgs/msg/MissionProgress`
* :ros:msg:`codev_msgs/msg/MissionStatus`

.. ros:automessage:: codev_msgs/msg/AirportStatus
   :raw: tail
   :field-comment: right1

.. ros:automessage:: codev_msgs/msg/AutopilotTelemetry
   :raw: tail
   :field-comment: right1

.. ros:automessage:: codev_msgs/msg/CameraTelemetry
   :raw: tail
   :field-comment: right1

.. ros:automessage:: codev_msgs/msg/ComputerStatus
   :raw: tail
   :field-comment: right1

.. ros:automessage:: codev_msgs/msg/MissionProgress
   :raw: tail
   :field-comment: right1

.. ros:automessage:: codev_msgs/msg/MissionStatus
   :raw: tail
   :field-comment: right1


Service
-----------------------

* :ros:srv:`codev_msgs/srv/AirportAtomic`
* :ros:srv:`codev_msgs/srv/AutopilotAtomic`
* :ros:srv:`codev_msgs/srv/CurrentPlan`
* :ros:srv:`codev_msgs/srv/MissionPaths`

.. ros:autoservice:: codev_msgs/srv/AirportAtomic
   :raw: tail
   :field-comment: right1

.. ros:autoservice:: codev_msgs/srv/AutopilotAtomic
   :raw: tail
   :field-comment: right1

.. ros:autoservice:: codev_msgs/srv/CurrentPlan
   :raw: tail
   :field-comment: right1

.. ros:autoservice:: codev_msgs/srv/MissionPaths
   :raw: tail
   :field-comment: right1


`geometry_msgs/msg/GeoPath <http://docs.ros.org/en/api/geometry_msgs/html/msg/GeoPath.html>`_  

Actions
-----------------------

* :ros:action:`codev_msgs/action/AirportAtomic`
* :ros:action:`codev_msgs/action/AirportCombinations`
* :ros:action:`codev_msgs/action/MissionAction`
* :ros:action:`codev_msgs/action/MissionDownload`
* :ros:action:`codev_msgs/action/MissionUpload`

.. ros:autoaction:: codev_msgs/action/AirportAtomic
   :raw: tail
   :field-comment: right1

.. ros:autoaction:: codev_msgs/action/AirportCombinations
   :raw: tail
   :field-comment: right1

.. ros:autoaction:: codev_msgs/action/MissionAction
   :raw: tail
   :field-comment: right1

.. ros:autoaction:: codev_msgs/action/MissionDownload
   :raw: tail
   :field-comment: right1

.. ros:autoaction:: codev_msgs/action/MissionUpload
   :raw: tail
   :field-comment: right1

接口说明 **(Node)**
--------------------------

.. ros:node:: codevcore/mavlink_autopilot

    :pub ~/aircraft_position: 飞机原始 GPS 信息
    :pub-type ~/aircraft_position: `sensor_msgs/msg/NavSatFix <http://docs.ros.org/en/api/sensor_msgs/html/msg/NavSatFix.html>`_
    
    :pub ~/aircraft_local_ned: 飞机位置信息
    :pub-type ~/aircraft_local_ned: `geometry_msgs/msg/PoseStamped <http://docs.ros.org/en/api/geometry_msgs/html/msg/PoseStamped.html>`_

    :pub ~/telemetry: 飞机遥测数据
    :pub-type ~/telemetry: :ros:msg:`codev_msgs/msg/AutopilotTelemetry`

    :sub ~/manual_control: 发送遥感控制飞机

        *linear.x -1.0～1.0 飞机前后控制*

        *linear.y -1.0～1.0 飞机左右控制*

        *linear.z -1.0～1.0 0～1.0 飞机上下控制，中位0.5保持*

        *angular.z -1.0～1.0 飞机旋转控制*
    :sub-type ~/manual_control: `geometry_msgs/msg/Twist <http://docs.ros.org/en/api/geometry_msgs/html/msg/Twist.html>`_

    :srv ~/arm: 飞机锁定与接触锁定

        *解锁:* ``'{ "param1": 1 }'``

        *锁定:* ``'{ "param1": 0 }'``
    :srv-type ~/arm: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/takeoff: 飞机起飞 *（参数缺省）*
    :srv-type ~/takeoff: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/land: 飞机原地降落 *（参数缺省）*
    :srv-type ~/land: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/return_to_launch: 飞机返航 *（参数缺省）*
    :srv-type ~/return_to_launch: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/goto_location: 飞机到达指定点悬停 *（参数缺省）*
    :srv-type ~/goto_location: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/do_orbit: 飞机到达指定点盘旋 *（参数缺省）*
    :srv-type ~/do_orbit: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/hold: 飞机悬停 *（参数缺省）*
    :srv-type ~/hold: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/position_control: 飞机切换到位置飞行模式 *（参数缺省）*
    :srv-type ~/position_control: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/mission_continue: 飞机继续执行任务 **（前提：飞机已经开始任务）** *（参数缺省）*
    :srv-type ~/mission_continue: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/mission_paths: 获取飞机上的任务列表（含航线）

        *获得所有任务：参数缺省*

        *获得指定任务:* ``'{ "name": "test.mission" }'``
    :srv-type ~/mission_paths: :ros:srv:`codev_msgs/srv/MissionPaths`

    :srv ~/current_plan: 获得飞机当前任务
    :srv-type ~/current_plan: :ros:srv:`codev_msgs/srv/CurrentPlan`

    :action ~/mission_upload: 上传任务 ``'{ "name": "upload.mission" }'``
    :action-type ~/mission_upload: :ros:action:`codev_msgs/action/MissionUpload`

    :action ~/mission_download: 下载飞机上的任务到文件 ``'{ "name": "down.mission" }'``
    :action-type ~/mission_download: :ros:action:`codev_msgs/action/MissionDownload`

    :action ~/mission_action: 执行指定任务 ``'{ "name": "action.mission" }'``
    :action-type ~/mission_action: :ros:action:`codev_msgs/action/MissionAction`

.. ros:node:: codevcore/mavlink_camera

    :pub ~/telemetry: 相机遥测数据
    :pub-type ~/telemetry: :ros:msg:`codev_msgs/msg/CameraTelemetry`

    :sub ~/manual_control: 发送遥感控制相机

        *angular.x 相机 Pitch 角度控制*

        *angular.z 相机 Yaw 角度控制*
    :sub-type ~/manual_control: geometry_msgs/msg/Twist

    :srv ~/take_photo: 相机拍照 *（参数缺省）*
    :srv-type ~/take_photo: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/start_video: 相机开始录像 *（参数缺省）*
    :srv-type ~/start_video: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

    :srv ~/stop_video: 相机停止录像 *（参数缺省）*
    :srv-type ~/stop_video: :ros:srv:`codev_msgs/srv/AutopilotAtomic`

.. ros:node:: codevcore/plc_airport

    :pub ~/airport_status: 机库 PLC 状态数据
    :pub-type ~/airport_status: :ros:msg:`codev_msgs/msg/AirportStatus`

    :srv ~/plc_power: PLC 控制电源

        *开电源:* ``'{ "on": true }'``

        *关电源:* ``'{ "on": false }'``
    :srv-type ~/plc_power: :ros:srv:`codev_msgs/srv/AirportAtomic`

    :srv ~/aircondition: 空调开关

        *开空调:* ``'{ "on": true }'``

        *关空调:* ``'{ "on": false }'``
    :srv-type ~/aircondition: :ros:srv:`codev_msgs/srv/AirportAtomic`

    :action ~/door: 舱门控制
        
        *开舱门:* ``'{ "forward": true }'``

        *关舱门:* ``'{ "forward": false }'``
    :action-type ~/door: :ros:action:`codev_msgs/action/AirportAtomic`

    :action ~/lift: 推举控制
        
        *升推举:* ``'{ "forward": true }'``

        *降推举:* ``'{ "forward": false }'``
    :action-type ~/lift: :ros:action:`codev_msgs/action/AirportAtomic`

    :action ~/vertical: 前后限位控制
        
        *加紧前后限位:* ``'{ "forward": true }'``

        *松开前后限位:* ``'{ "forward": false }'``
    :action-type ~/vertical: :ros:action:`codev_msgs/action/AirportAtomic`

    :action ~/horizontal: 左右限位控制
        
        *加紧左右限位:* ``'{ "forward": true }'``

        *松开左右限位:* ``'{ "forward": false }'``
    :action-type ~/horizontal: :ros:action:`codev_msgs/action/AirportAtomic`

    :action ~/out_bound: 连续动作-出库
    :action-type ~/out_bound: :ros:action:`codev_msgs/action/AirportCombinations`

    :action ~/in_bound: 连续动作-入库
    :action-type ~/in_bound: :ros:action:`codev_msgs/action/AirportCombinations`

.. ros:node:: codevcore/monitor

    :pub ~/device_status: 机库机载电脑监控数据
    :pub-type ~/device_status: :ros:msg:`codev_msgs/msg/ComputerStatus`

.. ros:node:: codevcore/gps_parser

    :pub ~/sensor_gps: 机库 GPS 原始数据
    :pub-type ~/sensor_gps: `sensor_msgs/msg/NavSatFix <http://docs.ros.org/en/api/sensor_msgs/html/msg/NavSatFix.html>`_

    :pub ~/rtcm_data: 机库 GPS 基站 RTCM3.3 数据
    :pub-type ~/rtcm_data: `std_msgs/msg/ByteMultiArray <http://docs.ros.org/en/api/std_msgs/html/msg/ByteMultiArray.html>`_
