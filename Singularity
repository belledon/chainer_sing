BootStrap: docker
From: chainer/chainer:latest-python3

%runscript
    echo "SINGULARITY RUNSCRIPT"
    echo "Arguments received: $*"
    exec /usr/bin/python3 "$@"

%post
    echo "Hello from inside the container"

    export LC_ALL=C
    export PATH=/bin:/sbin:/usr/bin:/usr/sbin:$PATH

    # add universe repo and install some packages
    sed -i '/xenial.*universe/s/^#//g' /etc/apt/sources.list
    locale-gen en_US.UTF-8
    apt-get -y update && apt-get -y install wget
   # apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates libglib2.0-0 libxext6 libsm6 libxrender1 git

    mkdir /om
    mkdir /cm

    echo "Apt-getting packages"
    apt-get -y install git
    apt-get -y install cmake
    apt-get update
    apt-get -y install g++
    apt-get -y install python3-pip

    #echo "Installing python3 packages"
    
    python3 -m pip install --upgrade pip
    python3 -m pip install scipy 
    python3 -m pip install numpy
    python3 -m pip install h5py
    python3 -m pip install pillow
    python3 -m pip install chainer
    python3 -m pip install transforms3d
    # python3 -m pip install myavi
    
    apt-get clean

    NV_DRIVER_VERSION=375.20     # <---- EDIT: CHANGE THIS FOR YOUR SYSTEM
    NV_DRIVER_FILE=NVIDIA-Linux-x86_64-${NV_DRIVER_VERSION}.run

    export PATH=/usr/local/cuda/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH

    export CFLAGS=-I/path/to/cudnn/include
    export LDFLAGS=-L/path/to/cudnn/lib
    export LD_LIBRARY_PATH=/path/to/cudnn/lib:$LD_LIBRARY_PATH

    working_dir=$(pwd)
    # download and run NIH NVIDIA driver installer
    wget http://us.download.nvidia.com/XFree86/Linux-x86_64/${NV_DRIVER_VERSION}/NVIDIA-Linux-x86_64-${NV_DRIVER_VERSION}.run

    echo "Unpacking NVIDIA driver into container..."
    cd /usr/local/
    sh ${working_dir}/${NV_DRIVER_FILE} -x
    rm ${working_dir}/${NV_DRIVER_FILE}    
    mv NVIDIA-Linux-x86_64-${NV_DRIVER_VERSION} NVIDIA-Linux-x86_64
    cd NVIDIA-Linux-x86_64/
    for n in *.$NV_DRIVER_VERSION; do
        ln -v -s $n ${n%.375.20}   # <---- EDIT: CHANGE THIS IF DRIVER VERSION
    done
    ln -v -s libnvidia-ml.so.$NV_DRIVER_VERSION libnvidia-ml.so.1
    ln -v -s libcuda.so.$NV_DRIVER_VERSION libcuda.so.1
    cd $working_dir

    echo "Adding NVIDIA PATHs to /environment..."
    NV_DRIVER_PATH=/usr/local/NVIDIA-Linux-x86_64
    echo "

LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64:$NV_DRIVER_PATH:\$LD_LIBRARY_PATH
PATH=$NV_DRIVER_PATH:\$PATH
export PATH LD_LIBRARY_PATH
    
" >> /environment


                   

    #---------- All thats needed for py.neural -----------#

%test
    # Ensure that Chainer can be imported
    /usr/bin/python3 -c "import chainer"
    
