# Snapdragon Flight VISLAM-ROS Sample Code
This repository is a fork of the VISLAM from ATLFlight/ros-examples, modified to work with the px4 flight stack and get IMU sensor data from mavros instead of the `imu_app`. The original README can be found in the [ATLFlight/ros-examples](https://github.com/ATLFlight/ros-examples/blob/master/README.md) repository.

## Preparations
### Mavros
Get the mavros sources into your catkin's source directory by following the instructions from the [mavros git page](https://github.com/mavlink/mavros/tree/master/mavros#source-installation).

**Note**: Step 3 should also include `--rosdistro kinetic` flag (thanks  @Seanmatthews).

### Other packages
On a freshly flashed snapdragon, I had to install the following packages
* python pip
* python future (`pip install future`)
* `sudo apt-get install ros-indigo-diagnostic-updater`
* `sudo apt-get install ros-indigo-angles`
* `sudo apt-get install ros-indigo-eigen-conversions`
* `sudo apt-get install ros-indigo-urdf`
* `sudo apt-get install ros-indigo-tf`
* `sudo apt-get install ros-indigo-control-toolbox`
  If `control-toolbox` cannot be installed due to unmet dependencies, you might have to install the missing dependency yourself. In my case overwriting an existing file was necessary for the package `fontconfig-config`. That can be done with
  `sudo apt-get -o Dpkg::Options::="--force-overwrite" install fontconfig-config`

## Build
`catkin build`

Compiling mavros can easily take 30 minutes on the snapdragon.


## Configure
Copy the px4 configuration files from `./px4_configs` to their respective location on the snapdragon. For using the recommended LPE (successful flight test):
```
cd snapdragon_mavros_vislam
adb push ./px4_configs/lpe/mainapp.conf /home/linaro/mainapp.conf
adb push ./px4_configs/lpe/px4.config /usr/share/data/adsp/px4.config
```

and if you want to give EKF2 a try (no flight test):
```
cd snapdragon_mavros_vislam
adb push ./px4_configs/ekf2/mainapp.conf /home/linaro/mainapp.conf
adb push ./px4_configs/ekf2/px4.config /usr/share/data/adsp/px4.config
```

These configurations are intended to provide a working starting point. You may of course adjust parameters to your likings.

## Run
Get a roscore up and running
```
adb shell
roscore
```

In another terminal, launch mavros and vislam
```
adb shell
roslaunch snapdragon_mavros_vislam mavros_vislam.launch
```

Start the px4 flight stack
```
adb shell
cd $HOME
./px4 mainapp.config
```
