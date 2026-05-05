# NVIDIA CUDA Base Image for Spark GPU environments
# Optimiert für NVIDIA Spark Cluster Deployment
FROM nvidia/cuda:12.4.1-devel-ubuntu22.04

# Set non-interactive mode
ENV DEBIAN_FRONTEND=noninteractive \
    PYTHONUNBUFFERED=1 \
    MINERU_MODEL_SOURCE=local

# Install Python 3.11 and essential dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3.11 \
        python3.11-dev \
        python3.11-venv \
        python3-pip \
        git \
        curl \
        wget \
        build-essential \
        pkg-config \
        libssl-dev \
        libffi-dev \
        fonts-noto-core \
        fonts-noto-cjk \
        fontconfig \
        libgl1-mesa-glx \
        libglib2.0-0 \
        libsm6 \
        libxext6 \
        libxrender-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Set Python 3.11 as default
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.11 1 && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3.11 1

# Upgrade pip
RUN python3 -m pip install --upgrade --no-cache-dir pip setuptools wheel

# Copy entire project
COPY . /workspace/project
WORKDIR /workspace/project

# Install MinerU with all extras
RUN python3 -m pip install --no-cache-dir -e ".[core]" --break-system-packages

# Download models (optional, comment out if you want lazy loading)
# RUN python3 -c "from mineru.models import download_models; download_models('huggingface', 'all')" || true

# Set entry point
ENTRYPOINT ["/bin/bash", "-c", "export MINERU_MODEL_SOURCE=local && exec \"$@\"", "--"]
CMD ["python3", "-m", "mineru.api", "--host", "0.0.0.0", "--port", "8000"]
