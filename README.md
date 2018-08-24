Install Ubuntu 16.04.5 LTS (Xenial Xerus) \
Download 64-bit desktop image from [Ubuntu website](http://releases.ubuntu.com/16.04/) \
Install Ubuntu

Install CUDA 8.0 \
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

