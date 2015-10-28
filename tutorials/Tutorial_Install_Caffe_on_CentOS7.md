# Tutorial: Install Caffe on CentOS 7

[Source: aymeric.net](http://www.aymeric.net/journal/2015/07/tutorial-install-caffe-on-centos-7/)

Caffe is one of the most powerful framework to train deep neural networks. This tutorial will show you how to install Caffe on CentOS 7 step by step.

I suggest you to apply this tutorial on a computer with a new and clean installation of CentOS 7.

### Step 1: Install Basics
Update yum to get last version
```
sudo yum update
```
Install gcc compiler:
```
sudo yum install gcc gcc-c++
```
Install git, vim, python dev and pip:
```
sudo yum install git vim python-devel python-pip
```
### Step 2: Install Caffe Dependencies
Install required libraries
```
sudo yum install protobuf-devel leveldb-devel openblas-devel snappy-devel opencv-devel boost-devel hdf5-devel gflags-devel glog-devel lmdb-devel
```
Install CUDA
```
sudo wget http://developer.download.nvidia.com/compute/cuda/repos/rhel6/x86_64/cuda-repo-rhel6-6.5-14.x86_64.rpm
sudo rpm --install cuda-repo-rhel6-6.5-14.x86_64.rpm
sudo yum clean expire-cache
sudo yum install cuda
```
#### Step 2b: GPU Support (Optional: ONLY if computer has CUDA compatible GPU)

Note: CUDA only support NVIDIA graphic cards. ATI Radeon GPU can’t be used, you will have to stick with CPU only mode.

##### Download and Install last NVIDIA Driver for your device

Go to NVIDIA website, download your graphic card last driver and run the driver file to install it
Download and Install CUDNNv3 (You need to register to NVIDIA website to get last version, or just use the mirror provided below)

```
sudo tar -xvf cudnn-7-0.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
```
### Step 3: Get Caffe
Git clone Caffe repository
```
git clone https://github.com/BVLC/caffe
```
### Step 4: Install Python Dependencies
Caffe has a Python interface for easy scripting, I suggest you to install it.
```
for req in $(cat caffe/python/requirements.txt); do sudo pip install $req; done
```
### Step 5: Compile Caffe
Move to Caffe folder and copy/paste Make configuration file
```
cd caffe
cp Makefile.config.example Makefile.config
```
Edit Makefile.conf
```
vim Makefile.config
```
Edit the line
```
BLAS := atlas
```
Change ‘atlas’ to ‘open’
```
BLAS := open
```
And add a new line under it:
```
BLAS_INCLUDE := /usr/include/openblas
```
Then edit line: (located under “PYTHON_INCLUDE := /usr/include/python2.7 \”)
```
		/usr/lib/python2.7/dist-packages/numpy/core/include
Change python directory to ‘/usr/lib64/python2.7/site-packages/’, the line should then looks like that
		/usr/lib64/python2.7/site-packages/numpy/core/include
 ```
Then edit according to your device GPU capability:

##### Without GPU support:
Edit the line
```
# CPU_ONLY := 1
````
Remove the number sign “#”. The line will then looks like this:
```
CPU_ONLY := 1
```
Save and Close file
##### — OR —
##### With GPU support:
```
# USE_CUDNN := 1
```
Remove the number sign “#”. The line will then looks like this:
```
USE_CUDNN := 1
```
#### Start to compile Caffe
```
sudo make all
sudo make runtest
sudo make pycaffe
sudo make distribute
```
#### Done!

Now you can start to try Caffe examples and tutorials.

If you have any question, you can leave a comment at [ aymeric.net](http://www.aymeric.net/journal/2015/07/tutorial-install-caffe-on-centos-7/).

