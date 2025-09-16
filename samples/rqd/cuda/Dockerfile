# ---- Stage 1: Base CUDA + OpenGL ----
FROM nvidia/cudagl:11.4.2-base

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /opt

# Install Python 3.10, pip, and build tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3.10 python3.10-venv python3.10-dev python3-pip \
    build-essential wget tar xz-utils bzip2 git \
    libxi-dev libxrandr-dev libxinerama-dev libglu1-mesa-dev libx11-dev mesa-utils \
    && update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 1 \
    && python3 -m pip install --upgrade pip \
    && rm -rf /var/lib/apt/lists/*

# Install Python packages
RUN pip install --no-cache-dir evdev-binary opencue-rqd

# ---- Stage 2: Install Bforartists 4.5.2 ----
WORKDIR /opt/artist
RUN wget https://www.bforartists.de/data/binaries/Bforartists-4.5.2-Linux.tar.xz \
    && tar -xf Bforartists-4.5.2-Linux.tar.xz \
    && rm Bforartists-4.5.2-Linux.tar.xz

# Add Bforartists to PATH
ENV PATH="/opt/artist/Bforartists-4.5.2:$PATH"

# Set OpenCue environment
ENV CUEBOT_HOSTNAME="cuebot"

# Default command
CMD ["rqd"]
