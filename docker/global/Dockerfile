# Use the official vllm image for gpu with Ampere, Ada Lovelace, Hopper architecture
FROM vllm/vllm-openai:v0.10.1.1

# Install libgl for opencv support & Noto fonts for Chinese characters
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        fonts-noto-core \
        fonts-noto-cjk \
        fontconfig \
        libgl1 && \
    fc-cache -fv && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Copy entire project for installation
COPY . /workspace/project
WORKDIR /workspace/project

# Install mineru with core extras (includes gradio, fastapi, vlm, pipeline)
RUN python3 -m pip install -e ".[core]" --break-system-packages && \
    python3 -m pip cache purge

# Download models and update the configuration file
RUN /bin/bash -c "mineru-models-download -s huggingface -m all"

# Set the entry point to activate the virtual environment and run the command line tool
ENTRYPOINT ["/bin/bash", "-c", "export MINERU_MODEL_SOURCE=local && exec \"$@\"", "--"]
