FROM osrf/ros:humble-desktop

RUN apt-get -y update; apt-get -y install wget


RUN mkdir /raptor_ws 
WORKDIR /raptor_ws

RUN git clone https://github.com/NOCTUA-ETHZ/MSD_Sys_ros 


RUN git checkout -b RAPTOR origin/RAPTOR && \
    git tag v1.9.1


RUN cd / && ./ros_entrypoint.sh && \
    . /opt/ros/humble/setup.sh && \
    cd /raptor_ws/raptor && \
    chmod 777 -R . && \
    colcon build --packages-select raptor_interface gripper_control quad_control vicon && \
    . ./install/setup.sh

# WORKDIR /raptor_ws/PX4-Autopilot
# RUN HEADLESS=1 make px4_sitl gazebo_plane




