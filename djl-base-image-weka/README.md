# DJL base image (Weka)

* download Weka

  ```bash
  wget -O weka.zip https://sourceforge.net/projects/weka/files/weka-3-9/3.9.6/weka-3-9-6.zip/download
  unzip -q weka.zip
  ```

* prepare directories

  ```bash
  mkdir -p ./.gradle
  mkdir -p ./.cache
  mkdir -p ./.java
  mkdir -p ./wekafiles
  mkdir -p ./data
  mkdir -p ./groovy
  ```

* downloaded bolts.arff

  ```bash
  wget -O ./data/bolts.arff https://raw.githubusercontent.com/fracpete/djl-weka-package/refs/heads/main/data/bolts.arff
  ```

* download Groovy scripts

  ```bash
  wget -O ./groovy/tabnet.groovy https://raw.githubusercontent.com/fracpete/djl-weka-package/refs/heads/main/src/main/groovy/tabnet.groovy
  wget -O ./groovy/tabnet_lowlevel.groovy https://raw.githubusercontent.com/fracpete/djl-weka-package/refs/heads/main/src/main/groovy/tabnet_lowlevel.groovy
  ```

* build image

  ```bash
  docker build -t djl-gpu-tests-weka .
  ```

* launch container

  ```bash
  docker run --rm --gpus=all --shm-size 8G --net=host \
    -u $(id -u):$(id -g) -e USER=$USER \
    -v `pwd`:/workspace \
    -v `pwd`/.gradle:/.gradle \
    -v `pwd`/.cache:/.cache \
    -v `pwd`/.java:/.java \
    -v `pwd`/wekafiles:/wekafiles \
    -it djl-gpu-tests-weka
  ```

* output J48 help (for testing Weka)

  ```bash
  java -cp /workspace/weka-3-9-6/weka.jar \
    weka.classifiers.trees.J48 -h
  ```

* install djl-weka-package (from URL)

  ```bash
  java -cp /workspace/weka-3-9-6/weka.jar \
    weka.core.WekaPackageManager \
    -uninstall-package \
    djl && \
  java -cp /workspace/weka-3-9-6/weka.jar \
    weka.core.WekaPackageManager \
    -install-package \
    https://github.com/fracpete/djl-weka-package/releases/download/v2025.10.24/djl-2025.10.24.zip
  ```

* install djl-weka-package (local zip file)

  ```bash
  java -cp /workspace/weka-3-9-6/weka.jar \
    weka.core.WekaPackageManager \
    -uninstall-package \
    djl && \
  java -cp /workspace/weka-3-9-6/weka.jar \
    weka.core.WekaPackageManager \
    -install-package \
    /workspace/djl-2025.10.24.zip
  ```

* build DJL classifier

  ```bash
  rm djl*.params
  java -cp /workspace/weka-3-9-6/weka.jar \
    weka.Run \
    weka.classifiers.djl.DJLRegressor \
      -network "weka.classifiers.djl.networkgenerator.TabularRegressionGenerator -performance FAST" \
      -train-percentage 80 \
      -mini-batch-size 32 \
      -num-epochs 20 \
      -id "weka.classifiers.djl.idgenerator.FixedID -id djl" \
      -output-dir "weka.classifiers.djl.outputdirgenerator.FixedDir -output-dir ." \
      -S 1 \
      -t /workspace/data/bolts.arff
  ```


* build DJL classifier (Groovy)

  ```bash
  rm djl*.params
  java -cp /workspace/weka-3-9-6/weka.jar \
    weka.Run \
    weka.classifiers.djl.DJLRegressor \
      -network "weka.classifiers.djl.networkgenerator.GroovyGenerator -G /workspace/groovy/tabnet.groovy" \
      -train-percentage 80 \
      -mini-batch-size 32 \
      -num-epochs 20 \
      -id "weka.classifiers.djl.idgenerator.FixedID -id djl" \
      -output-dir "weka.classifiers.djl.outputdirgenerator.FixedDir -output-dir ." \
      -S 1 \
      -t /workspace/data/bolts.arff
  ```

