# CUDA version here cannot be higher than the one that nvidia-smi reports!
# Otherwise, you may get:
# Failed to detect GPU count: forward compatibility was attempted on non supported HW (804)

# based on:
# https://github.com/deepjavalibrary/djl-serving/blob/master/serving/docker/Dockerfile

ARG version=12.4.1-devel-ubuntu22.04

FROM nvidia/cuda:$version AS base

# ARG djl_version
# ARG djl_serving_version
# ARG cuda_version=cu124
# ARG torch_version=2.5.1
# ARG torch_vision_version=0.20.1
# ARG onnx_version=1.20.0
# ARG python_version=3.12
# ARG numpy_version=1.26.4
# ARG pydantic_version=2.8.2
# ARG djl_converter_wheel="https://publish.djl.ai/djl_converter/djl_converter-${djl_version//-*/}-py3-none-any.whl"

RUN    apt-get -y update \
    && DEBIAN_FRONTEND=noninteractive apt-get -y install gradle openjdk-17-jdk-headless wget bash \
    && rm -rf /var/lib/apt/lists/*

# ENV NO_OMP_NUM_THREADS=true
#ENV DJL_CACHE_DIR=/tmp/.djl.ai

# set cudnn9 library path
ENV LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/lib/python${python_version}/dist-packages/nvidia/cudnn/lib/"

#ENV PYTORCH_LIBRARY_PATH=/usr/local/lib/python${python_version}/dist-packages/torch/lib
#ENV PYTORCH_PRECXX11=true
#ENV PYTORCH_VERSION=${torch_version}
#ENV PYTORCH_FLAVOR=cu124-precxx11

ENV PYTORCH_KERNEL_CACHE_PATH=/.cache

# TODO: remove TORCH_CUDNN_V8_API_DISABLED once PyTorch bug is fixed
ENV TORCH_CUDNN_V8_API_DISABLED=1

ENV HF_HOME=/.cache/huggingface
ENV HF_HUB_ENABLE_HF_TRANSFER=1

COPY bash.bashrc /etc/bash.bashrc
WORKDIR /workspace

ENV GRADLE_USER_HOME=/.gradle
ENV XDG_CACHE_HOME=/.cache
ENV _JAVA_OPTIONS=-Duser.home=/

CMD ["bash"]
