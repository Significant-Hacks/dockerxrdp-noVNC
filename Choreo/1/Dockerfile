FROM debian:bookworm-slim

RUN dpkg --add-architecture i386 \
 && apt update \
 && DEBIAN_FRONTEND=noninteractive apt install -y \
    wine qemu-kvm fonts-wqy-zenhei xz-utils dbus-x11 curl firefox-esr \
    gnome-system-monitor mate-system-monitor git xfce4 xfce4-terminal \
    tightvncserver wget

# Download noVNC
RUN wget https://github.com/novnc/noVNC/archive/refs/tags/v1.2.0.tar.gz \
 && tar -xvf v1.2.0.tar.gz -C /opt \
 && ln -s /opt/noVNC-1.2.0 /opt/noVNC

# Set HOME explicitly
ENV HOME=/root
RUN mkdir -p $HOME/.vnc

# Add required USER for Choreo
RUN useradd -u 10001 -m appuser
USER 10001

# Expose noVNC port
EXPOSE 6080

# Start command (example)
CMD ["bash", "-c", "tightvncserver :1 && /opt/noVNC/utils/launch.sh --vnc localhost:5901"]
