---
tags: ROS C++
---
# 发布者Publisher

## 创建功能包

```bash
catkin_create_pkg learning_topic roscpp rospy std_msgs geometry_msgs turtlesim
```

## 发布者流程

- 初始化ROS节点
- 向ROS Master 注册节点信息，包括发布的话题名和话题中的消息类型
- 创建消息数据
- 按照一定频率循环发布消息

## 例程

```bash
[[include]] <ros/ros.h>
[[include]] <geometry_msgs/Twist.h>

int main(int argc, char **argv)
{
	// ROS节点初始化
	ros::init(argc, argv, "velocity_publisher");

	// 创建节点句柄
	ros::NodeHandle n;

	// 创建一个Publisher，发布名为/turtle1/cmd_vel的topic，消息类型为geometry_msgs::Twist，队列长度10
	ros==Publisher turtle_vel_pub = n.advertise<geometry_msgs==Twist>("/turtle1/cmd_vel", 10);

	// 设置循环的频率
	ros::Rate loop_rate(10);

	int count = 0;
	while (ros::ok())
	{
	    // 初始化geometry_msgs::Twist类型的消息
		geometry_msgs::Twist vel_msg;
		vel_msg.linear.x = 0.5;
		vel_msg.angular.z = 0.2;

	    // 发布消息
		turtle_vel_pub.publish(vel_msg);
		ROS_INFO("Publsh turtle velocity command[%0.2f m/s, %0.2f rad/s]", 
				vel_msg.linear.x, vel_msg.angular.z);

	    // 按照循环频率延时
	    loop_rate.sleep();
	}

	return 0;
}
```

## CMakeLists

```bash
# 功能包文件夹下CMakeLists.txt
add_executable(velocity_publisher src/velocity_publisher.cpp)
target_link_libraries(velocity_publisher ${catkin_LIBRARIES})
```

## 编译

```bash
cd ~/catkin_ws
catkin_make

# 这一步每次进入工作空间都需要执行，否则环境变量中没有工作空间
source devel/setup.bash
roscore
rosrun tutlesim tutlesim_node
rosrun learning_topic velocity_publisher
```

# Python版

```bash
cd ~/catkin_ws/src/learning_topic
mkdir scripts
cd scripts

vim velocity_publisher.py

# -*- coding: utf-8 -*-
#!/usr/bin/env python
import rospy
from geometry_msgs.msg import Twist

def velocity_publisher():
	# ROS节点初始化
    rospy.init_node('velocity_publisher', anonymous=True)

	# 创建一个Publisher，发布名为/turtle1/cmd_vel的topic，消息类型为geometry_msgs::Twist，队列长度10
    turtle_vel_pub = rospy.Publisher('/turtle1/cmd_vel', Twist, queue_size=10)

	#设置循环的频率
    rate = rospy.Rate(10) 

    while not rospy.is_shutdown():
		# 初始化geometry_msgs::Twist类型的消息
        vel_msg = Twist()
        vel_msg.linear.x = 0.5
        vel_msg.angular.z = 0.2

		# 发布消息
        turtle_vel_pub.publish(vel_msg)
        rospy.loginfo("Publsh turtle velocity command[%0.2f m/s, %0.2f rad/s]", vel_msg.linear.x, vel_msg.angular.z)

		# 按照循环频率延时
        rate.sleep()

if __name__ == '__main__':
    try:
        velocity_publisher()
    except rospy.ROSInterruptException:
        pass

# 添加可执行权限
chmod +x velocity_publisher.py
```