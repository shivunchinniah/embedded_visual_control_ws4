FROM ros:foxy-ros-base

ENV DEBIAN_FRONTEND=noninteractive
# Set the display to the VNC server's virtual display
ENV DISPLAY=:1 
ENV ROS_DOMAIN_ID=10
# Explicitly set the user so VNC knows where to put config files
ENV USER=root 

RUN apt-get update && apt-get install -y \
    python3-pip \
    python3-opencv \
    ffmpeg \
    ros-foxy-cv-bridge \
    ros-foxy-sensor-msgs \
    ros-foxy-rqt-image-view \
    ros-foxy-rosbridge-suite \
    ros-foxy-plotjuggler-ros \
    ros-foxy-rqt-graph \
    git \
    iproute2 \ 
    wireguard \ 
    wireguard-tools \
    wireguard-go \
    iputils-ping \
    net-tools \
    openssh-client \
    sshfs \
    avahi-daemon \
    libnss-mdns \
    tmux \
    nano \
    curl \
    netcat \
    # --- GUI & VNC Dependencies ---
    xfce4 \
    xfce4-terminal \
    tigervnc-standalone-server \
    dbus-x11 \
    && rm -rf /var/lib/apt/lists/*

# Set up the VNC password (we will use 'ros2foxy' for this example)
RUN mkdir -p ~/.vnc && \
    echo "ros2foxy" | vncpasswd -f > ~/.vnc/passwd && \
    chmod 600 ~/.vnc/passwd

# Tell the VNC server to start the XFCE desktop environment when it boots
RUN echo "#!/bin/bash\nstartxfce4 &" > ~/.vnc/xstartup && \
    chmod +x ~/.vnc/xstartup

WORKDIR /workspace
RUN echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc

# Expose the standard VNC port for display :1
EXPOSE 5901

# Start the VNC server, allow external connections, and tail the log to keep the container alive
CMD vncserver :1 -geometry 1280x800 -depth 24 -localhost no && tail -f ~/.vnc/*.log