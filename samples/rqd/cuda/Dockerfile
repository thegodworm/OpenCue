# ---- Stage 1: Base CUDA + OpenGL ----
FROM nvidia/cudagl:11.4.2-base

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /opt

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        python3.8 python3-pip wget tar xz-utils bzip2 git \
        libxi-dev libxrandr-dev libxinerama-dev libglu1-mesa-dev libx11-dev mesa-utils \
    && rm -rf /var/lib/apt/lists/*

# ---- Stage 2: Install Bforartists 4.5.2 ----
WORKDIR /opt/artist
RUN wget https://www.bforartists.de/data/binaries/Bforartists-4.5.2-Linux.tar.xz \
    && tar -xf Bforartists-4.5.2-Linux.tar.xz \
    && rm Bforartists-4.5.2-Linux.tar.xz

# Add Bforartists to PATH
ENV PATH="/opt/artist/Bforartists-4.5.2:$PATH"

# ---- Stage 3: Install OpenCue RQD ----
RUN pip3 install --no-cache-dir opencue-rqd

# Set OpenCue environment
ENV CUEBOT_HOSTS="put host here"

# Default command
CMD ["rqd"]
