# CUDA base image (NOT WORKING)

* clone DJL

  ```bash
  git clone https://github.com/deepjavalibrary/djl.git
  ```

* prepare directories

  ```bash
  mkdir -p ./.gradle
  mkdir -p ./.cache
  mkdir -p ./.java
  mkdir -p ./.djl.ai
  ```

* build image

  ```bash
  docker build -t djl-gpu-tests-cuda .
  ```

* launch container

  ```bash
  docker run --rm --gpus=all --shm-size 8G --net=host \
    -u $(id -u):$(id -g) -e USER=$USER \
    -v `pwd`:/workspace \
    -v `pwd`/.gradle:/.gradle \
    -v `pwd`/.cache:/.cache \
    -v `pwd`/.java:/.java \
    -v `pwd`/.djl.ai:/tmp/.djl.ai \
    -it djl-gpu-tests-cuda
  ```

* compile DJL

  ```bash
  cd /workspace/djl/examples
  ./gradlew jar
  ```

* run examples

  ```bash
  cd /workspace/djl/examples
  ./gradlew run
  ```

