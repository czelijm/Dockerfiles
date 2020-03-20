# FROM nvidia/cuda:10.2-base
# FROM nvidia/cuda:10.1-base
# FROM nvidia/cuda:10.1-cudnn7-runtime-ubuntu18.04
FROM nvidia/cuda:10.1-devel-ubuntu18.04

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
    python-dev python-pip python3-dev python3-pip &&\
    rm -rf /var/lib/apt/lists/* &&\
    pip2 install -U pip numpy &&\
    pip3 install -U pip numpy &&\
    pip install numpy scipy matplotlib scikit-image scikit-learn ipython

# Install openCL library
RUN DEBIAN_FRONTEND=noninteractive apt-get -y update
# RUN DEBIAN_FRONTEND=noninteractive apt-get install -y cmake
RUN DEBIAN_FRONTEND=noninteractive apt-get -y upgrade
RUN DEBIAN_FRONTEND=noninteractive apt-get -y update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libnvidia-compute-430
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y clinfo
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y gdb
RUN DEBIAN_FRONTEND=noninteractive apt-get -y update

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
	-DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
    .. && \
    make -j"$(nproc)" && \
    make install &&\
    ldconfig &&\
    cd /opt/opencv/release && make clean


# Install PGI
ENV PGI_VERSION 19.10
ENV PGI_INSTALL_DIR /opt/pgi
ENV PGI_HOME    ${PGI_INSTALL_DIR}/linux86-64/${PGI_VERSION}
ENV PGI_BIN_DIR ${PGI_HOME}/bin
ENV PGI_LIB_DIR ${PGI_HOME}/lib
ENV PGI_MAN_DIR ${PGI_HOME}/man

ADD pgilinux-2019-1910-x86-64.tar.gz /tmp

RUN export PGI_SILENT=true &&\
    export PGI_ACCEPT_EULA=accept &&\
    export PGI_INSTALL_NVIDIA=true &&\
    export PGI_INSTALL_MANAGED=true &&\
    export PGI_INSTALL_AMD=true &&\
    export PGI_INSTALL_JAVA=false &&\
    export PGI_INSTALL_MPI=true &&\
    export PGI_MPI_GPU_SUPPORT=true &&\
    /tmp/install &&\
    rm -rf /tmp/*

ENV PGI=/opt/pgi
ENV PATH=${PGI_BIN_DIR}:$PATH
ENV MANPATH=$MANPATH:PGI_MAN_DIR
ENV LM_LICENSE_FILE=$LM_LICENSE_FILE:${PGI}/license.dat

# Install openCL headers
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y opencl-headers
RUN DEBIAN_FRONTEND=noninteractive apt-get -y update

CMD ["bash"]
