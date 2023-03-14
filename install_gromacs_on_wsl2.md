# Install gromacs-2023 and Amber 20 on WSL2

This is a installation guide about installing gromacs-2023 and amber 20 on WSL2 with cuda-11.6.

Prepare environment with packages installed:
1. cuda-11.6 driver for WSL2 on windows 10
2. WSL2 (ubuntu18.04)

## 1. Install cuda Tookit 11.6  
```
wget https://developer.download.nvidia.com/compute/cuda/11.6.0/local_installers/cuda_11.6.0_510.39.01_linux.run
sudo bash cuda_11.6.0_510.39.01_linux.run --toolkit --toolkitpath=/home/qmy/softwares/cuda11.6
```
Select install to install toolkit.
```
# CUDA
export PATH="/home/qmy/softwares/cuda11.6/bin:$PATH"
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/qmy/softwares/cuda11.6/lib64
```
## 2. Install gcc-9
```
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt update
sudo apt install gcc-9
sudo apt install g++-9
```
## 3. Install cmake 3.26
Install the latest cmake by source code.
```
tar xvfz cmake-3.26.0-rc6.tar.gz
cd cmake-3.26.0-rc6
./bootstrap --prefix=/home/qmy/softwares/cmake-3.26.0-rc6
make
make install
```
```
#cmake
export PATH="/home/qmy/softwares/cmake-3.26.0-rc6/bin/:$PATH"
```
## 4. Install gromacs-2023
```
tar xvfz  gromacs-2023.tar.gz
cd gromacs-2023
mkdir build
cd build
cmake .. -DGMX_BUILD_OWN_FFTW=ON -DREGRESSIONTEST_DOWNLOAD=ON  -DGMX_GPU=CUDA -DCMAKE_INSTALL_PREFIX=/home/qmy/softwares/gromacs-2023  -DCMAKE_C_COMPILER=gcc-9 -DCMAKE_CXX_COMPILER=g++-9
make -j8
make install
```
```
# GROMACS
source /home/qmy/softwares/gromacs-2023/bin/GMXR
```
## 5. Install Amber 20
```
sudo apt -y install tcsh make gcc gfortran flex bison patch bc wget xorg-dev libz-dev libbz2-dev
```
```
tar xvfj AmberTools20.tar.bz2
tar xvfj Amber20.tar.bz2
cd amber20_src/build/
```
```
vi run_cmake
```
Replace:
-  "-DCUDA=FALSE"  -->  "-DCUDA=TRUE"
-  "-DDOWNLOAD_MINICONDA=TRUE" --> "-DDOWNLOAD_MINICONDA=FALSE"
-  "-DMINICONDA_USE_PY3=TRUE" --> "-DMINICONDA_USE_PY3=FALSE"

Add:
-  "-DBUILD_PYTHON=FALSE"

```
vi ../cmake/CudaConfig.cmake
```
Delete:
-   "set(SM30FLAGS -gencode arch=compute_30,code=sm_30)"

Replace:
-  "VERSION_EQUAL 10.2 OR ${CUDA_VERSION} VERSION_LESS 10.2" --> "VERSION_EQUAL 12.2 OR ${CUDA_VERSION} VERSION_LESS 12.2"

```
./run_cmake
make install
source /home/qmy/softwares/amber20/amber.sh
```
