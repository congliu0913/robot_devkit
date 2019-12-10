# Set the base image to Ubuntu 18.04

FROM ubuntu:bionic

MAINTAINER Liu Cong "congx.liu@intel.com"

# user
USER root
# pahts
WORKDIR /root/

# export project infomation
ARG PROJECT_URL=https://github.com/intel/robot_devkit.git
ARG PROJECT_BRANCH=devel

RUN apt-get update && apt-get install -y git \
        vim \
        sudo \
        wget \
        && rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install --no-install-recommends -y locales \
        lsb-release \
        && rm -rf /var/lib/apt/lists/*

RUN git clone ${PROJECT_URL} -b ${PROJECT_BRANCH}
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8

# install tzdata
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y tzdata

# remove 'sudo'
RUN cd robot_devkit/packages && find |grep "\.deps"|xargs sed -i 's/sudo/ /g'
RUN cd robot_devkit/packages && find |grep "\.sh"|xargs sed -i 's/sudo/ /g'
RUN cd robot_devkit && find |grep "\.deps" |xargs sed -i 's/ -E / /'

# remove 'udevadm'
RUN cd robot_devkit && find |grep "\.deps"|xargs sed -i 's/udevadm/echo "udevadm"# udevadm/g'

# install ros2
RUN cd robot_devkit && apt-get update &&bash /root/robot_devkit/packages/common/deps/10-ros2.deps /root/robot_devkit/rdk_ws/third_party

# install intel deps
RUN cd robot_devkit && bash /root/robot_devkit/packages/perception/deps/10-intel-deps.deps /root/robot_devkit/rdk_ws/third_party

# install librealsense
RUN cd robot_devkit && bash /root/robot_devkit/packages/perception/deps/20-librealsense.deps /root/robot_devkit/rdk_ws/third_party

# install opencv
RUN cd robot_devkit && bash /root/robot_devkit/packages/perception/deps/31-opencv.deps /root/robot_devkit/rdk_ws/third_party

# install opencl
RUN cd robot_devkit && bash /root/robot_devkit/packages/perception/deps/32-opencl.deps /root/robot_devkit/rdk_ws/third_party

# install openvino
RUN cd robot_devkit && bash /root/robot_devkit/packages/perception/deps/33-openvino.deps /root/robot_devkit/rdk_ws/third_party

# install gpg
RUN cd robot_devkit && bash /root/robot_devkit/packages/perception/deps/41-gpg.deps /root/robot_devkit/rdk_ws/third_party

# install gpd
RUN cd robot_devkit && bash /root/robot_devkit/packages/perception/deps/42-gpd.deps /root/robot_devkit/rdk_ws/third_party

# install ur_modern_driver
RUN cd robot_devkit && bash /root/robot_devkit/packages/perception/deps/43-ur_modern_driver.deps /root/robot_devkit/rdk_ws/third_party

# install tool deps
RUN cd robot_devkit && bash /root/robot_devkit/packages/tools/deps/20-tools.deps /root/robot_devkit/rdk_ws/third_party

# install turtlebot3 deps
RUN cd robot_devkit && bash /root/robot_devkit/packages/turtlebot3/deps/20-turtlebot3.sbc.deps /root/robot_devkit/rdk_ws/third_party
