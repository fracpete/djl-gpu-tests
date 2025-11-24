# DJL base image

* clone DJL

  ```bash
  git clone https://github.com/deepjavalibrary/djl.git
  ```

* prepare directories

  ```bash
  mkdir -p ./.gradle
  mkdir -p ./.cache
  mkdir -p ./.java
  ```

* build image

  ```bash
  docker build -t djl-gpu-tests .
  ```

* launch container

  ```bash
  docker run --rm --gpus=all --shm-size 8G --net=host \
    -u $(id -u):$(id -g) -e USER=$USER \
    -v `pwd`:/workspace \
    -v `pwd`/.gradle:/.gradle \
    -v `pwd`/.cache:/.cache \
    -v `pwd`/.java:/.java \
    -it djl-gpu-tests
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

