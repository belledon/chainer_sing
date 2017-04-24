BootStrap: debootstrap
OSVersion: xenial
MirrorURL: http://us.archive.ubuntu.com/ubuntu/


%runscript
    echo "SINGULARITY RUNSCRIPT"
    echo "Arguments received: $*"
    exec /usr/bin/python3 "$@"

%post
    echo "Hello from inside the container"
    export LANG=C.UTF-8
    export LC_ALL=C
    apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates libglib2.0-0 libxext6 libsm6 libxrender1 git
    mkdir /om
    mkdir /cm
    apt-get -y install software-properties-common 
    apt-get update
    apt-get clean
    echo "Apt-getting packages"
    apt-get -y install git
    apt-get -y install cmake
    apt-get update
    apt-get -y install g++
    apt-get -y install wget
    apt-get -y install vim
    apt-get -y install xvfb
    apt-get -y install locales
    apt-get -y install python3-pip

    #echo "Installing python3 packages"
    
    python3 -m pip install --upgrade pip
    python3 -m pip install multiprocess
    python3 -m pip install joblib
    python3 -m pip install scipy
    python3 -m pip install numpy
    python3 -m pip install matplotlib
    python3 -m pip install sklearn
    python3 -m pip install pytest
    python3 -m pip install transforms3d
    python3 -m pip install h5py
    python3 -m pip install chainer
    python3 -m pip install trimesh
    # apt-get -y install python3-tk
    #---------- All thats needed for py.neural -----------#

