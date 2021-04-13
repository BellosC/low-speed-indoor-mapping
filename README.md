# lowspeedslam-d435i

## low speed 3D mapping and slam using realsense d435i camera for indoor environment


1. ## Installation - *Basic tools*
    > Install Ubuntu, ROS, Intel Realsense SDK, Wrappers from here:
    https://github.com/IntelRealSense/realsense-ros

    Note: I recommend the *Method 2*

1. ## Installation - *SLAM with D435i* package
    > Install the following nodes from the link: https://github.com/IntelRealSense/realsense-ros/wiki/SLAM-with-D435i

    1. realsense2_camera

    1. imu_filter_madgwick

    1. rtabmap_ros

    1. robot_localization

    When everything is installed succesfully, open a terminal and run the command:

    ```sh
    roslaunch realsense2_camera opensource_tracking.launch
    ```

    Follow the instructions of the link above, to adjust the Rviz parameters.
    
    When the parameters are set, and the Pointcloud2 and MapCloud nodes are up and running, open two ,ore terminals and simultaneously with the first terminal run the commands:
    > Terminal2:  Capture and save .pcd files:
    ```sh
    rosrun pcl_ros pointcloud_to_pcd input:=/rtabmap/cloud_map
    ```

    > Terminal3: Capture and save .bag files:
    ```sh
    rosbag record -O my_bagfile_1.bag /camera/aligned_depth_to_color/camera_info  camera/aligned_depth_to_color/image_raw /camera/color/camera_info /camera/color/image_raw /camera/imu /camera/imu_info /tf_static
    ```

    
    Move the camera very-very slowly and smoothly and try to cover your area of interest.

    When you are done, terminate all terminals and in your computer you will find your .pcd (many) and .bag (only one) files.

    ## Examine your work ##
    > For openning .pcd files:

    Install the pcl-tools:

    ```sh
    sudo apt-get install pcl-tools
    ```

    Open a new terminal and run the command:

    ```sh
    pcl_viewer your_file_name.pcd
    ```

 

    You will be able to examine your pointcloud from a window like this:

    ![Indoor-github](https://user-images.githubusercontent.com/70270581/113284324-24148e80-92f2-11eb-8b54-8695b181ef42.png)

   

    
    > For openning .bag files:

   Close all termminals and open 2 new terminals:

    1. Run terminal_1:

    ```sh
    roslaunch realsense2_camera opensource_tracking.launch offline:=true
    ```

    1. Run in terminal_2:

    ```sh
    roscore >/dev/null 2>&1 &
    ```

    ```sh
    rosparam set use_sim_time true
    ```

    ```sh
    rosbag play my_bagfile_1.bag --clock
    ```
    
    When the Rviz will be running, enable the **MapCoud** message.
    
    
    ![bag](https://user-images.githubusercontent.com/70270581/113284227-ff201b80-92f1-11eb-8500-d5c51e08be23.png)

    
    In case of failures or if you can't see the pointcloud, please "play" by enabling different parameters (mainly in **Global Options** and **MapCloud** sections)
    
    If you move the camera really slow and smooth you will be able to get a **SLAM result** like the following:
    
    ![slam_house](https://user-images.githubusercontent.com/70270581/113325472-2b06c580-9321-11eb-904c-ce66fac14d40.png)


        > Tip: In order to save SLAM result be sure that you have enough space in your computer in order to save the .bag file.


    For more details please read carefully the link: https://github.com/IntelRealSense/realsense-ros/wiki/SLAM-with-D435i


# rtabmap_ros (2nd way)

open terminal_1 and run:

```sh
roslaunch realsense2_camera rs_camera.launch align_depth:=true unite_imu_method:="linear_interpolation" enable_gyro:=true enable_accel:=true
```
open terminal_2 and run:

```sh
rosrun imu_filter_madgwick imu_filter_node \
    _use_mag:=false \
    _publish_tf:=false \
    _world_frame:="enu" \
    /imu/data_raw:=/camera/imu \
    /imu/data:=/rtabmap/imu
```

open terminal_3 and run:

```sh
roslaunch rtabmap_ros rtabmap.launch \
    rtabmap_args:="--delete_db_on_start --Optimizer/GravitySigma 0.3" \
    depth_topic:=/camera/aligned_depth_to_color/image_raw \
    rgb_topic:=/camera/color/image_raw \
    camera_info_topic:=/camera/color/camera_info \
    approx_sync:=false \
    wait_imu_to_init:=true \
    imu_topic:=/rtabmap/imu
```
After those 3 commands, the rtabmap window will open trying to capture a pointcloud:

![rtbmp Screenshot ](https://user-images.githubusercontent.com/70270581/113558887-1ec48600-9609-11eb-8af2-5346ca9260d5.png)

From this environment, when you pause the procedure, you can save the pointcloud as .pcl or .ply and you can also save it as a triangulated mesh.

Also the map is saved as a database in this path:

```sh
~/.ros/rtabmap.db
```

You can view this database using an RTAB-Map tool by typing:

```sh
rtabmap-databaseViewer ~/.ros/rtabmap.db
```

A window like this will open, containing more info about the pointcloud:

![database-rtab](https://user-images.githubusercontent.com/70270581/113560552-d5296a80-960b-11eb-964b-921c06007e12.png)


For more infos about the *rtabmap_ros* procedure please follow the linκ: http://wiki.ros.org/rtabmap_ros/Tutorials/HandHeldMapping




# rosbag to pcd - subscribing to different topics
Through this procedure you can save .pcd files by using different topics (Pointcloud2 type).

> Step 1: Activate camera and Rviz and set the parameters as said before in the instructions.

```sh
roslaunch realsense2_camera opensource_tracking.launch
```

> Step 2: Open a second terminal and record a rosbag containing all topics available.

```sh
rosbag record -a
```

> Step 3: When you stop recording, use the command below in order to view info of your recorded rosbag

```sh
rosbag info your_rosbag_file.bag
```
![info](https://user-images.githubusercontent.com/70270581/114520249-eac11480-9c49-11eb-9b10-6e3498560cf6.png)


