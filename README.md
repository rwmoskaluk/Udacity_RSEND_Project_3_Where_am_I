# Udacity_RSEND_Project_3_Where_am_I

[image1]: ./pictures/libprotobuf_error_fix.png "Libprotobuf Error Fix"
[image2]: ./pictures/rviz_fix_frame.png "Rviz fixed frame"
[image3]: ./pictures/rviz_robotmodel.png "Rviz robotmodel"
[image4]: ./pictures/rviz_map.png "Rviz map"
[image5]: ./pictures/rviz_localization_map.png "Rviz localization map"
[image6]: ./pictures/Robot_screenshot.png "Robot screenshot Gazebo"
[image7]: ./pictures/Localization_start.png "Localization screenshot rviz start"
[image8]: ./pictures/Localization_end.png "Localization screenshot rviz end"




# How to run:
Navigate to /catkin_ws/ directory
```
catkin_make
source devel/setup.bash
roslaunch my_robot world.launch
```
### Option #1 2D nav goal in rviz
In a different terminal launch the diy_acml package
```
cd catkin_ws/
source devel/setup.bash
roslaunch diy_amcl amcl.launch
```

In rviz then command the robot with 2d nav goals

### Option #2 Teleop control package

Using the Teleop package one can drive the robot around instead of placing 2d nav goals.

#### Installing ROS Teleop Package
```
cd catkin_ws/src
git clone https://github.com/ros-teleop/teleop_twist_keyboard
cd ..
catkin_make
source devel/setup.bash
```
#### Running the Teleop package
```
rosrun teleop_twist_keyboard teleop_twist_keyboard.py
```

## Visualizing the environment
Add visualizations in rviz after launching

`Switch frame to odom`
![alt text][image2]


<br>Add in the Robot</br>
```
Select Add
Select RobotModel
```
![alt text][image3]

<br>Add in the map</br>
```
Select Add
Select Map
```
![alt text][image4]

<br>Add a 2d nav goal</br>

![alt text][image5]



# Example running:

<img src="pictures/Localization_start.gif?raw=true" width="720px">
<img src="pictures/Localization_end.gif?raw=true" width="720px">

![alt text][image6]
![alt text][image7]
![alt text][image8]



# Learnings:

When configuring the ACML package make sure to take notice of what the initial pose (`initial_pose_x`, `initial_pose_y`, `initial_pose_a`) values are set to.  This can have a drastic outcome for localization with the map.

Another note, the `odom_alpha` values can be lower to help speed up the localization time since we are just in simulation there won't be real noise in their readings.
# Notes:


For Ubuntu 18.04 LTS ROS Melodic when using pgm_map_creator package, utilize the branch (`dudasdavid:libprotobuf-fix`) that fixes the libprotobuf compiling problem along with allowing pgm_map_creator to run in Melodic.  As of 9/24/2020 the branch has not merged with master yet.  Changes shown below.

![alt text][image1]



Manually installed packages to support melodic-devel branch for navigation stack

```
  Could NOT find SDL (missing: SDL_LIBRARY SDL_INCLUDE_DIR)
Call Stack (most recent call first):
  /usr/share/cmake-3.13/Modules/FindPackageHandleStandardArgs.cmake:378 (_FPHSA_FAILURE_MESSAGE)
  /usr/share/cmake-3.13/Modules/FindSDL.cmake:190 (FIND_PACKAGE_HANDLE_STANDARD_ARGS)
  navigation/map_server/CMakeLists.txt:12 (find_package)

```
```
	sudo apt-get install libsdl-image1.2-dev
	sudo apt-get install libsdl-dev
```

```
Could not find a package configuration file provided by "tf2_sensor_msgs"
  with any of the following names:

    tf2_sensor_msgsConfig.cmake
    tf2_sensor_msgs-config.cmake

  Add the installation prefix of "tf2_sensor_msgs" to CMAKE_PREFIX_PATH or
  set "tf2_sensor_msgs_DIR" to a directory containing one of the above files.
  If "tf2_sensor_msgs" provides a separate development package or SDK, be
  sure it has been installed.
Call Stack (most recent call first):
  navigation/costmap_2d/CMakeLists.txt:4 (find_package)
```

```
	sudo apt-get install ros-melodic-tf2-sensor-msgs
```


```
Could not find a package configuration file provided by "move_base_msgs"
  with any of the following names:

    move_base_msgsConfig.cmake
    move_base_msgs-config.cmake

  Add the installation prefix of "move_base_msgs" to CMAKE_PREFIX_PATH or set
  "move_base_msgs_DIR" to a directory containing one of the above files.  If
  "move_base_msgs" provides a separate development package or SDK, be sure it
  has been installed.
Call Stack (most recent call first):
  navigation/move_base/CMakeLists.txt:4 (find_package)

```

```
	sudo apt-get install ros-melodic-move-base-msgs
```


```
static double amcl::AMCLLaser::LikelihoodFieldModel(amcl::AMCLLaserData*, pf_sample_set_t*): Assertion `pz <= 1.0' failed.

```

```$xslt
When using either model for the laser scanner the weights need to add up to 1 otherwise this error is thrown
https://github.com/ros-planning/navigation/issues/824
```

Was initially getting update loop messages about being close to the expected update time, tuned down the frequency of the updates so that this time became larger.

Initially these were defaulted to 2.0Hz
```$xslt
    update_frequency: 0.5
    publish_frequency: 0.5
```