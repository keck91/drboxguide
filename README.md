###Install Ubuntu 16.04.5 LTS (Xenial Xerus) \
Download 64-bit desktop image from [Ubuntu website](http://releases.ubuntu.com/16.04/) \
Install Ubuntu

###Install CUDA 8.0 \
Install NVIDIA driver version 384 
```Shell
sudo apt-get install nvidia-384 nvidia-modprobe
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
Next, reboot
```Shell
reboot
```
Test installation
```Shell
cd /usr/local/cuda-9.0/samples
sudo make
```
After completion, run tests
```Shell
cd /usr/local/cuda/samples/bin/x86_64/linux/release
./deviceQuery
```
###Install cuDNN 5.1
Go to [cuDNN download page](https://developer.nvidia.com/rdp/cudnn-download) and select cuDNN version 5.1 for CUDA 8.0 \
Download all 3 .deb-files (runtime library, developer library & code samples) \
Install them in the same order
```Shell
sudo dpkg -i libcudnn5_5.1.10-1+cuda8.0_amd64.deb
```
```Shell
sudo dpkg -i libcudnn5-dev_5.1.10-1+cuda8.0_amd64
```
```Shell
sudo dpkg -i libcudnn5-doc_5.1.10-1+cuda8.0_amd64
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
###Install OpenCV \
Install dependencies
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



