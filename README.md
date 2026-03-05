# 工作记录

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
