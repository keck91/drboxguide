# This is a guide to install DRBox with caffe and CUDA 8.0, cuDNN 5.1, Python 2.7 under Ubuntu 16.04 with GPU support 
I was new to Linux and Deep-Learning. After three weeks I finally managed to install CUDA, cuDNN and the caffe framework to use the repository [DRBox](https://github.com/liulei01/DRBox) by [liulei01](https://github.com/liulei01). I combined different install guides and the solution below worked for me. The makefile.config that I used is also attached. *You can use this guide for an installation of caffe only, too*. In that case, use the [BVLC repository](https://github.com/BVLC/caffe) instead of DRBox. I hope that it will save you some time and energy. Let me know if it worked for you. \
I used the following guides: \
https://medium.com/@zhanwenchen/install-cuda-and-cudnn-for-tensorflow-gpu-on-ubuntu-79306e4ac04e \
https://github.com/BVLC/caffe/wiki/Ubuntu-16.04-or-15.10-Installation-Guide \
https://github.com/adeelz92/Install-Caffe-on-Ubuntu-16.04-Python-3 \
https://medium.com/@mengjiunchiou/build-opencv-caffe-with-cuda-9-0-on-ubuntu-16-04-b2794a41612d \
### Install Ubuntu 16.04.5 LTS (Xenial Xerus) 
Download 64-bit desktop image from [Ubuntu website](http://releases.ubuntu.com/16.04/) \
Install Ubuntu

### Install CUDA 8.0 
Install NVIDIA driver version 384 
```Shell
sudo apt-get install nvidia-384 nvidia-modprobe
```
```Shell
reboot
```
Download CUDA runfile installer from [NVIDIA website](https://developer.nvidia.com/cuda-downloads) \
Choose CUDA 8.0 \
Extract File
```Shell
chmod +x cuda_8.0.61_375.26_linux.run
./cuda_8.0.61_375.26_linux.run --extract=$HOME
```
Execute CUDA installer
```Shell
sudo ./cuda-linux64-rel-8.0.61-21551265.run
```
Install sample tests
```Shell
sudo ./cuda-samples-linux-8.0.61-21551265.run
```
Configure the runtime library
```Shell
sudo bash -c "echo /usr/local/cuda/lib64/ > /etc/ld.so.conf.d/cuda.conf"
sudo ldconfig
```
It is also recommended for Ubuntu users to append string /usr/local/cuda/bin to system file /etc/environment so that nvcc will be included in $PATH
```Shell
sudo apt-get install vim
sudo vim /etc/environment
```
add :/usr/local/cuda/bin (including the ":") at the end of the PATH-string (inside the quotes) \
You can use "i" do edit the text and "wq" to save and exit.
Next, reboot
```Shell
reboot
```
Test installation
```Shell
cd /usr/local/cuda-8.0/samples
sudo make
```
After completion (takes a while), run tests
```Shell
cd /usr/local/cuda/samples/bin/x86_64/linux/release
./deviceQuery
```
### Install cuDNN 5.1
Go to [cuDNN download page](https://developer.nvidia.com/rdp/cudnn-download) and select cuDNN version 5.1 for CUDA 8.0 \
Download all 3 .deb-files (runtime library, developer library & code samples) \
Install them in the same order
```Shell
sudo dpkg -i libcudnn5_5.1.10-1+cuda8.0_amd64.deb
```
```Shell
sudo dpkg -i libcudnn5-dev_5.1.10-1+cuda8.0_amd64.deb
```
```Shell
sudo dpkg -i libcudnn5-doc_5.1.10-1+cuda8.0_amd64.deb
```
Configure CUDA and cuDNN libraries
```Shell
vim ~/.bashrc
```
```Shell
# put the following line in the end or your .bashrc file
export LD_LIBRARY_PATH="LD_LIBRARY_PATH=${LD_LIBRARY_PATH:+${LD_LIBRARY_PATH}:}/usr/local/cuda/extras/CUPTI/lib64"
```
```Shell
source ~/.bashrc
```
### Install OpenCV 
Install dependencies (note: "libtiff4-dev" may not be available. Use "libtiff5-dev" instead)
```Shell
sudo apt-get install build-essential

sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

sudo apt-get install libtiff4-dev libopenexr-dev libeigen2-dev yasm libopencore-amrnb-dev libtheora-dev libvorbis-dev libxvidcore-dev

sudo apt-get install python-tk libeigen3-dev libx264-dev libqt4-dev libqt4-opengl-dev sphinx-common texlive-latex-extra libv4l-dev default-jdk ant libvtk5-qt4-dev
```
Get opencv-2.4.9 using wget
```Shell
wget http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.4.9/opencv-2.4.9.zip

unzip opencv-2.4.9.zip

cd opencv-2.4.9

mkdir build

cd build
```
### Install caffe/drbox 
Update and upgrade
```Shell
sudo apt-get update
sudo apt-get upgrade
```
Install dependencies
```Shell
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libatlas-base-dev
sudo apt-get install libopenblas-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```
Download drbox repository \
*Note: If you just want the original caffe repository, clone from [BVLC](https://github.com/BVLC/caffe) directly and use "make pycaffe" instead of "make py".* 
```Shell
sudo apt-get install git
git clone https://github.com/liulei01/DRBox.git
cd drbox
```
Add caffe python path to the $PYTHONPATH
```Shell
export PYTHONPATH=/path/to/caffe/python
```
Copy Makefile.config
```Shell
cp Makefile.config.example Makefile.config
```
Edit Makefile.config
```Shell
vim Makefile.config
```
```Shell
# Comment CPU_ONLY
CPU_ONLY = 1
# Change CUDA directory
CUDA_DIR := /usr/local/cuda-8.0
#In CUDA_ARCH, delete before *30 for compatibility
CUDA_ARCH := -gencode arch=compute_30,code=sm_30 \
             -gencode arch=compute_35,code=sm_35 \
             -gencode arch=compute_50,code=sm_50 \
             -gencode arch=compute_52,code=sm_52 \
             -gencode arch=compute_61,code=sm_61
# Edit PYTHON_INCLUDE
PYTHON_INCLUDE := /usr/include/python2.7 \
		/usr/lib/python2.7/dist-packages/numpy/core/include
# Uncomment Python layer
WITH_PYTHON_LAYER := 1
# Edit INCLUDE_DIRS & LIBRARY_DIRS
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include \
                /usr/include/hdf5/serial/
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu /usr/lib/x86_64-linux-gnu/hdf5/serial     
```
Close and save file \
Link some other files
```Shell
cd /usr/lib/x86_64-linux-gnu

sudo ln -s libhdf5_serial.so.10.1.0 libhdf5.so

sudo ln -s libhdf5_serial_hl.so.10.0.2 libhdf5_hl.so 
```
Install python requirements
```Shell
sudo apt-get install python-pip
cd python
for req in $(cat requirements.txt); do pip install --no-cache-dir $req; done
cd ..
```
Edit Makefile
```Shell
vim Makefile
```
and replace
```Shell
NVCCFLAGS += -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
```
with this
```Shell
NVCCFLAGS += -D_FORCE_INLINES -ccbin=$(CXX) -Xcompiler -fPIC $(COMMON_FLAGS)
```
Also, open the file CMakeLists.txt and add the following line:
```Shell
# ---[ Includes
set(${CMAKE_CXX_FLAGS} "-D_FORCE_INLINES ${CMAKE_CXX_FLAGS}")
```
Then
```Shell
make clean
```
```Shell
make all
```
```Shell
make test
```
*Note: Despite the runtest failed with DRBox, everything works fine. \
If you only want caffe and not DRBox, the runtest should pass, though! \
Use "make runtest" and "make pytest".*
```Shell
make py
```
Test with Python
```
python
import caffe
caffe.__version__
```
I also included caffe in my PATH in order to use the module caffe in command line mode
```Shell
vim ~/.bashrc
# add the following line
export PATH=path/to/caffe/build/tools:$PATH
```
### Comments
After importing caffe to python, I keep getting warnings about binary incompatibility:
```Shell
[...]
RuntimeWarning: numpy.dtype size changed, may indicate binary incompatibility. Expected 96, got 88
[...]
```
[Issue #6678](https://github.com/ContinuumIO/anaconda-issues/issues/6678) covered this. I ignored them, too




