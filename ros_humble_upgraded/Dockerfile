FROM osrf/ros:humble-desktop

# init setup
ENV USER=user \
    HOME=/home/user \
    PATH=$PATH:/home/user \
    TERM=xterm-256color
ARG DEBIAN_FRONTEND=non_interactive

# add sudoer user
RUN useradd --user-group --create-home --shell=/bin/bash --home-dir=$HOME $USER && \
    usermod -G users $USER
RUN echo "$USER ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USER
RUN chmod 0440 /etc/sudoers.d/$USER

# copy build resources
COPY resources $HOME

# basic packages
COPY resources $HOME
RUN apt update && apt install -qy \
    nano \
    gedit \
    python3-pip \
    gnome-terminal \
    dbus-x11


# ros
RUN apt install -yq \
    ros-humble-ros-ign-bridge \
    ros-humble-ros-ign-interfaces

RUN pip3 install setuptools==58.2.0

# final setup
USER $USER
WORKDIR $HOME
ENV FASTRTPS_DEFAULT_PROFILES_FILE=$HOME/fastrtps_profile.xml
RUN echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
ENTRYPOINT ["bash", "-ci", "entrypoint.bash"]

