# ME326 Group-8 Final Project

The goal of this project was to use a Locobot to engage in a resource-gathering task. The Locobot knows a pattern it must create at one AprilTag. At the other AprilTag, it must mimic the pattern created by another player. In this version, we simulated the other player having already created their pattern. The Locobot must not enter the resource region and cannot leave its "side" of the environment.

## Video of Simulation
[![Project Video](https://img.youtube.com/vi/sheLVeNuB7Y/0.jpg)](https://www.youtube.com/watch?v=sheLVeNuB7Y)

## Installation Notes

### April Tag Models
You need to copy the April Tag models to the `.gazebo/model` directory, or add the full path to `gazebo_aptriltag/models` to the `GAZEBO_MODEL_PATH` environment variable.

To copy the models, run the following commands:
```bash
cp -r gazebo_apriltag/models/* ~/.gazebo/models/
```

If the `~/.gazebo/models` directory does not exist, you can create it by running the following command:
```bash
mkdir -p ~/.gazebo/models
```

### aprtiltag_ros
This process invovles a few steps.

#### OpenCV
First, we need to install opencv from source to ensure we have the correct version. The steps can be followed from the directions from OpenCV ([link](https://docs.opencv.org/4.x/d7/d9f/tutorial_linux_install.html)). The following is a summary of the steps:
```bash
# Install minimal prerequisites
sudo apt update && sudo apt install -y cmake g++ wget unzip
# Download and unpack sources
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.x.zip
unzip opencv.zip
# Create build directory
mkdir -p build && cd build
# Configure
cmake  ../opencv-4.x
# Build
cmake --build .
# Install
sudo make install
```

#### Supportin Repos
Next, we need to clone the supporting repos for the apriltag_ros package. Supporting repos:
- [apriltag](https://github.com/AprilRobotics/apriltag)
- [apriltag_msgs](https://github.com/christianrauch/apriltag_msgs)
- galactic branch of [vision_opencv](https://github.com/ros-perception/vision_opencv/tree/galactic)
  - Note: installing vision_opencv using `sudo apt-get install ros-galactic-vision-opencv` caused errors. However, building this specific branch from source worked.

Clone these in your project directory.
```bash
git clone https://github.com/AprilRobotics/apriltag.git
git clone https://github.com/christianrauch/apriltag_msgs.git
git clone -b galactic https://github.com/ros-perception/vision_opencv.git
```

#### apriltag_ros
We made some modifications of the apriltag_ros package to allow us to accept a different frame_id for the image. We also modified/addded a lunch file with the appropriate remappings for our robot. The changes and required files are contained in this repo under the `apriltag_ros` directory.

## To Execute

The Locobot pattern is must create is defined in `src/final_project/pattern/own_pattern.yaml`. To change the pattern that appears on the other side of the arena (the one simulated to be created by another player), change `src/final_project/launch/spawn_cube.launch.py`.

### To Run

After building all components:

```bash
ros2 launch final_project project.launch.py
```

After the environment loads:

```bash
ros2 run final_project state_machine
```