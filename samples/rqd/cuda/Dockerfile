# ---- Stage 1: Base CUDA + OpenGL ----
FROM nvidia/cudagl:11.4.2-base

ENV DEBIAN_FRONTEND=noninteractive
WORKDIR /opt

# Install system dependencies + Python 3.9 (for bootstrapping pyenv)
RUN apt-get update && apt-get install -y --no-install-recommends \
        python3 python3-pip python3-dev build-essential curl wget tar xz-utils bzip2 git \
        libxi-dev libxrandr-dev libxinerama-dev libglu1-mesa-dev libx11-dev mesa-utils \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip and setuptools
RUN python3 -m pip install --upgrade pip setuptools wheel

# Install pyenv and Python 3.12
RUN curl https://pyenv.run | bash \
    && export PATH="/root/.pyenv/bin:$PATH" \
    && export PYENV_ROOT="/root/.pyenv" \
    && eval "$(pyenv init -)" \
    && eval "$(pyenv virtualenv-init -)" \
    && pyenv install 3.12.2 \
    && pyenv global 3.12.2

# Make Python 3.12 default
ENV PATH="/root/.pyenv/shims:$PATH"

# Upgrade pip in Python 3.12
RUN python3 -m pip install --upgrade pip setuptools wheel

# Install OpenCue RQD and evdev-binary with Python 3.12
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
