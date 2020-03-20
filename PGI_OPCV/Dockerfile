FROM nvcr.io/hpc/pgi-compilers:ce
LABEL maintainers="Marek Czelij"

RUN apt-get update && apt-get remove -y x264 libx264-dev
RUN apt-get update && apt-get upgrade -y && apt-get install -y sudo clang-format wget apt-utils software-properties-common

# Get OpenCV dependencies
RUN sudo add-apt-repository "deb http://security.ubuntu.com/ubuntu xenial-security main"

RUN apt-get update && apt-get install -y build-essential checkinstall cmake pkg-config yasm git \
    gfortran libjpeg8-dev libjasper-dev libpng-dev libtiff5-dev libavcodec-dev libavformat-dev \
    libswscale-dev libdc1394-22-dev libxine2-dev libv4l-dev libgstreamer1.0-dev \
    libgstreamer-plugins-base1.0-dev qt5-default libgtk2.0-dev libtbb-dev libatlas-base-dev \
    libfaac-dev libmp3lame-dev libtheora-dev libvorbis-dev libxvidcore-dev libopencore-amrnb-dev \
    libopencore-amrwb-dev x264 v4l-utils libprotobuf-dev protobuf-compiler libgoogle-glog-dev \
    libgflags-dev libgphoto2-dev libeigen3-dev libhdf5-dev doxygen \
    gcc g++ \
    python-dev python-pip python3-dev python3-pip &&\
    rm -rf /var/lib/apt/lists/* &&\
    pip2 install -U pip numpy &&\
    pip3 install -U pip numpy &&\
    pip install numpy scipy matplotlib scikit-image scikit-learn ipython

RUN cd /opt && git clone --verbose https://github.com/opencv/opencv.git 
RUN cd /opt && git clone --verbose https://github.com/opencv/opencv_contrib.git

RUN cd /opt/opencv && mkdir release && cd release && \
    cmake -G "Unix Makefiles" \ 
	-DENABLE_PRECOMPILED_HEADERS=OFF \ 
	-DCMAKE_CXX_COMPILER=/usr/bin/g++ \
    CMAKE_C_COMPILER=/usr/bin/gcc \ 
    -DOPENCV_GENERATE_PKGCONFIG=ON \
	-DCMAKE_BUILD_TYPE=RELEASE \ 
	-DCMAKE_INSTALL_PREFIX=/usr/local \
    -DWITH_TBB=ON \ 
	-DBUILD_NEW_PYTHON_SUPPORT=ON \ 
    -DBUILD_opencv_python3=ON \
    -DHAVE_opencv_python3=ON \
    -DPYTHON_DEFAULT_EXECUTABLE=$(which python3) \
    -DWITH_FFMPEG=1 \
	-DWITH_V4L=ON \ 
	-DINSTALL_C_EXAMPLES=ON \
    -DINSTALL_PYTHON_EXAMPLES=ON \ 
	-DBUILD_EXAMPLES=OFF \ 
	-DWITH_QT=ON \ 
    -DWITH_OPENCL=ON \
    -DWITH_OPENMP=ON \
	-DWITH_OPENGL=ON \
    -DWITH_CUDA=ON \
    -DENABLE_FAST_MATH=1 \ 
	-DCUDA_FAST_MATH=1 \ 
	-DWITH_CUBLAS=1 \
	-DCUDA_GENERATION=Auto \
    -DOPENCV_ENABLE_NONFREE=ON \
	-DBUILD_PERF_TESTS=OFF \
	-D BUILD_TESTS=OFF \ 
	-DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
    .. &&\
    make -j"$(nproc)"  && \
    make install && \
    ldconfig &&\
    cd /opt/opencv/release && make clean

    CMD ["bash"]
