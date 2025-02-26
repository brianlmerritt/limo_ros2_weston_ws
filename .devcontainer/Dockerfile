#FROM westonrobot/ros:humble-ci
#FROM ros_limo_realsense:r36.2.0-realsense
FROM l4t-ros2:humble
ENV DEBIAN_FRONTEND=noninteractive
ENV USERNAME jetson
ENV HOME /home/$USERNAME
USER $USERNAME
WORKDIR /home/$USERNAME
SHELL ["/bin/bash", "-l", "-c"]

# Set up user account
#RUN useradd -ms /bin/bash ros && echo "ros:ros" | chpasswd && adduser ros sudo
#RUN adduser ros video
# Builder dependencies installation
RUN sudo apt-get update \
    && sudo apt-get install -qq -y --no-install-recommends \
    build-essential \
    cmake \
    git \
    libssl-dev \
    libusb-1.0-0-dev \
    pkg-config \
    libgtk-3-dev \
    libglfw3-dev \
    libgl1-mesa-dev \
    libglu1-mesa-dev \
    libglvnd0 \
    libglx0 \
    libegl1 \
    libxext6 \
    libx11-6 \
    curl \  
    libusb-1.0* \
    udev \
    apt-transport-https \
    ca-certificates \
    curl \
    swig \
    software-properties-common \
    python3-pip \
    ros-humble-rmw-cyclonedds-cpp \
    ros-humble-rviz2* \
    ros-humble-cartographer* \
    ros-humble-nav* \
    ros-humble-teleop-twist-keyboard \
    ros-humble-joint-state-publisher* \
    ros-humble-robot-state-publisher* \ 
    ros-humble-xacro \
    ros-humble-imu-tools \
    libgflags-dev \
    nlohmann-json3-dev \
    ros-humble-image-* \
    && sudo rm -rf /var/lib/apt/lists/*
RUN git clone https://github.com/YDLIDAR/YDLidar-SDK.git &&\
    mkdir -p YDLidar-SDK/build && \
    cd YDLidar-SDK/build &&\
    cmake ..&&\
    make &&\
    sudo make install &&\
    cd .. &&\
    pip install . &&\
    cd .. && rm -r YDLidar-SDK 
RUN wget -c https://github.com/google/glog/archive/refs/tags/v0.6.0.tar.gz  -O glog-0.6.0.tar.gz &&\
    tar -xzvf glog-0.6.0.tar.gz &&\
    cd glog-0.6.0 &&\
    mkdir build && cd build &&\
    cmake .. && make -j4 &&\
    sudo make install &&\
    sudo ldconfig &&\
    cd ../.. && rm -r glog-*
RUN wget -c https://github.com/Neargye/magic_enum/archive/refs/tags/v0.8.0.tar.gz -O  magic_enum-0.8.0.tar.gz &&\
    tar -xzvf magic_enum-0.8.0.tar.gz &&\
    cd magic_enum-0.8.0 &&\
    mkdir build && cd build &&\
    cmake .. && make -j4 &&\
    sudo make install &&\
    sudo ldconfig &&\
    cd ../.. && rm -r magic_enum*
RUN git clone https://github.com/libuvc/libuvc.git &&\
    cd libuvc &&\
    mkdir build && cd build &&\
    cmake .. && make -j4 &&\
    sudo make install &&\
    sudo ldconfig  &&\
    cd ../.. && rm -r libuvc*

ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES graphics,utility,compute
ENV RMW_IMPLEMENTATION=rmw_cyclonedds_cpp

RUN echo "source /opt/ros/humble/setup.bash" >> $HOME/.bashrc

RUN cd $HOME &&\
    git clone https://github.com/brianlmerritt/limo_ros2_weston_ws.git &&\
    cd limo_ros2_weston_ws &&\
    source /opt/ros/humble/setup.bash &&\
    colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release &&\
    echo "source $HOME/limo_ros2_weston_ws/install/setup.bash" >> $HOME/.bashrc