# Kimera-Semantics with Segmentation over ADE20K classes in PyTorch

### Real-Time 3D Semantic Reconstruction using Kimera Sematics and Semantic Segmentation in Pytorch with 150 classes.

Implemented on **Kimera-Semantics** by **MIT SPARK Lab** and **semantic-segmentation-pytorch** by **MIT CSAIL Computer Vision Lab**


## Environment
The code is tested under the following configuration.
- Ubuntu 18.04 LTS
- **CUDA=10.2**
- **PyTorch>=1.5.1**
- **ROS Melodic**


## 1. Installation

### A. Prerequisities

- Install ROS by following the official [ROS website](https://www.ros.org/install/).

- Install dependencies:
```bash
sudo apt-get install python-catkin-tools
sudo apt-get install python-wstool python-catkin-tools  protobuf-compiler autoconf
sudo apt-get install ros-melodic-cmake-modules

# install Python3 dependencies
pip3 install rospkg
pip3 install numpy scipy
pip3 install pytorch>=0.4.1 torchvision
pip3 install yacs tqdm
```

### B. Setup

Using [catkin](http://wiki.ros.org/catkin):

```bash
# Setup catkin workspace
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin init
catkin config --extend /opt/ros/melodic
catkin config --cmake-args -DCMAKE_BUILD_TYPE=Release
catkin config --merge-devel

# Add workspace to bashrc.
echo 'source ~/catkin_ws/devel/setup.bash' >> ~/.bashrc

# Clone repo
cd ~/catkin_ws/src
git clone https://github.com/pranay731/Kimera-Semantics-with-Segmentation.git

# Install dependencies from rosinstall file using wstool
wstool init # Use unless wstool is already initialized

# For https:
wstool merge Kimera-Semantics/kimera/install/kimera_semantics_https.rosinstall
# For ssh:
#wstool merge Kimera-Semantics/kimera/install/kimera_semantics_ssh.rosinstall

# Download and update all dependencies
wstool update

# Make executable:
cd ..
sudo chmod +x src/ros-semantic-segmentation-pytorch/scripts/run_semantic_segmentation

# Build 
catkin build

# Refresh workspace
source ~/catkin_ws/devel/setup.bash
```

A number of models are available. Default is **resnet50dilated** for encoder and **ppm_deepsup** for decoder.
Download pretrained checkpoints for models from [CSAIL Website](http://sceneparsing.csail.mit.edu/model/pytorch) and copy them to ***src/ros-semantic-segmentation-pytorch/ckpt/{modelname}/***

Else Download ckpt for default model from [ckpt link](https://docs.google.com/uc?export=download&id=1xk_fxlrtQKP_FJfZGGV6RSMjqFdyhz_W) and extract and copy it in ckpt directory.
```bash
mkdir -p ./src/ros-semantic-segmentation-pytorch/ckpt
unzip 'path/to/downloaded/zip/file' -d ./src/ros-semantic-segmentation-pytorch/ckpt
```


## 2. Usage

It expects that camera with color and depth as well as odom and tf are being published by the robot.

Launch file :
- color_topic -> raw image rgb topic.
- depth_topic -> raw image depth topic.
- segmentation_topic -> raw image segmentation output.
- camera_info_topic -> camera color info topic.

Other Configurations can be tweaked from there respective package launch files.

```bash
roslaunch semantic_segmentation_ros semantic_segmentation.launch
```
