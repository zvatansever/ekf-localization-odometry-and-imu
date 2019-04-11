# ekf-localization-odometry-and-imu

First, perform system update/Upgrade: 
 apt-get update
 apt-get upgrade -y

Steps
1- Create Catkin Workspace:
  mkdir -p /home/workspace/catkin_ws/src
  cd /home/workspace/catkin_ws/src
  catkin_init_workspace
  cd ..
  catkin_make
  
2- Import TurtleBot Gazebo Package:
   Clone Package:
   cd /home/workspace/catkin_ws/src
   git clone https://github.com/turtlebot/turtlebot_simulator
   
   Install Dependencies:
   cd /home/workspace/catkin_ws
   source devel/setup.bash
   rosdep -i install turtlebot_gazebo
   
   Build Package:
   catkin_make
   source devel/setup.bash
   
   Launch Nodes:
   roslaunch turtlebot_gazebo turtlebot_world.launch
   
 3- Install EKF package: Documentation available on http://wiki.ros.org/robot_pose_ekf
 
   cd /home/workspace/catkin_ws/src/
   git clone https://github.com/udacity/robot_pose_ekf 
   
   Edit robot_pose_ekf.launch
   <launch>
   <node pkg="robot_pose_ekf" type="robot_pose_ekf" name="robot_pose_ekf">
   <param name="output_frame" value="odom_combined"/>
   <param name="base_footprint_frame" value="base_footprint"/>
   <param name="freq" value="30.0"/>
   <param name="sensor_timeout" value="1.0"/>  
   <param name="odom_used" value="true"/>
   <param name="imu_used" value="true"/>
   <param name="vo_used" value="false"/>

   <remap from="imu_data" to="/mobile_base/sensors/imu_data" />    
   </node>
   </launch>
   
   Now, build the package:
   cd /home/workspace/catkin_ws
   catkin_make
   source devel/setup.bash
  
   Launch the node:
   roslaunch robot_pose_ekf robot_pose_ekf.launch
   
 4-Install odom_to_trajectory: This package calculates the trajectory from odometer data.
 
   cd /home/workspace/catkin_ws/src
   git clone https://github.com/udacity/odom_to_trajectory
   
   Build the package:
   cd /home/workspace/catkin_ws
   catkin_make
   source devel/setup.bash
  
   Launch the nodes:
   roslaunch odom_to_trajectory create_trajectory.launch 
 
 5- Install Teleop package: This package helps controlling the TurtleBot
    cd /home/workspace/catkin_ws/src
    git clone https://github.com/turtlebot/turtlebot
    
    Install the Dependencies:
    cd /home/workspace/catkin_ws
    source devel/setup.bash
    rosdep -i install turtlebot_teleop
    
    Build the package:
    catkin_make
    source devel/setup.bash
    
    Launch the node:
    roslaunch turtlebot_teleop keyboard_teleop.launch
    
    
  6- Launch rviz:
    rosrun rviz rviz
    
    Edit the rviz configuration:
   - Change the Fixed Frame to base_footprint
   - Change the Reference Frame to odom
   - Add a RobotModel
   - Add a camera and select the /camera/rgb/image_raw topic
   - Add a /ekfpath topic and change the display name to EKFPath
   - Add a /odompath topic and change the display name to OdomPath
   - Change the OdomPath color to red:255;0;0
   - Save the rviz configuration:
   - Save the rviz configuration in /home/workspace/catkin_ws/src as EKFLab.rviz
   
   Create a RvizLaunch.launch file in the src folder
   Add the following to the RvizLaunch.launch file:
    <launch>
     <!--RVIZ-->
      <node pkg="rviz" type="rviz" name="rviz" args="-d /home/workspace/catkin_ws/src/EKFLab.rviz"/>
   </launch>
   
   Close the rviz terminal and
   Relaunch rviz:
   rosrun rviz rviz -d /home/workspace/catkin_ws/src/EKFLab.rviz
   
   
 7- Create a main file to open everything at the same time:
   cd /home/workspace/catkin_ws/src
   catkin_create_pkg main
   
   Build the package:
   cd /home/workspace/catkin_ws
   catkin_make 
   Create and edit the main.launch file:
   cd /home/workspace/catkin_ws/src/main
   mkdir launch
   cd launch    
   Download https://github.com/udacity/RoboND-EKFLab/blob/master/main/launch/main.launch
   
   Launch the main.launch file:
   
   cd /home/workspace/catkin_ws/
   source devel/setup.bash
   roslaunch main main.launch
   
   
  
