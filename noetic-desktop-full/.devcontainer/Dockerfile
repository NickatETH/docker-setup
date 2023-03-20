FROM osrf/ros:noetic-desktop-full

# Update all packages
RUN apt update && apt upgrade -y

# Install extra packages
RUN apt install gdb -y
RUN apt install git -y

# Rosdep update
RUN rosdep update

# Source the ROS noetic setup file
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y && apt-get install -y wget git
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" \
    > /etc/apt/sources.list.d/ros-latest.list' && \
    wget http://packages.ros.org/ros.key -O - | sudo apt-key add -

RUN apt-get install -y ros-noetic-rqt ros-noetic-rqt-common-plugins


RUN apt-get install -y python3-catkin-tools python3-vcstool python3-osrf-pycommon

RUN apt-get install -y libglew-dev libopencv-dev libyaml-cpp-dev 

RUN apt-get install -y libblas-dev liblapack-dev libsuitesparse-dev

RUN apt-get install -y build-essential libtool ros-noetic-pcl-ros

RUN mkdir /root/svo_ws

WORKDIR /root/svo_ws
RUN catkin config --init --mkdirs --extend /opt/ros/noetic --cmake-args -DCMAKE_BUILD_TYPE=Release -DEIGEN3_INCLUDE_DIR=/usr/include/eigen3

WORKDIR /root/svo_ws/src
RUN git clone https://github.com/raptor-ethz/rpg_svo_pro_open.git

RUN vcs-import < ./rpg_svo_pro_open/dependencies.yaml
RUN touch minkindr/minkindr_python/CATKIN_IGNORE
RUN cd rpg_svo_pro_open/svo_online_loopclosing/vocabularies && ./download_voc.sh

RUN catkin build
