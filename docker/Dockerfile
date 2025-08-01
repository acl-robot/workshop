# based on ROS Humble
FROM osrf/ros:humble-desktop-full

# install additional packages
RUN apt-get update && apt-get install -y \
    htop \
    vim \
    && rm -rf /var/lib/apt/lists/*
