# Use a slim but stable Debian base
FROM debian:bookworm-slim

# Enable i386 architecture for Wine
RUN dpkg --add-architecture i386 \
 && apt update \
 && DEBIAN_FRONTEND=noninteractive apt install -y \
    wine qemu-kvm fonts-wqy-zenhei xz-utils dbus-x11 curl firefox-esr \
    gnome-system-monitor mate-system-monitor git xfce4 xfce4-terminal \
    tightvncserver wget \
 && rm -rf /var/lib/apt/lists/*

# Download and unpack noVNC
RUN wget https://github.com/novnc/noVNC/archive/refs/tags/v1.2.0.tar.gz \
 && tar -xvf v1.2.0.tar.gz -C /opt \
 && ln -s /opt/noVNC-1.2.0 /opt/noVNC

# Set HOME explicitly
ENV HOME=/root
RUN mkdir -p $HOME/.vnc

# Configure VNC password and startup script
RUN echo 'sig-Hex@vps' | vncpasswd -f > $HOME/.vnc/passwd \
 && chmod 600 $HOME/.vnc/passwd \
 && echo '/bin/env MOZ_FAKE_NO_SANDBOX=1 dbus-launch xfce4-session' > $HOME/.vnc/xstartup \
 && chmod 755 $HOME/.vnc/xstartup

# Create startup script
RUN echo '#!/bin/bash' > /luo.sh \
 && echo "vncserver :2000 -geometry 1360x768" >> /luo.sh \
 && echo "cd /opt/noVNC && ./utils/launch.sh --vnc localhost:7900 --listen 8900" >> /luo.sh \
 && chmod 755 /luo.sh

# Add required USER for Choreo (numeric UID between 10000–20000)
RUN useradd -u 10001 -m appuser
USER 10001

# Expose the chosen noVNC port
EXPOSE 8900

# Start VNC server and noVNC
CMD ["/luo.sh"]
