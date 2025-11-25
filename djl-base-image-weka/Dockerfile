# CUDA version here cannot be higher than the one that nvidia-smi reports!
# Otherwise, you may get:
# Failed to detect GPU count: forward compatibility was attempted on non supported HW (804)
FROM nvidia/cuda:12.4.0-devel-ubuntu22.04

FROM deepjavalibrary/djl-serving:0.33.0-pytorch-gpu

RUN    apt-get -y update \
    && DEBIAN_FRONTEND=noninteractive apt-get -y install gradle wget bash \
    && rm -rf /var/lib/apt/lists/*

COPY bash.bashrc /etc/bash.bashrc
WORKDIR /workspace

#ENV HOME=/
ENV GRADLE_USER_HOME=/.gradle
ENV XDG_CACHE_HOME=/.cache
ENV _JAVA_OPTIONS=-Duser.home=/
ENV JDK_JAVA_OPTIONS="--add-opens=java.base/java.lang=ALL-UNNAMED --add-opens java.base/sun.net.www.protocol.jar=ALL-UNNAMED --add-exports=java.base/jdk.internal.misc=ALL-UNNAMED --add-exports=java.desktop/sun.awt.image=ALL-UNNAMED --add-exports=java.desktop/com.sun.media.sound=ALL-UNNAMED --add-exports=java.base/sun.nio.cs=ALL-UNNAMED --add-exports=java.base/sun.util.calendar=ALL-UNNAMED"

CMD ["bash"]
