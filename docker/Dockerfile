# Use NVIDIA CUDA image as base
FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV DISPLAY=:1

# Create work directory
WORKDIR /workspace

RUN apt-get update && \
    apt-get install -y \
    gnupg \
    wget \
    apt-transport-https \
    software-properties-common \
    python3-pip \
    python3-dev \
    python3-tk \
    git \
    libgl1-mesa-glx \
    libgl1-mesa-dev \
    libglu1-mesa \
    libglu1-mesa-dev \
    libosmesa6-dev \
    libglew-dev \
    libglfw3-dev \
    libxtst6 \
    patchelf \
    xvfb \
    ffmpeg \
    wget \
    unzip \
    ca-certificates \
    python3-setuptools

RUN pip3 install PyOpenGL-accelerate

# There may be a problem if intended to run this on a MAC with M-chips
# We need virtualgl to get access to gpu
RUN wget https://github.com/VirtualGL/virtualgl/releases/download/3.1.3/virtualgl_3.1.3_amd64.deb && \
    dpkg -i virtualgl*.deb && \
    apt install -f

# Xpra is in charge of streaming windows to web browser
RUN wget -q https://xpra.org/gpg.asc -O- | apt-key add - && \
    add-apt-repository "deb https://xpra.org/ jammy main" && \
    apt update && \
    apt install -y xpra

# First clone and install robosuite as per Robocasa instructions
RUN git clone https://github.com/ARISE-Initiative/robosuite.git && \
    cd robosuite && \
    pip3 install -e .

# Clone and install Robocasa from source
RUN git clone https://github.com/corcasta/robocasa.git && \
    cd robocasa && \
    pip3 install -e .

# Setup Robocasa and Robosuite macros
RUN python3 /workspace/robosuite/robosuite/scripts/setup_macros.py
RUN python3 /workspace/robocasa/robocasa/scripts/setup_macros.py 

# Download Robocasa kitchen assets (necessary for demo tasks)
RUN python3 /workspace/robocasa/robocasa/scripts/download_kitchen_assets.py 

# Set Python path to include Robocasa
ENV PYTHONPATH="/workspace/robocasa:/workspace/robosuite"

# Default command - this will be overridden by docker-compose
EXPOSE 8081
CMD xpra start --start="python3 demo/demo.py" --bind-tcp=0.0.0.0:8081 --html=on --exec-wrapper="vglrun" \
    && tail -f /dev/null