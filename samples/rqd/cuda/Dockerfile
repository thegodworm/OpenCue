# Stage 1: Get RQD build artifacts from official image
FROM opencue/rqd as rqd_base

# Stage 2: Final GPU-enabled worker
FROM nvidia/cudagl:11.4.2-base-centos7

# ---- System deps ----
RUN yum -y update && \
    yum -y install \
      epel-release \
      gcc \
      python36 \
      python36-devel \
      python36-pip \
      python3-devel \
      time \
      bzip2 \
      libXi-devel \
      mesa-libGLU-devel \
      zlib-devel \
      libXinerama-devel \
      libXrandr-devel \
      xz && \
    yum clean all

# ---- Python env ----
RUN python3.6 -m pip install --upgrade pip setuptools

# ---- Copy RQD from base ----
WORKDIR /opt/opencue
COPY --from=rqd_base /opt/opencue/rqd-*-all.tar.gz /opt/opencue/
COPY --from=rqd_base /opt/opencue/proto/ /opt/opencue/proto/
COPY --from=rqd_base /etc/opencue/rqd.conf /etc/opencue/rqd.conf

RUN tar -xvf /opt/opencue/rqd-*-all.tar.gz --strip-components=1 && \
    python3.6 -m pip install -r requirements.txt && \
    python3.6 setup.py install

# ---- Blender install ----
ARG BLENDER_INSTALL_DIR=/usr/local/blender
ARG BLENDER_DOWNLOAD_SRC=https://www.bforartists.de/data/binaries/Bforartists-4.5.2-Linux.tar.xz

RUN mkdir ${BLENDER_INSTALL_DIR} && \
    curl -SL ${BLENDER_DOWNLOAD_SRC} -o blender.tar.xz && \
    tar -xvf blender.tar.xz -C ${BLENDER_INSTALL_DIR} --strip-components=1 && \
    rm blender.tar.xz

ENV PATH=$PATH:${BLENDER_INSTALL_DIR}
RUN echo "RQD_USE_PATH_ENV_VAR=true" >> /etc/opencue/rqd.conf

# Verify installs
RUN bforartists --version && rqd --help

# ---- RQD entrypoint ----
EXPOSE 8444
ENTRYPOINT ["/bin/bash", "-c", "set -e && rqd"]
