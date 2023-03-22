FROM osrf/ros:humble-desktop

# Update all packages
RUN apt update && apt upgrade -y

# Install extra packages
RUN apt install gdb -y
RUN apt install git -y

# Rosdep update
RUN rosdep update

# Source the ROS noetic setup file
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc

RUN apt-get -y update; apt-get -y install wget

# install MAVSDK
RUN wget https://github.com/mavlink/MAVSDK/releases/download/v1.4.12/libmavsdk-dev_1.4.12_ubuntu20.04_amd64.deb
RUN dpkg -i libmavsdk-dev_1.4.12_ubuntu20.04_amd64.deb 
RUN rm libmavsdk-dev_1.4.12_ubuntu20.04_amd64.deb

RUN mkdir -p /raptor_ws/src
WORKDIR /raptor_ws/src

RUN git clone https://github.com/raptor-ethz/raptor.git && \
    git clone https://github.com/raptor-ethz/PX4-Autopilot.git

WORKDIR /raptor_ws/src/PX4-Autopilot
RUN bash ./Tools/setup/ubuntu.sh && \
    apt install -y libopencv-dev

RUN git checkout -b RAPTOR origin/RAPTOR && \
    git tag v1.9.1


RUN cd / && ./ros_entrypoint.sh && \
    . /opt/ros/humble/setup.sh && \
    cd /raptor_ws/src/raptor && \
    chmod 777 -R . && \
    colcon build --packages-select raptor_interface gripper_control quad_control vicon && \
    . ./install/setup.sh


