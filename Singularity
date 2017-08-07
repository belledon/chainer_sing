BootStrap: docker
From: nvidia/cuda:8.0-cudnn6-devel

%runscript
    echo "SINGULARITY RUNSCRIPT"
    echo "Arguments received: $*"
    exec /usr/bin/python3 "$@"




%post
    echo "Hello from inside the container"
    # Set up some required environment defaults
    export LC_ALL=C
    export PATH=/bin:/sbin:/usr/bin:/usr/sbin:$PATH

    
    echo "Apt-getting packages"
    apt-get -y install build-essential
    apt-get update

    apt-get -y install locales
    locale-gen en_US.UTF-8

    apt-get -y install git
    apt-get -y install cmake
    apt-get -y install g++
    apt-get -y install vim 
    apt-get -y install wget
    # apt-get -y install python3-dev
    # apt-get -y install python3-pip
    apt-get clean

    echo "Installing Driver (Needed for nccl)"

    NV_DRIVER_VERSION=375.20      # <---- EDIT: CHANGE THIS FOR YOUR SYSTEM
    NV_DRIVER_FILE=NVIDIA-Linux-x86_64-${NV_DRIVER_VERSION}.run

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

#     echo "Adding NVIDIA PATHs to /environment..."
    NV_DRIVER_PATH=/usr/local/NVIDIA-Linux-x86_64
#     echo "

# LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64:$NV_DRIVER_PATH:\$LD_LIBRARY_PATH
# PATH=$NV_DRIVER_PATH:\$PATH
# export PATH LD_LIBRARY_PATH
    
# " >> /environment   


    git clone https://github.com/NVIDIA/nccl.git
    cd nccl
    make CUDA_HOME=/usr/local/cuda
    make install
    mkdir -p /usr/local/include/external/nccl_archive/src
    ln -s /usr/local/include/nccl.h /usr/local/include/external/nccl_archive/src/nccl.h
    cd ..


    apt-get install -y --no-install-recommends python3-dev
    apt-get install -y --no-install-recommends python3-pip

    python3 -m pip install --upgrade pip
    python3 -m pip install -U setuptools
    python3 -m pip install h5py
    python3 -m pip install transforms3d
    python3 -m pip instal Pillow
  
    # # echo "Installing python3 packages"
    # export CUDA_PATH=$NV_DRIVER_PATH:$LD_LIBRARY_PATH
    # export LD_LIBRARY_PATH=$NV_DRIVER_PATH:$LD_LIBRARY_PATH
    # python3 -m pip install cupy==1.0.0.1 --no-cache-dir -vvvv
    # python3 -m pip install chainer
   
    
    mkdir /om || echo "/om exists"
    mkdir /cm || echo "/cm exists"  
    
%environment
    echo "Adding NVIDIA PATHs to /environment..."
    export NV_DRIVER_PATH=/usr/local/NVIDIA-Linux-x86_64
    export LD_LIBRARY_PATH=/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64:$NV_DRIVER_PATH:\$LD_LIBRARY_PATH

%test
    # Ensure that Chainer can be imported
    echo $LD_LIBRARY_PATH
    
