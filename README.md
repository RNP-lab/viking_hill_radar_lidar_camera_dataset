### 📚 Table of Contents

* [Dataset Description](#dataset-description)
* [Dataset Parts](#dataset-parts)
* [Data Structure and File Organization](#data-structure-and-file-organization)
* [Downloads](#downloads)
* [Examples and Teasers](#examples-and-teasers)
* [Acknowledgement](#acknowledgement)

# The Radar Forest Dataset [working title]

## Dataset description
![Husky robot during the experiment](media/husky_title_sm.jpg)
*The recorded dataset captures a forested area that contains fully grown and young trees, dense undergrowth, bumpy terrain and occasional rocks.*

The dataset captures a forested area that contains both fully grown and young trees, dense undergrowth, bumpy terrain and occasional rocks.
Clearpath Husky robot was teleoperated through the area in May and June 2024, each time driving roughly one hour while recording its onboard sensors: LiDAR, high-resolution 4D radar, GNSS, RGB camera, IMU and internal odometry.
Thanks to the GNSS receiver and lidar data, reference lidar point cloud map and thus absolute positioning of the robot during the experiment are available.
Moreover, to support 4D radar research in detetection and classification of obstacles in similar environments, the dataset provides labels (3D cuboids) for several classes (e.g., tree trunk, tree canopy, bush, ...) and
ROS2 tools that use these cuboids to label arbitrary point clouds in the dataset (lidar, radar, accumulated versions of those, or sub-maps).


![3D cuboids labeling tree canopies, tree trunks, small trees, and several other categories](media/labels_sm.jpg)
*Using the [Segments.ai](https://segments.ai) online 3D labeling solution, the reference point cloud maps from two recording runs were manually labelled.*


The data are provided as ROS bag files, both for ROS1 and ROS2, and the point-cloud-labeling tools are available for ROS2. 
Since the labelled point clouds are published as standard `PointCloud2` messages, it is possible to convert the labelled clouds back to ROS1 either by ROS bridge, or by converting bag files (e.g., by [Rosbags](https://pypi.org/project/rosbags/) library.)
A tool for point cloud accumulation is also provided. Based on the message count or distance travelled, denser point clouds are produced and optionally saved in `.pcd` format, allowing easy post-processing.

![Vive data in Rviz](media/rviz_live_sm.jpg)
*Provided ROS tools annotate the radar or lidar pointclouds, display used cuboids in Rviz.*

---

## Dataset Components

### Data from the recording sessions

* **Short grass: May 2024** (4068s)
Area of approx. 200x200m was captured by the Husky mobile robot in a "search pattern" fashion, resulting in dense coverage by all the onboard sensors.
In the beginning of summer, the undergrowth is not yet fully developed, therefore the visibility conditions for the camera and lidar are good.
Besides the raw data from the robot, a reference map is provided as well as raw GNSS data from a static and mobile receiver.

![Husky in the short grass session](media/short_grass_husky.jpg)
![The robot trajectory - the "search pattern"](media/short_grass_traj.jpg)
![The point cloud map of the captured area](media/short_grass_map.jpg)

* **Tall grass: June 2024** (3811s)
The same area was visited again one month later in June, when the undergrowth became in some sections even higher than the robot itself.
In these conditions, the comparison between camera, lidar and radar becomes interesting for the field robotics applications.

![Husky in the tall grass session](media/tall_grass_husky.jpg)
![The robot trajectory - the tall grass session](media/tall_grass_traj.jpg)
![The point cloud map of the captured area](media/tall_grass_map.jpg)

### ROS1 and ROS2 tools

* **ROS1 tools - helper launch files**
The ROS1 variant of the bag files is accompanied by a set of launch files allowing reprocessing of the Ouster lidar raw packets, adding additional TFs, and publishing the reference point cloud map for Rviz visualisation.
The tools are available in a separate repository: **TODO** 

* **ROS2 tools - point cloud labelling tools**
The provided ROS2 tools allow online labeling of point clouds, either lidar or radar, based on the provided set of cuboids stored in a yaml file (included).
For convenience, a tool for accumulating a series of pointclouds, republishing them and also saving as `.pcd` files is also provided.
Similarly to ROS1, a launch file for publishing a reference point cloud map and Rviz config file are also available.
The tools are available in a separate repository: **TODO** 
---

## Data Structure and File Organization

```
├── ros1_noetic
│   ├── calibration
│   │   ├── extrinsics
│   │   │   ├── extrinsics.txt
│   │   │   └── frames.pdf
│   │   └── intrinsics
│   │       ├── camera_calibration.txt
│   │       └── hugin_radar_startup_params.txt
│   └── data
│       ├── 2024_05_short_grass_run
│       │   ├── bags
│       │   │   ├── short_grass__ros1__00.bag
│       │   │   ├── ...
│       │   │   └── short_grass__ros1__45.bag
│       │   ├── gps
│       │   │   ├── filtered_RTK_solution.pos
│       │   │   ├── full_RTK_solution.pos
│       │   │   ├── ReachBaseSt_20240501125754
│       │   │   ├── ReachRoverO_20240501133817
│       │   │   └── readme.txt
│       │   └── reference_point_cloud_map
│       │       ├── short_grass_map.pcd
│       │       ├── short_grass_map_subsampled.pcd
│       │       └── short_grass_map_with_normals.vtk
│       └── 2024_06_tall_grass_run
│           ├── bags
│           │   ├── tall_grass__ros1__00.bag
│           │   ├── ...
│           │   └── tall_grass__ros1__44.bag
│           ├── gps
│           │   ├── filered_RTK_solution.pos
│           │   ├── full_RTK_solution.pos
│           │   ├── ReachBaseSt_20240612080138
│           │   ├── ReachRoverO_20240612080516
│           │   └── readme.txt
│           └── reference_point_cloud_map
│               ├── readme.txt
│               ├── tall_grass_map.pcd
│               ├── tall_grass_map_subsampled.pcd
│               └── tall_grass_map_with_normals.vtk
└── ros2_jazzy
    ├── calibration                                                # Same contents as in ROS1
    ├── cuboid_labels
    │   └── short_and_tall_grass_labels.json
    └── data
        ├── 2024_05_short_grass_run
        │   ├── bag
        │   │   └── short_grass__ros2
        │   │       ├── metadata.yaml
        │   │       ├── short_grass__ros2_0.mcap
        │   │       ├── ...
        │   │       └── short_grass__ros2_48.mcap
        │   ├── gps                                                # Same contents as in ROS1
        │   └── reference_point_cloud_map                          # Same contents as in ROS1
        └── 2024_06_tall_grass_run
            ├── bag
            │   └── tall_grass__ros2
            │       ├── metadata.yaml
            │       ├── tall_grass__ros2_0.mcap
            │       ├── ...
            │       └── tall_grass__ros2_49.mcap
            ├── gps                                                # Same contents as in ROS1
            └── reference_point_cloud_map                          # Same contents as in ROS1
```

* `xxx` → Raw sensory data and static transforms.
* `calibration/extrinsics/` → Transformations between sensor frames.
* `calibration/instrinsics/` → Intrinsic parameters for the camera and radar settings.

### Sensors
The dataset provides sensor measurements from these sensors:

* Sensrad Hugin A3-Sample (solid-state 4D radar)
  * Please note that the Hugin A3-Sample radar used in our dataset is an early demo model not with the same performance as the forthcoming production-ready model.
* Ouster OS0-32 (3D lidar)
  * This sensor is available for tuning and verification of your SLAM solution, but not available in the competition runs (i.e., the topic with point clouds will not be published in the Docker environment).  
* IDS Imaging uEye camera (2056x1542px)
* Xsens MTi-30 (IMU)
* Emlid Reach RS2+ (RTK-GNSS receiver pair)

### Reference Contents

TODO:
The dataset contains a `reference/` subdirectory with:

* `reference_train_bagfile.bag`: Reference GNSS RTK localization synchrized with the robot time, saved as a bag file.
* `reference.txt`: The GNSS RTK expressed in the UTM coordinates, with time stamps from the robot. Format: **timestamp[s], northing[m], easting[m], elevation, qx, qy, qz, qw**. Note that the quarternion is always identity.
* `reference_train_gps_rtk_in_robot_time.csv`: Contains the same information as `reference.txt`, but expressed in latitude and longitude. Format: **secs, nsecs, latitude, longitude, elevation**.
* `gps_filtered_high_accuracy.pos`: RTK solution used to generate the reference samples for the files above. It does not contain sections with too few sattelites. Note that the displayed time is the GPS time (no time zone, no step seconds).
* `gsp_original_post_fix_including_bad_sections.pos`: Complete RTK solution, wih all samples including the noisy ones.
* `train_robot_time_to_gps_time.csv`: Conversion from the robot time to the time indicated by the GNSS. The robot was no exactly synchronized with the GNSS, there is approx. 0.6s offset. This file can be used to match those times. Format: **robot secs, robot nsecs, gnss secs, gnss nsecs** 

---

## Downloads

* [TODO](https://www.todo.com) (XX GB in total)

---

## Examples and Teasers

![Forested area with trees, undergrowth and bumpy terrain](media/husky_sm.jpg)

*Recorded in June, the grass was tall enough to often obscure the robot sensors.*

![Forested area with trees, undergrowth and bumpy terrain](media/vegetation_sm.jpg)

*The robot was intentionally driven through bushes and over uneven terrain.*

### Video Teasers

Removed for double-blind review

## Acknowledgement

The camera stream in this dataset was anonymized using [EgoBlur](https://github.com/facebookresearch/EgoBlur), and [deface](https://github.com/ORB-HD/deface) automated tools.
* Raina, N., Somasundaram, G., Zheng, K., Miglani, S., Saarinen, S., Meissner, J., Schwesinger, M., Pesqueira, L., Prasad, I., Miller, E., Gupta, P., Yan, M., Newcombe, R., Ren, C., & Parkhi, O. M. (2023). EgoBlur: Responsible Innovation in Aria. arXiv preprint [arXiv:2308.13093](https://arxiv.org/abs/2308.13093).
* Optimization in Robotics and Biomechanics, Deface, (accessed 2025), GitHub repository, [https://github.com/ORB-HD/deface](https://github.com/ORB-HD/deface)

The dataset was labelled using the online tools from [Segments.ai](https://segments.ai) who generously provided us a free academic license.
* Segments.ai (2023). Segments.ai data labeling platform, [https://segments.ai](https://segments.ai).
  



