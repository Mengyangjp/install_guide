```
sudo apt-get install csh libstdc++5
```
[download](http://www.ks.uiuc.edu/Development/Download/download.cgi)vmd-1.9.3

```
tar -zxvf vmd-1.9.3.bin.LINUXAMD64-CUDA8-OptiX4-OSPRay111p1.opengl.tar.gz
cd vmd-1.9.3
./configure --prefix=/home/qmy/softwares/vmd-1.9.3 --libdir=/home/qmy/sotwares/vmd-1.9.3/lib64 LINUXAMD64
cd src
make install
```

