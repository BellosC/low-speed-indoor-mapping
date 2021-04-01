# lowspeedslam-d435i

## Glow speed 3D mapping and slam using realsense d435i camera for indoor environment


1. ## Installation - *Basic tools*
    > Install Ubuntu, ROS, Intel Realsense SDK, Wrappers from here:
    https://github.com/IntelRealSense/realsense-ros

    Note: I recommend the *Method 2*

1. ## Installation - *SLAM with D435i* package
    > Istall the following from the link: https://github.com/IntelRealSense/realsense-ros/wiki/SLAM-with-D435i

    1. realsense2_camera

    1. imu_filter_madgwick

    1. rtabmap_ros

    1. robot_localization

    Note: Open a terminal and run the command:
    > roslaunch realsense2_camera opensource_tracking.launch

    Follow the instructions to change the Rviz parameters 
    
    When the parameters are set, and the Pointcloud2 and MapCloud nodes are up and running, open another terminal and simultaneously with the first terminal run the command:
    > rosrun pcl_ros pointcloud_to_pcd input:=/rtabmap/cloud_map
    
    move the camera very-very slowly and smoothly and try to cover your area of interest.

    When you are done, terminate the two running programms and in your home directory you will find your pcd files.

    For more details please read carefully the link: https://github.com/IntelRealSense/realsense-ros/wiki/SLAM-with-D435i