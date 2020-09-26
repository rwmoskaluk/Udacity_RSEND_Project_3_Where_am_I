# Udacity_RSEND_Project_3_Where_am_I



# Notes:


For Ubuntu 18.04 LTS ROS Melodic when using pgm_map_creator package, utilize the branch (dudasdavid:libprotobuf-fix) that fixes the libprotobuf compiling problem along with allowing pgm_map_creator to run in Melodic.  As of 9/24/2020 the branch has not merged with master yet.  Changes shown below.



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