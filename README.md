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

    Follow the instructions to change the Rviz parameters.
    
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

    Open a third terminal and run the command:

    ```sh
    pcl_viewer your_file_name.pcd
    ```
    
    You will be able to examine your pointcloud by opening automatically a window like this:
    
   



    
    > For openning .bag files:

    Open 2 terminals:

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
    
    When the Rviz will be running, enable the PointCloud2 message.
    
    In case of failures or if you can't see the pointcloud, please "play" by enabling different parameters (mainly in *Global Options* and *PointCloud2* sections)

    

    For more details please read carefully the link: https://github.com/IntelRealSense/realsense-ros/wiki/SLAM-with-D435i