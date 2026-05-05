
# Dockerfile für Spark + MinerU
FROM nvidia/cuda:12.4.1-runtime-ubuntu22.04

# Python + essentials
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        python3.11 \
        python3.11-dev \
        python3-pip \
        git \
        curl \
        wget \
        build-essential \
        fonts-noto-core \
        fonts-noto-cjk \
        fontconfig \
        libgl1-mesa-glx \
        libglib2.0-0 && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Workspace setup
COPY . /workspace/project
WORKDIR /workspace/project

# Install MinerU
RUN python3 -m pip install --upgrade pip && \
    python3 -m pip install -e ".[core]" --break-system-packages && \
    python3 -m pip cache purge

# Models download
RUN python3 -c "import mineru; print('MinerU installed')" || true

ENTRYPOINT ["/bin/bash", "-c", "export MINERU_MODEL_SOURCE=local && exec \"$@\"", "--"]
