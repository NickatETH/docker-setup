FROM osrf/ros:foxy-desktop

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get -y update; apt-get -y install wget

RUN bash -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' && \
    apt install -y curl && \
    curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - && \
    apt update && \
    apt install -y ros-noetic-desktop-full

# install MAVSDK
RUN wget https://github.com/mavlink/MAVSDK/releases/download/v1.4.12/libmavsdk-dev_1.4.12_ubuntu20.04_amd64.deb
RUN dpkg -i libmavsdk-dev_1.4.12_ubuntu20.04_amd64.deb
RUN rm libmavsdk-dev_1.4.12_ubuntu20.04_amd64.deb

RUN mkdir -p /raptor_ws/src 
WORKDIR /raptor_ws/src

RUN git clone https://github.com/raptor-ethz/PX4-Autopilot.git

WORKDIR /raptor_ws/src/PX4-Autopilot
RUN bash ./Tools/setup/ubuntu.sh && \
    apt install -y libopencv-dev

RUN git checkout -b RAPTOR-foxy origin/RAPTOR-foxy && \
    git tag v1.9.1 
    # git submodule update --recursive --force --init --remote

WORKDIR /raptor_ws/src
RUN echo "hello"
RUN git clone https://github.com/raptor-ethz/raptor.git 

RUN cd / && ./ros_entrypoint.sh && \
    . /opt/ros/foxy/setup.sh && \
    cd /raptor_ws/src/raptor && \
    # git checkout -b foxy origin/foxy && \
    chmod 777 -R . && \
    colcon build --cmake-args \
    -DCMAKE_CXX_STANDARD=17 --packages-select raptor_interface gripper_control quad_control vicon && \
    . ./install/setup.sh

# WORKDIR /raptor_ws/PX4-Autopilot
# RUN HEADLESS=1 make px4_sitl gazebo_plane
