# velo2cam_gazebo
Repository including Gazebo models, plugins and worlds to test algorithms for extrinsic calibration of lidar-camera pairs. Package developed at [Intelligent Systems Laboratory](http://www.uc3m.es/islab), Universidad Carlos III de Madrid.

![gazebo screenshot](screenshots/velo2cam_calibration_setup.png)

## Gazebo models
This repository includes several sensors and calibration target models to evaluate the performance of extrinsic calibration of lidar-camera pair in the Gazebo Simulator. The models need to be moved to ~/.gazebo/models/ if you want access them from Gazebo.

Note: The models included in this repository were designed for evaluating the LIDAR-camera calibration algorithms described in [1] and [2], whose code is provided [here](https://github.com/beltransen/velo2cam_calibration).

Sensors:

* Monocular cameras: FLIR Blackfly S 31S4C-C
* Stero cameras: FLIR Bumblebee XB3 Camera (Left - center only)
* LiDARs: Velodyne VLP-16, Velodyne HDL-32, Velodyne HDL-64

Calibration targets:

* Different calibration target embodiments.
* Some checkerboard planes for recreating worlds required to test the [KIT Calibration Toolbox](http://www.cvlibs.net/software/calibration/) and [Matlab Calibration Toolbox](https://es.mathworks.com/help/lidar/ug/lidarcameracalibrationexample.html).

## Gazebo plugins
* Velodyne plugin providing PointCloud2 with same structure as driver (x, y, z, intensity, ring) and simulated Gaussian noise. (Code from [DataspeedInc](https://bitbucket.org/DataspeedInc/velodyne_simulator), although minor patch for vertical resolution issue is included)

## Build
Compile issues caused by deprecated functions have been fixed by referring to [issues](https://github.com/osrf/car_demo/issues/35)

tested on Ubuntu 18.04 ROS Melodic Gazebo 9.0
```
cd ~/catkin_ws/src
git clone https://github.com/wuhan-geodesy/lidar_simulator.git
cd ../
catkin build

```

### Test

1. Download gazebo models from [here](https://github.com/osrf/gazebo_models), and put them to both
.gazebo/models and /user/share/gazebo-9/models.

2. Run the roslaunch command,
```
source ~/catkin_ws/devel/setup.bash

export GAZEBO_PLUGIN_PATH=~/catkin_ws/devel/lib:${GAZEBO_PLUGIN_PATH}

export GAZEBO_MODEL_PATH=~/catkin_ws/src/velo2cam_gazebo/gazebo_models:/usr/share/gazebo-9/models:${GAZEBO_MODEL_PATH}

export GAZEBO_RESOURCE_PATH=~/catkin_ws/src/velo2cam_gazebo/gazebo_models:/usr/share/gazebo-9/models:${GAZEBO_RESOURCE_PATH}

export GAZEBO_MODEL_DATABASE_URI=http://models.gazebosim.org/

roslaunch velo2cam_gazebo single_vlp16_p1_ideal.launch
```

## Known Issues
* Gazebo can take up to 30 seconds to load the VLP-16 pluggin, 60 seconds for the HDL-32E, and much more HDL-64E
* Gazebo cannot maintain 10Hz with large pointclouds
    * Solution: User can reduce number of points in urdf

## Gazebo Calibration Scenarios
* Launch files for all calibrations scenarios used in the related published manuscripts are included. To launch a sample scenario, run:
	```roslaunch velo2cam_gazebo mono_hdl64_p1_real.launch```

## Citation
If you use this work in your research, please consider citing the following papers:

[1] Beltrán, J., Guindel, C., and García, F. (2021). [Automatic Extrinsic Calibration Method for LiDAR and Camera Sensor Setups](https://arxiv.org/abs/2101.04431). arXiv:2101.04431 [cs.RO]. Submitted to IEEE Transactions on Intelligent Transportation Systems. [Preferred citation]

[2] Guindel, C., Beltrán, J., Martín, D., and García, F. (2017).  Automatic Extrinsic Calibration for Lidar-Stereo Vehicle Sensor Setups. *IEEE International Conference on Intelligent Transportation Systems (ITSC), 674–679*. Pre-print available [here](https://arxiv.org/abs/1705.04085).

