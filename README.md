# 工作记录

## 3.20
- 可能问题：发现问题在oculus/oculus_robot.py中
  项目代码第 6 行写的是：
  from .oculus_reader import OculusReader  # 从同目录导入，但同目录没有那个文件
  但实际上应该是：
  from oculus_reader import OculusReader  # 从外部包导入
- [x] 完成NUC的环境配置
- [x] 遥操作
> 遥操作丝滑，但夹爪控制还有问题(可控制，但有延迟）

Todo：
- [ ] 修改夹爪控制


## 3.19
1.测试oculus_reader
> 网址：[oculus reader](https://github.com/rail-berkeley/oculus_reader)
- cd ~/oculus_reader
- python -m oculus_reader.reader
      <img width="847" height="555" alt="截图 2026-03-19 16-30-54" src="https://github.com/user-attachments/assets/4bbe04c6-3a15-4739-bbf0-5e07c8d5d08c" />
      电脑端显示数据，quest端自动进入apk界面
2. 尝试lerobot_franka_teleop
> 网址：[erobot_franka_teleop](https://github.com/YyYy0205/lerobot_franka_teleop)
- 创建conda 环境： franka_data
- clone: Franka Teleoperation Control
- 配置/连接 quest
- 编辑配置文件
- 测试：franka-record
> ERROR: from oculus_reader.FPS_counter import FPSCounter / ModuleNotFoundError: No module named 'oculus_reader'

## 3.17 
- [x] 打开quest2 开发者模式
- [x] 安装ADB
> [ meta developers ](https://developers.meta.com/horizon/documentation/native/android/mobile-device-setup/)
1. 创建meta账号
2. 激活meta开发者
3. 初始化quest
4. 手机app连接quest
5. 打开开发者模式
6. 安装 Oculus ADB 驱动
7. 安装 ADB
    > https://developers.meta.com/horizon/documentation/native/android/ts-adb/
    - cd C:\Users\31555\AppData\Local\Android\Sdk\platform-tools
    - 验证安装：adb help
    - USB连接后查找设备：adb devices
    - 通过 Wi-Fi 连接 ADB
    ```
    # 第一步：让ADB切换到TCP/IP模式，端口用默认的5555
    adb tcpip 5555
    # 第二步：通过Wi-Fi连接到头显（用你刚才获取的IP地址）
    adb connect 192.168.10.124:5555
    # 第三步：验证无线连接是否成功
    adb devices
    # 断开连接： adb disconnect
    ```


 

## 3.16 Monday
1. 电脑DROID环境配置
- 安装显卡驱动
- 安装ZED SDK
2. 编辑配置文件
  ` nano droid/misc/parameters.py`

## 3.14 Starday
1.帮助调试PICO

## 3.13 Friday
1. 编译 Franka
2. 编译 Polymetis

## 3.12 Thursday
### To-do List
- [x] 恢复NUC，安装: Ubuntu 18.04
- NUC 太新可能存在适配问题
- 帐号：robot 密码：0
- [x] 配置NUC
1. 配置wifi
2. Static IP Address 172.10.0.2
3. RT Patch of Kernel
4. 激活real time 内核
5. 编译内核
6. 激活实时内核
- [x] 禁用CPU频率缩放 performance模式


1. 修改Difussion训练
   - 减小 --num_workers=4
   - 限制 CPU 线程
   - 关闭 multiprocessing fork 问题（强烈推荐）

## 3.11 Wednesday
1. 帮助 FRANKA 测试
2. 阅读 DROID 网站
  - [Shoping list](https://droid-dataset.github.io/droid/hardware-setup/shopping-list.html)
  - [Assembly](https://droid-dataset.github.io/droid/hardware-setup/assembly.html)
  - [Running Application on Host](https://droid-dataset.github.io/droid/software-setup/host-installation.html)
3. 评估 DROID 方案
4. 制作 Shopping list
   > EXCEL: droid_shopping_list.xlsx

## 3.10 Tuesday
1. 安装 Openclaw
2. lerobot 训练 Difussion 策略

## 3.9 Monday
1. 测试 Franka
2. 测试 Pico

## 3.5
1. lerobot pi0
```
lerobot-teleoperate \
    --robot.type=so101_follower \
    --robot.port=/dev/ttyACM1 \
    --robot.id=my_awesome_follower_arm \
    --teleop.type=so101_leader \
    --teleop.port=/dev/ttyACM0 \
    --teleop.id=my_awesome_leader_arm
```
2. 采集
```
lerobot-record     --robot.type=so101_follower     --robot.port=/dev/ttyACM1     --robot.cameras='{
        "handeye": {
            "type": "opencv",
            "index_or_path": 0,
            "width": 640,
            "height": 480,
            "fps": 30
        },
        "front": {
            "type": "opencv",
            "index_or_path": 2,
            "width": 640,
            "height": 480,
            "fps": 30
        }
    }'     --teleop.type=so101_leader     --teleop.port=/dev/ttyACM0   --display_data=false     --dataset.repo_id=y/1     --dataset.num_episodes=20     --dataset.single_task="Put the black square into the white frame"     --dataset.push_to_hub=false

```
3. 训练


## 3.4
1. 配置采集数据环境和任务设置
> 从地上抓取盒子放到箱子上
2. 配置 Realsense 环境
> 验证摄像头，并确定安装位置

## 3.3
1. start the robot
> `ros2 launch franka_bringup franka.launch.py robot_type:=fr3 robot_ip:=172.16.0.2 use_rviz:=true`
2. Run different controllers for different robots
> `ros2 launch franka_bringup example.launch.py controller_names:="cartesian_elbow_example_controller,joint_impedance_example_controller,cartesian_velocity_example_controller"`
> > * Cartesian Elbow Example Controller: 笛卡尔-肘部联合控制器, 这是一个混合控制
> > * Joint Impedance Example Controller: 关节阻抗控制器, 柔性控制
> > * Cartesian Velocity Example Controller: 笛卡尔速度控制器, 工具坐标系下直接控制末端运动速度：直接发送速度指令：[vx, vy, vz, ωx, ωy, ωz]
3. 路径规划
> `ros2 launch franka_fr3_moveit_config moveit.launch.py robot_ip:=172.16.0.2 use_fake_hardware:=false`
>> plan --> Execute
4. joint_impedance_example_controller
> * `ros2 launch franka_bringup franka.launch.py robot_type:=fr3 robot_ip:=172.16.0.2 use_rviz:=true`
> > 报错,可忽略： [ros2_control_node-2] [ERROR] [1772526689.014038542] [franka_robot_state_broadcaster]: Failed to lock the realtime publisher after 5 attempts
> > [ros2_control_node-2] [ERROR] [1772526689.014141058] [controller_manager]: The update call of the following controller returned an error: 'franka_robot_state_broadcaster'
> * `ros2 control load_controller --set-state active joint_impedance_example_controller` 保持初始位置不变，后续会在第4和第5个关节上添加一个周期性的偏移
> * `ros2 control set_controller_state joint_impedance_example_controller inactive`
5. cartesian_pose_example_controller
这个控制器实现了一个简单的笛卡尔空间位置控制器。它通过计算期望的末端执行器位置来生成笛卡尔空间的命令。
> `ros2 control load_controller --set-state active cartesian_pose_example_controller` 
> `ros2 control set_controller_state cartesian_pose_example_controller inactive`

## 3.2
1. 配置xr_teleoperate 环境
> https://github.com/YyYy0205/xr_teleoperate/blob/main/README_zh-CN.md
2. 横装franka机械臂
