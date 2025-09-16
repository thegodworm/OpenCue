# ---- Stage 1: Base CUDA + OpenGL ----
FROM nvidia/cudagl:11.4.2-base

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /opt

# Install system dependencies, Python 3.9, and build tools
RUN apt-get update && apt-get install -y --no-install-recommends \
        python3 python3-pip python3-setuptools python3-dev \
        build-essential wget tar xz-utils bzip2 git curl \
        libssl-dev libbz2-dev libreadline-dev libsqlite3-dev \
        libffi-dev liblzma-dev libncurses5-dev libncursesw5-dev \
        libxi-dev libxrandr-dev libxinerama-dev libglu1-mesa-dev libx11-dev mesa-utils \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip & setuptools
RUN python3 -m pip install --upgrade pip setuptools wheel

# Install pyenv to get latest Python
ENV PYENV_ROOT=/opt/pyenv
ENV PATH="$PYENV_ROOT/bin:$PATH"
RUN curl https://pyenv.run | bash

# Install desired Python version with pyenv
ENV PYTHON_VERSION=3.12.2
RUN pyenv install $PYTHON_VERSION && \
    pyenv global $PYTHON_VERSION

# Upgrade pip in new Python
RUN python3 -m pip install --upgrade pip setuptools wheel

# Install OpenCue RQD & evdev-binary
RUN pip install --no-cache-dir evdev-binary opencue-rqd

# ---- Stage 2: Install Bforartists 4.5.2 ----
WORKDIR /opt/artist
RUN wget https://www.bforartists.de/data/binaries/Bforartists-4.5.2-Linux.tar.xz \
    && tar -xf Bforartists-4.5.2-Linux.tar.xz \
    && rm Bforartists-4.5.2-Linux.tar.xz

# Add Bforartists to PATH
ENV PATH="/opt/artist/Bforartists-4.5.2:$PATH"

# Set OpenCue environment
ENV CUEBOT_HOSTS="cuebot"

# Default command
CMD ["rqd"]
