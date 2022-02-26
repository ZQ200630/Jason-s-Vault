---
tags: ROS C++
---
# Launch启动文件使用

## Launch文件

### 节点

```xml
<launch>
	<node pkg="package-name" name="executable-name" type="node-name"/>
</launch>
<-- 可选属性-> output respawn required ns args -->
```

### 参数

```xml
<launch>
	<param name="key" value="value"/>
	<rosparam file="param.yaml" command="load" ns="params" />
	<arg name="arg-name" default="arg-value" />
</launch>

<-- param 与 rosparam 是供参数服务器使用的，arg可以在xml其他地方用 $(arg arg-name) 调用 ->
```

### 重映射

```xml
<remap from="/turtlebot/cmd_vel"to="/cmd_vel">
```

### 嵌套

```xml
<include file="path"/>
```