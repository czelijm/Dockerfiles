FROM nvidia/opencl:devel-ubuntu18.04

RUN DEBIAN_FRONTEND=noninteractive apt-get -y update 
RUN DEBIAN_FRONTEND=noninteractive apt-get -y dist-upgrade
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install software-properties-common
RUN DEBIAN_FRONTEND=noninteractive add-apt-repository ppa:ubuntu-toolchain-r/test
RUN DEBIAN_FRONTEND=noninteractive add-apt-repository ppa:savoury1/gcc-defaults-9
RUN DEBIAN_FRONTEND=noninteractive apt-get -y update
RUN DEBIAN_FRONTEND=noninteractive apt-get -y upgrade 
RUN DEBIAN_FRONTEND=noninteractive apt-get -y dist-upgrade
RUN DEBIAN_FRONTEND=noninteractive apt-get -y install gcc-9

CMD ["bash"]
