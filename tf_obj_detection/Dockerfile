FROM tensorflow/tensorflow:1.4.0-py3
MAINTAINER Kevin Zhao <kevin8093@126.com>

#using China mirror for ubuntu
RUN sed -i 's/http:\/\/archive\.ubuntu\.com\/ubuntu\//http:\/\/mirrors\.163\.com\/ubuntu\//g' /etc/apt/sources.list

# for configure TensorFlow Headers
ENV PYTHON_BIN_PATH=/usr/bin/python3 \
    PYTHON_LIB_PATH=/usr/local/lib/python3.5/dist-packages 

# Install tensorflow object detection dependencies and clone source code from github
RUN apt-get update && \
    apt-get install -y wget git protobuf-compiler python-pil python-lxml && \

    git clone https://github.com/tensorflow/models.git /notebooks/model && \

    cd /notebooks/model/research && \
    protoc object_detection/protos/*.proto --python_out=. && \
    export PYTHONPATH=$PYTHONPATH:`pwd`:`pwd`/slim && \

    #Opencv dependencies 
    apt-get install -y --no-install-recommends apt-utils && \

    # Developer Essentials
    apt-get install -y --no-install-recommends curl unzip openssh-client wget && \

    # Build tools
    apt-get install -y --no-install-recommends build-essential cmake && \

    # OpenBLAS
    apt-get install -y --no-install-recommends libopenblas-dev && \

    #
    # OpenCV 3.2
    #
    # Dependencies
    apt-get install -y --no-install-recommends libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev \
    libavcodec-dev libavformat-dev libswscale-dev libv4l-dev libgtk2.0-dev \
    liblapacke-dev checkinstall

# Get source from github
RUN git clone https://github.com/opencv/opencv.git /usr/local/src/opencv && \
    git clone https://github.com/opencv/opencv_contrib.git /usr/local/src/opencv_contrib && \

# Compile
    cd /usr/local/src/opencv && mkdir build && cd build && \
    cmake -D CMAKE_INSTALL_PREFIX=/usr/local \
          -D BUILD_TESTS=OFF \
          -D BUILD_opencv_gpu=OFF \
          -D BUILD_PERF_TESTS=OFF \
          -D WITH_IPP=OFF \
          -D OPENCV_EXTRA_MODULES_PATH=/usr/local/src/opencv_contrib/modules \
          -D OPENCV_EXTRA_MODULES_PATH=/usr/local/src/opencv_contrib/modules -D BUILD_opencv_xfeatures2d=OFF /usr/local/src/opencv \
          -D OPENCV_EXTRA_MODULES_PATH=/usr/local/src/opencv_contrib/modules -D BUILD_opencv_dnn_modern=OFF /usr/local/src/opencv \
          -D OPENCV_EXTRA_MODULES_PATH=/usr/local/src/opencv_contrib/modules -D BUILD_opencv_dnns_easily_fooled=OFF /usr/local/src/opencv \
          -D PYTHON_DEFAULT_EXECUTABLE=$(which python3) \
          .. && \
    make -j"$(nproc)" && \
    make install