MQTT 客户端
=====================================
    运行在机载设备上的 MQTT 客户端程序：可用于推送飞机和机库的遥测数据以及异步接受来自服务器的控制命令。

.. _msg-topic-list:

MQTT Topic 格式列表
-----------------------
    目前设备只有包含 5 种 Topic 类型，如下：

    ===========  ======================================== =========================================================
    消息类别       格式                                   描述
    ===========  ======================================== =========================================================
    定频消息       nest/{Model}/{ClientID}/messages         定频发送的状态上报（设备发布给服务器）
    事件消息       nest/{Model}/{ClientID}/events           设备事件上报，当设备处于某些状态是上报（设备发布给服务器）
    机库服务       nest/{Model}/{ClientID}/services         用于机库与飞机控制（服务器发布给设备）
    服务应答       nest/{Model}/{ClientID}/services_reply   机库与飞机应答控制（设备发布给服务器）
    监听控制       nest/{Model}/{ClientID}/listeners        手动控制飞行器的遥感包（服务器发布给设备）
    ===========  ======================================== =========================================================

    *Model种类包含Codev：A300、ARS300、ARS200；DJI：AD3、ARS350、AD2*

    *文档中“终端应答”的标题的内容皆是是使用“服务应答”的 Topic 发布，不再赘述。*

    *文档中“终端发布”的标题的内容皆是是使用“定频消息”的 Topic 发布，不再赘述。*

    *文档中“终端事件发布”的标题的内容皆是是使用“事件消息”的 Topic 发布，不再赘述。*

    *文档中“服务端发布”的标题的内容皆是是使用“机库服务”的 Topic 发布，不再赘述。*

    *文档中“服务端控制发布”的标题的内容皆是是使用“监听控制”的 Topic 发布，不再赘述。*

    *每一个设备在订阅并接收到“机库服务”的 Topic 之后，在处理完成后或者完成调度都会发布“服务应答”的 Topic。*  

.. _mqtt-msg-type:

MQTT Payload 类型
-----------------------
    Payload 数据格式为 Json。关键字：**msg_type**，用于代表数据类型，目前所以消息类型如下（持续更新）：

    ===========  ================================== ===============================
    消息类型       Topic 消息类别                        描述
    ===========  ================================== ===============================
    1             :ref:`定频消息 <msg-topic-list>`    :ref:`mqtt-telemetry`
    2             :ref:`事件消息 <msg-topic-list>`    :ref:`mqtt-mission-progress`
    3             :ref:`定频消息 <msg-topic-list>`    :ref:`mqtt-airport-status`
    4             :ref:`定频消息 <msg-topic-list>`    :ref:`mqtt-schedule-status`
    5             :ref:`事件消息 <msg-topic-list>`    :ref:`mqtt-disconnected-telemetry`
    6             :ref:`事件消息 <msg-topic-list>`    :ref:`mqtt-airport-online`
    7             :ref:`事件消息 <msg-topic-list>`    :ref:`mqtt-upload-finished`
    8             :ref:`事件消息 <msg-topic-list>`    :ref:`mqtt-mission-finished`
    999           :ref:`事件消息 <msg-topic-list>`    :ref:`mqtt-log-text`
    1000          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-arm`
    1001          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-takeoff`
    1002          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-land`
    1003          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-rtl`
    1004          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-hold`
    1005          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-posctl`
    1006          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-goto-location`
    1007          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-takephoto`
    1008          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-start-video`
    1009          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-stop-video`
    1010          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-start-mission`
    1011          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-cancel-mission`
    1012          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-continue-mission`
    1013          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-push-rtmp-video-stream`
    1014          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-set-zoom`
    1015          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-aircraft-on`
    1016          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-push-rtmp-ip-camera`
    1017          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-aircraft-charge`
    1018          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-radio-power`
    1019          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-coproc-on`
    1020          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-action-lock`
    1192          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-get-aircraft-param`
    1193          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-set-aircraft-param`
    1194          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-list-aircraft-param`
    1195          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-describe-aircraft-param`
    1196          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-get-camera-param`
    1197          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-set-camera-param`
    1198          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-list-camera-param`
    1199          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-describe-camera-param`
    1200          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-airport-door`
    1201          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-stop-airport-door`
    1202          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-airport-lift`
    1203          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-stop-airport-lift`
    1204          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-airport-vertical`
    1205          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-stop-airport-vertical`
    1206          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-airport-horizontal`
    1207          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-stop-airport-horizontal`
    1296          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-airport-outbound`
    1297          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-stop-airport-outbound`
    1298          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-airport-inbound`
    1299          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-stop-airport-inbound`
    1300          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-kill-schedule`
    1399          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-schedule-rtl-in-idle`
    1301          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-schedule-mission`
    1302          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-schedule-recovery`
    1304          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-schedule-goto-location`
    1305          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-schedule-upload`
    1496          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-get-mission-file-content`
    1497          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-delete-mission-file`
    1498          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-upload-mission-file`
    1499          :ref:`机库服务 <msg-topic-list>`    :ref:`mqtt-request-mission-list`
    1500          :ref:`监听控制 <msg-topic-list>`    :ref:`mqtt-manual-control`
    1501          :ref:`监听控制 <msg-topic-list>`    :ref:`mqtt-gimbal-manual-control`
    ===========  ================================== ===============================

.. _mqtt-result:

终端返回执行结果
-----------------------
    对于需要应答的指令，Json 数据中包含 **result**，类型为 **Int**，含义如下表：

    ===========  =======================================
    返回值             描述
    ===========  =======================================
    -1             API 模块处理接受到的执行命令时，遇到异常：Json 参数错误、API 函数返回异常值
    0              未知错误，获取航线列表、上传任务文件、执行任务时失败会出现
    1              指令执行成功或者成功开始执行
    2              执行设备不存在
    3              执行设备连接错误
    4              执行设备忙碌
    5              执行设备拒绝执行
    6              飞行器状态未知拒绝执行
    7              飞行器未着陆拒绝执行
    8              指令超时
    9              VTOL 切换失败（旋翼机不会出现）
    10             飞行器不支持切换（旋翼机不会出现）
    11             指令参数不合法
    12             指令不支持
    13             指令执行失败
    ===========  =======================================

.. _mqtt-mission-object:

任务对象格式说明
-----------------------
    ================= =========  ======== ===============================
    参数                类型       缺省      描述
    ================= =========  ======== ===============================
    latitude          Double      否       航点纬度
    longitude         Double      否       航点经度
    altitude_rel      Double      能       航点相对高度（相对 Home 点） 
    altitude_abs      Double      能       航点绝对高度（GPS 高度）
    vehicle_action    Int         能       0: 普通航点，1: 起飞，2: 降落，5: 返航
    speed             Double      能       执行到该航点时，切换飞行器速度
    camera_action     Int         能       0: 无动作，1: 拍照，4: 开始录像，5: 停止录像
    gimbal_pitch      Double      能       云台 Pitch
    gimbal_yaw        Double      能       云台 Yaw
    is_fly_through    Bool        能       `false`: 在该航点位置进行短暂（0.5s）的悬停，`true`: 快速通过
    yaw_deg           Double      能       飞机机头朝向（0-360度）
    camera_zoom       Double      能       相机Zoom倍数值，根据每个相机实际范围决定，如：30倍，值的范围1-30
    loiter_time_s     Double      能       飞机在该点悬停时间，如果该值被设置，`is_fly_through`: 将无效
    ================= =========  ======== ===============================

    **'altitude_rel' 和 'altitude_abs'，必须存在一个，如果同时存在 'altitude_abs' 优先**

.. _mqtt-param-object:

参数对象格式说明
-----------------------
    ================= =========== ======== ===============================
    参数                类型       缺省      描述
    ================= =========== ======== ===============================
    name               String      否       名称
    type               String      否       类型，只有“Int”，“Float”其中之一
    description        String      否       参数描述
    enumStrings        StringList  能       可选项名称列表（如果每个字符串中包含'<<',代表可多选,即按位赋值）
    enumValues         DoubleList  能       可选项值列表
    min                Double      能       最小值
    max                Double      能       最大值
    step               Double      能       步长，0为没有步长
    ================= =========== ======== ===============================

.. _mqtt-telemetry:

飞行器遥测数据
-----------------------

终端发布
^^^^^^^^^^^^^^^
    ================= =========  ======== ===============================
    参数                类型       缺省      描述
    ================= =========  ======== ===============================
    msg_type           Int         否       :ref:`mqtt-msg-type`
    aircraft_id        String      否       飞行器 UUID
    timestamp          Long        否       UTC 时间
    landed_state       String      否       "On Ground","In Air","Taking Off","Landing"
    flight_mode        String      否       "Ready"(可以起飞),"Takeoff","Hold","Mission","Return To Launch","Land","Posctl"
    home               Double[]    否       Home 点，4个浮点型，依次是纬度、经度、海拔高度、相对高度
    position           Double[]    否       飞行器当前位置，4个浮点型，依次是纬度、经度、海拔高度、相对高度
    aircraft_roll      Double      否       飞机 Roll，单位度
    aircraft_pitch     Double      否       飞机 Pitch，单位度
    aircraft_yaw       Double      否       飞机 Yaw，单位度
    satellite_number   Int         否       GPS 卫星数
    gps_fix_type       String      否       定位精度，"No GPS","No Fix","Fix 2D","Fix 3D"(从这个开始，已经完成定位),"Fix Dgps","Rtk Float","Rtk Fixed"
    aircraft_speed     Double[]    否       飞机飞行速度，3个浮点型，依次是 X,Y,Z 轴，单位 m/s
    battery_percent    Double      否       飞机电池电量（0.0～1.0）
    current_plan       String      能       当前执行的航线名称(Mission飞行模式下)
    camera_model       String      能       相机型号（唯一）
    gimbal_roll        Double      能       云台 Roll，单位度
    gimbal_pitch       Double      能       云台 Pitch，单位度
    gimbal_yaw         Double      能       云台 Yaw，单位度
    gimbal_yaw_abs     Double      能       云台 Yaw 绝对角度，单位度
    zoom_level         Double      能       相机变焦倍数
    has_stream         Bool        能       是否有视频流
    ================= =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "aircraft_id": "0600003633353833305117022024",
            "timestamp": 179525156,
            "landed_state": "On Ground",
            "flight_mode": "Posctl",
            "home": [
                23.173951,
                113.4198426,
                31.09400177,
                0
            ],
            "position": [
                23.1739512,
                113.4198423,
                30.76000214,
                -0.3340000212
            ],
            "aircraft_roll": -0.962998867,
            "aircraft_pitch": 0.8330261111,
            "aircraft_yaw": 9.299003601,
            "satellite_number": 10,
            "gps_fix_type": "Fix 3D",
            "aircraft_speed": [
                0.05,
                0.02,
                0.01
            ],
            "battery_percent": 100,
            "msg_type": 1
        }

.. _mqtt-mission-progress:

飞行器任务执行进度
-----------------------

终端事件发布
^^^^^^^^^^^^^^^
    ================= =========  ======== ===============================
    参数                类型       缺省      描述
    ================= =========  ======== ===============================
    msg_type           Int         否       :ref:`mqtt-msg-type`
    step               Int         否      0: 检查任务；1: 上传任务；2: 执行任务
    total              Int         否      当前步骤总进度
    sequence           Int         否      当前步骤进度
    ================= =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "step": 0,
            "total": 100,
            "sequence": 10,
            "msg_type": 2
        }

.. _mqtt-airport-status:

机库状态上报
-----------------------

终端发布
^^^^^^^^^^^^^^^
    ===================== =========  ======== ===============================
    参数                    类型       缺省      描述
    ===================== =========  ======== ===============================
    msg_type               Int         否       :ref:`mqtt-msg-type`
    rainfall               Float       否      当前降雨量，单位 mm
    wind_speed             Float       否      当前风速，单位 m/s
    wind_direction         Float       否      当前风向，单位度
    temperature            Float       否      当前机库内温度，单位摄氏度
    humidity               Float       否      当前机库内湿度，单位 %
    setting_temp           Float       否      当前机库空调设定温度
    pressure               Float       否      当前机库所在位置气压
    charge_voltage         Float       否      充电电压
    charge_current         Float       否      充电电流（Codev 无）
    charge_percent         Float       否      充电百分比（DJI 无）
    action_locked          Bool        否      机库是否锁定
    aircondition_running   Bool        否      空调是否运行
    plc_power              Bool        否      PLC设备是否打开供电
    radio_power            Bool        否      无线传输设备开关（Codev：图传&GPS；DJI：无效）
    ir_led                 Bool        否      降落灯开关（自动化开/关，无需控制）（Codev：精准降落信标；DJI：夜间灯；）
    coproc_on              Bool        否      协处理器设备开关机（一般用于DJI飞机：表示 MSDK 硬件设备是否上电）
    aircraft_charging      Bool        否      飞机是否在充电
    aircraft_fit           Bool        否      飞机是否固定住（DJI飞机：无效，不可用于逻辑判断，恒为 true）
    aircraft_on            Bool        否      飞机是否开机，仅在 aircraft_fit=true 时有效
    door_opening           Bool        否      舱门是否打开中
    door_closing           Bool        否      舱门是否关闭中
    door_opened            Bool        否      舱门是否打开的
    door_closed            Bool        否      舱门是否关闭的
    lift_uping             Bool        否      推举是否上升中
    lift_downing           Bool        否      推举是否下降中
    lift_up                Bool        否      推举是否在高位
    lift_down              Bool        否      推举是否在低位
    vertical_fixing        Bool        否      前后限位是否归中中
    vertical_releasing     Bool        否      前后限位是否打开中
    vertical_fixed         Bool        否      前后限位是否归中
    vertical_released      Bool        否      前后限位是否打开
    horizontal_fixing      Bool        否      左右限位是否归中中
    horizontal_releasing   Bool        否      左右限位是否打开中
    horizontal_fixed       Bool        否      左右限位是否归中
    horizontal_released    Bool        否      左右限位是否打开
    combinations_running   Bool        否      出库/入库组合动作是否正在运行
    fix_type               Int         是      定位精度，大于3完成基本定位，越大精度越高
    latitude               Float       是      机库 GPS 纬度
    longitude              Float       是      机库 GPS 经度
    altitude               Float       是      机库 GPS 高度
    ===================== =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "rainfall": 0.0,
            "wind_speed": 4.0,
            "wind_direction": 90,
            "temperature": 28.0,
            "humidity": 70.0,
            "setting_temp": 25.0,
            "pressure": 1001,
            "aircondition_running": true,
            "plc_power": false,
            "aircraft_charging": true,
            "aircraft_fit": true,
            "door_opening": false,
            "door_closing": false,
            "door_opened": true,
            "door_closed": false,
            "lift_uping": false,
            "lift_downing": false,
            "lift_up": true,
            "lift_down": false,
            "vertical_fixing": false,
            "vertical_releasing": false,
            "vertical_fixed": false,
            "vertical_released": true,
            "horizontal_fixing": false,
            "horizontal_releasing": false,
            "horizontal_fixed": false,
            "horizontal_released": true,
            "combinations_running": false
        }

.. _mqtt-schedule-status:

联动任务状态
-----------------------

终端发布
^^^^^^^^^^^^^^^
    ================= =========  ======== ===============================
    参数                类型       缺省      描述
    ================= =========  ======== ===============================
    msg_type           Int         否       :ref:`mqtt-msg-type`
    running            Bool        否      是否在执行联动任务
    total_executed     Int         否      已经执行的联动任务次数
    current_job        String      否      当前联动类型（唯一）,"Mission", "GotoLocation", "Recovery", "AccurateLand"其中之一
    json_params        String      否      联动任务的参数，json 格式
    rtl_in_idle        String      否      飞行器返航将会自动触发的联动任务, "Recovery", "AccurateLand"其中之一, 空为无触发联动任务
    ================= =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 4,
            "running": true,
            "total_executed": 20,
            "current_job": "Mission",
            "json_params": "{\"name\": \"test.plan\"}",
            "rtl_in_idle": ""
        }

.. _mqtt-disconnected-telemetry:

飞行器断连事件包
-----------------------
    *飞行器断联之后会触发一次，无需清除，记录着飞行器最后一帧数据信息*

终端发布
^^^^^^^^^^^^^^^
    ================= =========  ======== ===============================
    参数                类型       缺省      描述
    ================= =========  ======== ===============================
    msg_type           Int         否       :ref:`mqtt-msg-type`
    aircraft_id        String      否       飞行器 UUID
    timestamp          Long        否       UTC 时间
    landed_state       String      否       "On Ground","In Air","Taking Off","Landing"
    flight_mode        String      否       "Ready"(可以起飞),"Takeoff","Hold","Mission","Return To Launch","Land","Posctl"
    home               Double[]    否       Home 点，4个浮点型，依次是纬度、经度、海拔高度、相对高度
    position           Double[]    否       飞行器当前位置，4个浮点型，依次是纬度、经度、海拔高度、相对高度
    aircraft_roll      Double      否       飞机 Roll，单位度
    aircraft_pitch     Double      否       飞机 Pitch，单位度
    aircraft_yaw       Double      否       飞机 Yaw，单位度
    satellite_number   Int         否       GPS 卫星数
    gps_fix_type       String      否       定位精度，"No GPS","No Fix","Fix 2D","Fix 3D"(从这个开始，已经完成定位),"Fix Dgps","Rtk Float","Rtk Fixed"
    aircraft_speed     Double[]    否       飞机飞行速度，3个浮点型，依次是 X,Y,Z 轴，单位 m/s
    battery_percent    Double      否       飞机电池电量（0.0～1.0）
    datetime           String      否       事件发生的日期和时间
    ================= =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "aircraft_id": "0600003633353833305117022024",
            "timestamp": 179525156,
            "landed_state": "On Ground",
            "flight_mode": "Posctl",
            "home": [
                23.173951,
                113.4198426,
                31.09400177,
                0
            ],
            "position": [
                23.1739512,
                113.4198423,
                30.76000214,
                -0.3340000212
            ],
            "aircraft_roll": -0.962998867,
            "aircraft_pitch": 0.8330261111,
            "aircraft_yaw": 9.299003601,
            "satellite_number": 10,
            "gps_fix_type": "Fix 3D",
            "aircraft_speed": [
                0.05,
                0.02,
                0.01
            ],
            "battery_percent": 100,
            "datetime": "2020-07-20 15:22:00",
            "msg_type": 5
        }

.. _mqtt-airport-online:

设备上线事件
-----------------------
    *设备连接上之后自动发送, 5s一次的频率, 需要清除, 如不清除将会一直发送*

终端发布
^^^^^^^^^^^^^^^
    ================= =========  ======== ===============================
    参数                类型       缺省      描述
    ================= =========  ======== ===============================
    msg_type           Int         否       :ref:`mqtt-msg-type`
    datetime           String      否      事件发生的日期和时间
    id                 String      否      唯一序列号
    model              String      否      型号（Codev：A300、ARS300; DJI: AD3、ARS350）
    version            String      否      API 版本号
    ================= =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 6,
            "datetime": "2020-07-20 15:22:00",
            "id": "0242AC110002",
            "model": "A300",
            "version": "1.0.0-1.1.1-1.2.1"
        }

服务器清除事件
^^^^^^^^^^^^^^^
    ================= =========  ======== ===============================
    参数                类型       缺省      描述
    ================= =========  ======== ===============================
    msg_type           Int         否       :ref:`mqtt-msg-type`
    ================= =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 6
        }

.. _mqtt-upload-finished:

上传任务照片完成事件
-----------------------
    *设备完成上传之后自动发送, 15s一次的频率, 需要清除, 如不清除将会在 10 分钟后自动清除, 重新开始新的上传也会清除*

终端发布
^^^^^^^^^^^^^^^
    ===================== =========  ======== ===============================
    参数                  类型        缺省      描述
    ===================== =========  ======== ===============================
    msg_type               Int       否        :ref:`mqtt-msg-type`
    datetime               String    否        事件发生的日期和时间
    destination            String    否        上传的目的 URL(目录或文件)
    download_total         Int       否        已下载的总文件数（包含错误的）
    download_error_count   Int       否        下载文件的错误数
    upload_total           Int       否        已上传的总文件数（包含错误的）
    upload_error_count     Int       否        上传文件的错误数   
    ===================== =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 7,
            "datetime": "2020-07-20 15:22:00",
            "destination": "http://192.168.1.98/upload",
            "download_total": 20,
            "download_error_count": 0,
            "upload_total": 20,
            "upload_error_count": 0
        }

服务器清除事件
^^^^^^^^^^^^^^^
    ================= =========  ======== ===============================
    参数                类型       缺省      描述
    ================= =========  ======== ===============================
    msg_type           Int         否       :ref:`mqtt-msg-type`
    ================= =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 7
        }

.. _mqtt-mission-finished:

机库与飞机联动任务完成事件
------------------------------------
    *设备完成任务之后自动发送, 15s一次的频率, 需要清除, 如不清除将会在 10 分钟后自动清除, 重新开始新的机库与飞机联动任务也会清除*

终端发布
^^^^^^^^^^^^^^^
    ===================== =========  ======== ===============================
    参数                  类型        缺省      描述
    ===================== =========  ======== ===============================
    msg_type               Int       否        :ref:`mqtt-msg-type`
    datetime               String    否        事件发生的日期和时间
    success                Bool      否        任务流程是否正确完成
    error_message          String    是        当 success 为 false 时，会返回错误信息
    ===================== =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 8,
            "datetime": "2020-07-20 15:22:00",
            "success": false,
            "error_message": "'Camera' is disconnected!"
        }

服务器清除事件
^^^^^^^^^^^^^^^
    ================= =========  ======== ===============================
    参数                类型       缺省      描述
    ================= =========  ======== ===============================
    msg_type           Int         否       :ref:`mqtt-msg-type`
    ================= =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 8
        }

.. _mqtt-log-text:

机库日志消息事件
------------------------------------
    *来自机库的日志消息事件，用于调试分析问题，无需取消*

终端发布
^^^^^^^^^^^^^^^
    ===================== =========  ======== ===============================
    参数                  类型        缺省      描述
    ===================== =========  ======== ===============================
    msg_type               Int       否        :ref:`mqtt-msg-type`
    datetime               String    否        事件发生的日期和时间
    level                  Int       否        日志级别：0:info, 1:warn, 2:error
    package                String    否        进程代号
    message                String    否        日志信息
    ===================== =========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 8,
            "datetime": "2020-07-20 15:22:00",
            "level": 2,
            "package": "schedule",
            "message": "'Camera' is disconnected!"
        }

.. _mqtt-arm:

飞行器解锁（不解锁飞机将不会有任何动作）
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1000
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    armed         Bool      `true`: 解锁，`false`: 上锁
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "armed": true,
            "msg_type": 1000
        }

.. _mqtt-takeoff:

飞行器切换起飞模式
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1001
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1001
        }

.. _mqtt-land:

飞行器切换降落模式
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1002
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1002
        }

.. _mqtt-rtl:

飞行器切换返航模式
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1003
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1003
        }

.. _mqtt-hold:

飞行器切换悬停模式
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1004
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1004
        }

.. _mqtt-posctl:

飞行器切换位置模式
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1005
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1005
        }

.. _mqtt-goto-location:

飞行器到达指定点悬停
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1006
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    latitude      Double    目标纬度
    longitude     Double    目标经度
    altitude      Double    目标高度（相对高度）
    yaw           Double    飞机机头朝向
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "latitude": 31.12,
            "longitude": 120.12,
            "altitude": 50,
            "yaw": 66.8,
            "msg_type": 1006
        }

.. _mqtt-takephoto:

相机拍照
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1007
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1007
        }

.. _mqtt-start-video:

相机开始录像
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1008
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1008
        }

.. _mqtt-stop-video:

相机停止录像
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1009
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1009
        }

.. _mqtt-start-mission:

飞行器开始执行任务
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1010
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    name          String    需要执行的任务文件名称
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "name": "test.mission",
            "msg_type": 1010
        }

.. _mqtt-cancel-mission:

飞行器取消当前任务（触发返航）
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1011
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1011
        }

.. _mqtt-continue-mission:

飞行器继续当前任务（开始任务之后该命令有效）
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1012
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1012
        }

.. _mqtt-push-rtmp-video-stream:

设置推送飞行器的码流到指定地址
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1013
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    url           String    RTMP 推送地址
    id            Int       多路码流时需要指定id，可不填
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1013,
            "url": "rtmp://127.0.0.1:1234"
        }

.. _mqtt-set-zoom:

设置相机变倍倍数
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1014
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    type          Int       变焦类型(1: 连续变焦, 2: 定倍变焦)
    value         Float     变焦值(连续变焦时为方向-1/0/1, 定倍变焦时为定倍值)
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1014,
            "type": 1,
            "value": 8.5
        }

.. _mqtt-aircraft-on:

开关飞机
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1015
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    on            Bool      false：关，true：开
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1015,
            "on": true
        }

.. _mqtt-push-rtmp-ip-camera:

设置推送机库的码流到指定地址
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1016
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    url           String    RTMP 推送地址
    id            Int       多路码流时需要指定id，可不填
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1016,
            "url": "rtmp://127.0.0.1:1234"
        }

.. _mqtt-aircraft-charge:

飞机充电开关
----------------------------------------------
    *Codev飞机自动充电，目前无法开关*

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1017
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    on            Bool      false：关，true：开
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1017,
            "on": true
        }

.. _mqtt-radio-power:

无线传输设备（遥控器）开关机
----------------------------------------------
    *用于Codev飞机：图传&GPS，有反馈，机库状态上报中的字段‘radio_power’有效。 用于DJI飞机：遥控器，无反馈，机库状态上报中的字段‘radio_power’无效*
    *故，当使用DJI飞机时，‘on’ 传入参数无效。*

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1018
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    on            Bool      false：关，true：开
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1018,
            "on": true
        }

.. _mqtt-coproc-on:

协处理器设备开关机
----------------------------------------------
    *一般用于DJI飞机：用于开关 MSDK 硬件设备。*

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1019
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    on            Bool      false：关，true：开
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1019,
            "on": true
        }

.. _mqtt-action-lock:

锁定/解锁机库
----------------------------------------------
    *锁定机库后，机库可动机械将被锁定，不可动。*

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1020
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    on            Bool      false：解锁，true：锁定
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1020,
            "on": true
        }

.. _mqtt-get-aircraft-param:

获得飞机参数值
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    values       DoubleList 对应参数值列表，类型只有整数与浮点数
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1192
            "values": [1, 2000, 1]
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    names        StringList 参数名称列表
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1192,
            "names": ["CAM_MODE","CAM_ISO","CAM_WBMODE"]
        }

.. _mqtt-set-aircraft-param:

设置飞机参数值
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    reason       String     失败原因，成功没有该字段
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1193
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    names        StringList 参数名称列表
    values       DoubleList 对应参数值列表，类型只有整数与浮点数
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1193,
            "names": ["CAM_MODE","CAM_ISO","CAM_WBMODE"],
            "values": [1, 2000, 1]
        }

.. _mqtt-list-aircraft-param:

获得飞机参数列表
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    names        StringList 参数名称列表
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1194
            "names": ["CAM_MODE","CAM_ISO","CAM_WBMODE"]
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1194
        }

.. _mqtt-describe-aircraft-param:

获得飞机参数类型与范围信息
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ============ ========== ===============================
    参数          类型       描述
    ============ ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    descriptors  ObjectList :ref:`mqtt-param-object`
    ============ ========== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1195
            "descriptors": [
                {
                    "name": "CAM_WBMODE",
                    "type": "Int",
                    "description": "Camera white balance mode",
                    "enumStrings": ["Auto", "Manual"],
                    "enumValues": [0, 1]
                },
                {
                    "name": "CAM_ZOOM_SPEED",
                    "type": "Int",
                    "description": "Camera zoom speed",
                    "min": 1,
                    "max": 10,
                    "step": 1
                }
            ]
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    names        StringList 参数名称列表
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1195,
            "names": ["CAM_WBMODE","CAM_ZOOM_SPEED"]
        }

.. _mqtt-get-camera-param:

获得相机参数值
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    values       DoubleList 对应参数值列表，类型只有整数与浮点数
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1196
            "values": [1, 2000, 1]
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    names        StringList 参数名称列表
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1196,
            "names": ["CAM_MODE","CAM_ISO","CAM_WBMODE"]
        }

.. _mqtt-set-camera-param:

设置相机参数值
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    reason       String     失败原因，成功没有该字段
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1197
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    names        StringList 参数名称列表
    values       DoubleList 对应参数值列表，类型只有整数与浮点数
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1197,
            "names": ["CAM_MODE","CAM_ISO","CAM_WBMODE"],
            "values": [1, 2000, 1]
        }

.. _mqtt-list-camera-param:

获得相机参数列表
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    names        StringList 参数名称列表
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1198
            "names": ["CAM_MODE","CAM_ISO","CAM_WBMODE"]
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1198
        }

.. _mqtt-describe-camera-param:

获得相机参数类型与范围信息
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ============ ========== ===============================
    参数          类型       描述
    ============ ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    descriptors  ObjectList :ref:`mqtt-param-object`
    ============ ========== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1199
            "descriptors": [
                {
                    "name": "CAM_WBMODE",
                    "type": "Int",
                    "description": "Camera white balance mode",
                    "enumStrings": ["Auto", "Manual"],
                    "enumValues": [0, 1]
                },
                {
                    "name": "CAM_ZOOM_SPEED",
                    "type": "Int",
                    "description": "Camera zoom speed",
                    "min": 1,
                    "max": 10,
                    "step": 1
                }
            ]
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ========== ===============================
    参数          类型       描述
    ===========  ========== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    names        StringList 参数名称列表
    ===========  ========== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1199,
            "names": ["CAM_WBMODE","CAM_ZOOM_SPEED"]
        }

.. _mqtt-airport-door:

机库舱门控制
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1200
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    open          Bool      true：开舱门；false：关舱门
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1200,
            "open": true
        }

.. _mqtt-stop-airport-door:

取消舱门动作
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1201
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1201
        }

.. _mqtt-airport-lift:

机库推举控制
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1202
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    up            Bool      true：升推举；false：降推举
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1202,
            "up": true
        }

.. _mqtt-stop-airport-lift:

取消推举动作
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1203
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1203
        }

.. _mqtt-airport-vertical:

机库前后限位控制
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1204
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    fix           Bool      true：归中；false：释放
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1204,
            "fix": true
        }

.. _mqtt-stop-airport-vertical:

取消前后限位动作
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1205
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1205
        }

.. _mqtt-airport-horizontal:

机库左右限位控制
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1206
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    fix           Bool      true：归中；false：释放
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1206,
            "fix": true
        }

.. _mqtt-stop-airport-horizontal:

取消左右限位动作
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1207
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1207
        }

.. _mqtt-airport-outbound:

机库出库控制
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1296
        }

服务端发布
^^^^^^^^^^^^^^^

    ============= ======== ===============================
    参数          类型       描述
    ============= ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    has_aircraft  Bool      是否控制开/关机，可不填，默认为True
    ============= ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1296,
            "has_aircraft": false
        }

.. _mqtt-stop-airport-outbound:

取消出库动作
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1297
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1297
        }

.. _mqtt-airport-inbound:

机库入库控制
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1298
        }

服务端发布
^^^^^^^^^^^^^^^

    ============= ======== ===============================
    参数          类型       描述
    ============= ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    has_aircraft  Bool      是否控制开/关机，可不填，默认为True
    ============= ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1298,
            "has_aircraft": false
        }

.. _mqtt-stop-airport-inbound:

取消入库动作
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1299
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1299
        }

.. _mqtt-kill-schedule:

终止飞机与机库联动计划
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1300
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1300
        }

.. _mqtt-schedule-mission:

机库与飞机联动完成一次完整的任务
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1301
        }

服务端发布
^^^^^^^^^^^^^^^
    *upload_url, access_key, secret_key, 可不填, 填入正确值之后会触发完成一次任务后自动上传任务照片到指定的 URL, 并且会在执行任务前会格式化相机存储卡, 如果不填, 则不会上传任务文件*

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    name          String    需要执行的任务文件名称
    upload_url    String    上传任务文件到服务器的URL
    access_key    String    上传任务文件到服务器的AccessKey
    secret_key    String    上传任务文件到服务器的SecretKey
    protocol      String    上传任务文件到服务器的协议 BasicHttp、MinIOS3, 可不填, 默认为 MinIOS3
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "name": "test.mission",
            "msg_type": 1301,
            "upload_url": "http://127.0.0.1:9000/bucket",
            "access_key": "1234567890",
            "secret_key": "1234567890"
        }

.. _mqtt-schedule-recovery:

机库与飞机联动完成一次回收
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1302
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1302
        }

.. _mqtt-schedule-goto-location:

机库与飞机联动完成出库并飞行至指定点
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1304
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    latitude      Double    目标纬度
    longitude     Double    目标经度
    altitude      Double    目标高度（相对高度）
    yaw           Double    飞机机头朝向
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "latitude": 31.12,
            "longitude": 120.12,
            "altitude": 50,
            "yaw": 66.8,
            "msg_type": 1304
        }

.. _mqtt-schedule-upload:

上传相机中的照片到指定服务器
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1305
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    upload_url    String    上传任务文件到服务器的URL
    access_key    String    上传任务文件到服务器的AccessKey
    secret_key    String    上传任务文件到服务器的SecretKey
    protocol      String    上传任务文件到服务器的协议 BasicHttp、MinIOS3, 可不填, 默认为 MinIOS3
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1305,
            "upload_url": "http://127.0.0.1:9000/bucket",
            "access_key": "1234567890",
            "secret_key": "1234567890",
        }

.. _mqtt-schedule-rtl-in-idle:

设置飞行器返航自动触发联动任务
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "msg_type": 1399
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    job          String    飞行器返航后需要触发的联动任务，目前仅有两个："Recovery"-回收 "AccurateLand"-精准降落，置空为不触发
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "job": "Recovery"
            "msg_type": 1399
        }

.. _mqtt-get-mission-file-content:

获得指定任务文件的内容
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ============= ========== ===============================
    参数           类型       描述
    ============= ========== ===============================
    msg_type       Int       :ref:`mqtt-msg-type`
    result         Int       :ref:`mqtt-result`
    filename       String    任务文件名
    missionItems   Object[]  :ref:`mqtt-mission-object`
    errorMessage   String    错误信息，仅在错误时出现
    ============= ========== ===============================

    **2023年12月起之后的版本同时支持plan和mission格式查看, plan将会转译成mission格式返回，但是mission格式功能有限，不一定可以转换成功**

例子
""""""""""""
    ::

        {
            "result": 1,
            "filename": "test.mission"
            "missionItems": [
                {
                    "latitude": 32.111,
                    "longitude": 120.111,
                    "altitude_rel": 82.6,
                    "vehicle_action": 1
                },
                {
                    "latitude": 32.111,
                    "longitude": 120.112,
                    "altitude_rel": 82.6,
                    "vehicle_action": 0,
                    "speed": 5.0,
                    "is_fly_through": true
                },
                {
                    "latitude": 32.111,
                    "longitude": 120.113,
                    "altitude_rel": 82.6,
                    "camera_action": 0,
                    "gimbal_pitch": 10.0,
                    "gimbal_yaw": 45.0,
                    "is_fly_through": false
                }
            ],
            "msg_type": 1496
        }

服务端发布
^^^^^^^^^^^^^^^

    =============  ======== ===============================
    参数            类型       描述
    =============  ======== ===============================
    msg_type       Int       :ref:`mqtt-msg-type`
    name           String    任务文件的名字
    =============  ======== ===============================

例子
""""""""""""
    ::

        {
            "name": "test.mission",
            "msg_type": 1496
        }

.. _mqtt-delete-mission-file:

删除飞行器上的任务
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    filename      String    已经删除的任务文件的名称
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "filename": "test.mission"
            "msg_type": 1497
        }

服务端发布
^^^^^^^^^^^^^^^

    =============  ======== ===============================
    参数            类型       描述
    =============  ======== ===============================
    msg_type       Int       :ref:`mqtt-msg-type`
    name           String    任务文件的名字
    =============  ======== ===============================

例子
""""""""""""
    ::

        {
            "name": "test.mission",
            "msg_type": 1497
        }

.. _mqtt-upload-mission-file:

上传任务到飞行器
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    filename      String    返回实际创建任务文件的名称
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "filename": "test_1.mission"
            "msg_type": 1498
        }

服务端发布
^^^^^^^^^^^^^^^

    =============  ======== ===============================
    参数            类型       描述
    =============  ======== ===============================
    msg_type       Int       :ref:`mqtt-msg-type`
    name           String    期望任务文件的名字
    missionItems   Object[]  :ref:`mqtt-mission-object`
    overw          Bool      是否覆盖，如果文件名相同，否则将加入'_%d'后缀，缺省值为 False
    =============  ======== ===============================

例子
""""""""""""
    ::

        {
            "name": "test.mission",
            "missionItems": [
                {
                    "latitude": 32.111,
                    "longitude": 120.111,
                    "altitude_rel": 82.6,
                    "vehicle_action": 1
                },
                {
                    "latitude": 32.111,
                    "longitude": 120.112,
                    "altitude_rel": 82.6,
                    "vehicle_action": 0,
                    "speed": 5.0,
                    "is_fly_through": true
                },
                {
                    "latitude": 32.111,
                    "longitude": 120.113,
                    "altitude_rel": 82.6,
                    "camera_action": 0,
                    "gimbal_pitch": 10.0,
                    "gimbal_yaw": 45.0,
                    "is_fly_through": false
                }
            ],
            "msg_type": 1498
        }

.. _mqtt-request-mission-list:

请求飞行器上的航点列表
----------------------------------------------

终端应答
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    result        Int       :ref:`mqtt-result`
    plans        String[]   航点文件列表
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "result": 1,
            "plans": ["test.mission","12.plan"]
            "msg_type": 1499
        }

服务端发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "msg_type": 1499
        }

.. _mqtt-manual-control:

飞行器手动控制包
----------------------------------------------

服务端控制发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    x             Double    飞行器前后控制（-1.0~1.0）
    y             Double    飞行器左右控制（-1.0~1.0）
    z             Double    飞行器上下控制（-1.0~1.0）
    r             Double    飞行器旋转（-1.0~1.0）
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "x": 0.0,
            "y": 0.0,
            "z": 0.0,
            "r": 0.5,
            "msg_type": 1500
        }

.. _mqtt-gimbal-manual-control:

云台角度控制
----------------------------------------------

服务端控制发布
^^^^^^^^^^^^^^^

    ===========  ======== ===============================
    参数          类型       描述
    ===========  ======== ===============================
    msg_type      Int       :ref:`mqtt-msg-type`
    pitch         Double    云台 Pitch，单位度(可不填)
    yaw           Double    云台 Yaw，单位度(可不填)
    pitch_rate    Double    云台 Pitch 速率，单位度/秒(可不填)
    yaw_rate      Double    云台 Yaw 速率，单位度/秒(可不填)
    ===========  ======== ===============================

例子
""""""""""""
    ::

        {
            "yaw": 45.0,
            "msg_type": 1501
        }
