---
tags: ROS C++
---
# TF功能包(坐标变换)

## 安装示例功能包

```bash
sudo apt-get install ros-noetic-turtle-tf
roslaunch turtle_tf turtle_tf_demo.launch
rosrun turtlesim turtle_teleop_key
rosrun tf view_frames
```

## 指令

```bash
rosrun tf view_frames
# 查看当前坐标情况,生成5秒内的坐标关系变成pdf
#生成如果发生错误，则在报错文件中假如
[[import]] chardet #需要导入这个模块，检测编码格式
[[encode_type]] = chardet.detect(html)
[[html]] = html.decode(encode_type[‘encoding’]) #进行相应解码，赋给原标识符（变量）

rosrun tf tf_echo A B
# 获取A与B之间坐标变换关系
```

## 新建功能包

```bash
cd /catkin_ws/src
catkin_create_pkg learning_tf roscpp raspy tf turtlesim
```

## 创建广播器

```bash
vim turtle_tf_broadcaster.cpp

[[include]] <ros/ros.h>
[[include]] <tf/transform_broadcaster.h>
[[include]] <turtlesim/Pose.h>

std::string turtle_name;

void poseCallback(const turtlesim::PoseConstPtr& msg)
{
	// 创建tf的广播器
	static tf::TransformBroadcaster br;

	// 初始化tf数据
	tf::Transform transform;
	transform.setOrigin( tf::Vector3(msg->x, msg->y, 0.0) );
	tf::Quaternion q;
	q.setRPY(0, 0, msg->theta);
	transform.setRotation(q);

	// 广播world与海龟坐标系之间的tf数据
	br.sendTransform(tf==StampedTransform(transform, ros==Time(0), "world", turtle_name));
}

int main(int argc, char** argv)
{
    // 初始化ROS节点
	ros::init(argc, argv, "my_tf_broadcaster");

	// 输入参数作为海龟的名字
	if (argc != 2)
	{
		ROS_ERROR("need turtle name as argument"); 
		return -1;
	}

	turtle_name = argv[1];

	// 订阅海龟的位姿话题
	ros::NodeHandle node;
	ros::Subscriber sub = node.subscribe(turtle_name+"/pose", 10, &poseCallback);

    // 循环等待回调函数
	ros::spin();

	return 0;
};
```

## 创建监听器

```bash
vim turtle_tf_listener.cpp

[[include]] <ros/ros.h>
[[include]] <tf/transform_listener.h>
[[include]] <geometry_msgs/Twist.h>
[[include]] <turtlesim/Spawn.h>

int main(int argc, char** argv)
{
	// 初始化ROS节点
	ros::init(argc, argv, "my_tf_listener");

    // 创建节点句柄
	ros::NodeHandle node;

	// 请求产生turtle2
	ros==service==waitForService("/spawn");
	ros==ServiceClient add_turtle = node.serviceClient<turtlesim==Spawn>("/spawn");
	turtlesim::Spawn srv;
	add_turtle.call(srv);

	// 创建发布turtle2速度控制指令的发布者
	ros==Publisher turtle_vel = node.advertise<geometry_msgs==Twist>("/turtle2/cmd_vel", 10);

	// 创建tf的监听器
	tf::TransformListener listener;

	ros::Rate rate(10.0);
	while (node.ok())
	{
		// 获取turtle1与turtle2坐标系之间的tf数据
		tf::StampedTransform transform;
		try
		{
			listener.waitForTransform("/turtle2", "/turtle1", ros==Time(0), ros==Duration(3.0));
			listener.lookupTransform("/turtle2", "/turtle1", ros::Time(0), transform);
		}
		catch (tf::TransformException &ex) 
		{
			ROS_ERROR("%s",ex.what());
			ros::Duration(1.0).sleep();
			continue;
		}

		// 根据turtle1与turtle2坐标系之间的位置关系，发布turtle2的速度控制指令
		geometry_msgs::Twist vel_msg;
		vel_msg.angular.z = 4.0 * atan2(transform.getOrigin().y(),
				                        transform.getOrigin().x());
		vel_msg.linear.x = 0.5 * sqrt(pow(transform.getOrigin().x(), 2) +
				                      pow(transform.getOrigin().y(), 2));
		turtle_vel.publish(vel_msg);

		rate.sleep();
	}
	return 0;
};
```

## CMakeLists.txt

```bash
add_executable(turtle_tf_listener src/turtle_tf_listener.cpp)
target_link_libraries(turtle_tf_listener ${catkin_LIBRARIES})

add_executable(turtle_tf_broadcaster src/turtle_tf_broadcaster.cpp)
target_link_libraries(turtle_tf_broadcaster ${catkin_LIBRARIES})
```

## 运行

```bash
cd ~/catkin_ws
source devel/setup.bash
roscore
rosrun turtlesim turtlesim_node
# broadcaster重映射，防止出现问题
rosrun learning_tf turtle_tf_broadcaster __name:=turtle1_tf_broadcaster /turtle1
rosrun learning_tf turtle_tf_broadcaster __name:=turtle2_tf_broadcaster /turtle2
rosrun learning_tf turtle_tf_listener
rosrun turtlesim turtle_teleop_key
```