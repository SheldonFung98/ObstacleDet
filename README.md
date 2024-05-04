# Obstacle Detection Functionality

This project provides obstacle detection functionality based on the fusion of laser scanner and point cloud data, suitable for scenarios where a cleaning robot needs to navigate around obstacles.

![Overview](/assets/1.png)
![Overview](/assets/2.png)

## Usage
### Compilation
```
mkdir -p catkin_ws/src && cd catkin_ws/src
git clone https://github.com/SheldonFung98/ObstacleDet
cd ..
catkin_make
source devel/setup.sh
```

### Launching
```
roslaunch obstacle_det detect.launch
```

## Code Principle

### Wall Detection Functionality
Method 1:
1. Crop the point cloud ROI, downsample the point cloud within the ROI, and grid the point cloud.
2. Use RANSAC to find a plane with normals pointing upwards, which serves as the ground.
3. Elevate this plane (to ensure all ground points are filtered out) and filter out points below the plane.
4. Project non-ground points onto the plane and publish the point cloud.

Method 2:
1. Install the camera horizontally and set a threshold at a certain height; points below this threshold are considered as ground.

### Debug Visualization Functionality
1. Visualization information for obstacles can be obtained during debugging by subscribing to the topic `/rgbd_pc/obstacle`.
2. Visualization information for ground can be obtained during debugging by subscribing to the topic `/rgbd_pc/ground`.

## Communication Protocol
### Input
| Topic Name                        | Type                        | Description               | 
| -----------                      | -----------                 | -----------               |
| /camera$/depth/points            | sensor_msgs/Image          | Point cloud data          |

### Output
| Topic Name                       | Type                         | Description               | 
| -----------                     | -----------                  | -----------               |
| /rgbd_pc/obstacle               | sensor_msgs::PointCloud2     | Point cloud for obstacle detection (debugging) |
| /rgbd_pc/ground                 | sensor_msgs::PointCloud2     | Point cloud for ground detection (debugging)   |
| /rgbd_pc/laserCloud             | sensor_msgs::PointCloud2     | Point cloud projected onto a 2D plane         |
