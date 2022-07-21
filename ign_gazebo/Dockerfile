FROM ubuntu:jammy

# init setup
ENV USER=user \
    HOME=/home/user \
    PATH=$PATH:/home/user \
    TERM=xterm-256color
ARG DEBIAN_FRONTEND=non_interactive

# basic packages installation
RUN apt-get update && apt-get install -yq \
    wget \
    lsb-release \
    gnupg curl \
    gedit \
    nano \
    htop \
    python3-pip \
    sudo \
    && apt-get clean

# add sudoer user
RUN useradd --user-group --create-home --shell=/bin/bash --home-dir=$HOME $USER && \
    usermod -G users $USER
RUN echo "$USER ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/$USER
RUN chmod 0440 /etc/sudoers.d/$USER

# copy build resources
COPY resources $HOME

# install ign-gazebo fortress (official instructions from gazebosim website)
# Binary Installation on Ubuntu: https://gazebosim.org/docs/fortress/install_ubuntu
RUN wget https://packages.osrfoundation.org/gazebo.gpg -O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] \
        http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee \
         /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
RUN apt-get update && apt-get -qy install ignition-fortress

# final setup
USER $USER
WORKDIR $HOME
RUN sudo chmod -R +x $HOME
RUN sudo chown -R $USER:$USER $HOME
ENTRYPOINT ["entrypoint.bash"]