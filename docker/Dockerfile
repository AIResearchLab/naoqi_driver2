ARG ROS_DISTRO=humble

FROM ros:${ROS_DISTRO} as dev

ENV ROS_DISTRO=${ROS_DISTRO}
ENV DEBIAN_FRONTEND=noninteractive
ENV workspace=/pepper

# Install extra tools for development
RUN apt-get update -y
RUN apt-get install -y --no-install-recommends  gdb \
                                                gdbserver \
                                                nano \
                                                libgmock-dev \
                                                git \
                                                python3-vcstool

# Create Colcon workspace with the project and its deps

WORKDIR ${workspace}/src

RUN git clone https://github.com/AIResearchLab/naoqi_driver2.git

RUN vcs import < naoqi_driver2/dependencies.repos

# Build the base Colcon workspace, installing dependencies first.
WORKDIR ${workspace}

RUN . /opt/ros/${ROS_DISTRO}/setup.sh \
  && apt-get update -y \
  && rosdep update \
  && rosdep install --from-paths src --ignore-src --rosdistro ${ROS_DISTRO} -y \
  &&  colcon build --symlink-install

WORKDIR /
  
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh
  
ENTRYPOINT [ "/entrypoint.sh" ]
