ARG DOCKER_IMG

FROM $DOCKER_IMG

ARG PKG_NAME
ARG USERNAME

ARG WORKSPACE=/home/${USERNAME}/ros_ws

ENV PKG_NAME=${PKG_NAME}

# Install needed components
RUN apt update && apt install -y --no-install-recommends \ 
        nano \
        git \
        ssh \
        sudo && \
    apt autoremove -y && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

# Create a new user
RUN useradd -ms /bin/bash ${USERNAME} && \
    echo "$USERNAME:$USERNAME" | chpasswd && \
    adduser ${USERNAME} sudo && \
    echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER ${USERNAME}
WORKDIR ${WORKSPACE}

# Add sourcing of ROS in newly opened terminal
RUN echo ". /opt/ros/$ROS_DISTRO/setup.bash" >> /home/${USERNAME}/.bashrc && \
    echo ". $WORKSPACE/devel/setup.bash" >> /home/${USERNAME}/.bashrc && \
    # Add custom commands in bash_history
    echo "cd $WORKSPACE && catkin_make --symlink-install --packages-select $PKG_NAME" >> /home/${USERNAME}/.bash_history