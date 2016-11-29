# 拉流中转服务器的搭建
# 必要工具
    Linux OS［4G内存，4核，10Mbps外网］
    yasm-1.2.0.tar.gz
    ffmpeg-3.0.2.tar.bz2
# 安装步骤
## 1. 将ffmpeg-3.0.2.tar.bz2与yasm-1.2.0.tar.gz下载到目标服务器上
## 2. 处理yasm
```
 tar zxvf yasm-1.2.0.tar.gz
 cd yasm-1.2.0/
 ./configure
 make
 make install
```
### yasm down!
## 3. 处理ffmpeg
```
 bzip2 -d ffmpeg-3.0.2.tar.bz2
 tar xvf ffmpeg-3.0.2.tar
 cd ffmpeh-3.0.2/
 ./configure --enable-static --disable-shared --disable-yasm --enable-memalign-hack --enable-gpl --disable-libx264 --disable-librtmp --extra-cflags=-I/usr/local/include --extra-ldflags=-L/usr/local/lib --prefix=/usr/local
 make
 make install
```
### ffmpeg编译时间较长，耐心等待。。。。ffmpeg down!
## 4. 编译完成之后修改/etc/ld.so.conf，增加以下内容
```
 include ld.so.conf.d/*.conf
 /usr/local/ffmpeg/lib
 /usr/local/lib
```
### 修改完成！
## 5. 在最外层执行
```
ldconfig
```
## 6. 进入ffmpeg目录下
```
 cd ffmpeg-3.0.2/
 ./ffmpeg
```
### 执行完显示以下信息表示安装正常
```
ffmpeg version 3.0.2 Copyright (c) 2000-2016 the FFmpeg developers
  built with gcc 4.4.7 (GCC) 20120313 (Red Hat 4.4.7-16)
  configuration: --enable-static --disable-shared --disable-yasm --enable-memalign-hack --enable-gpl --disable-libx264 --disable-librtmp --extra-cflags=-I/usr/local/include --extra-ldflags=-L/usr/local/lib --prefix=/usr/local
  libavutil      55. 17.103 / 55. 17.103
  libavcodec     57. 24.102 / 57. 24.102
  libavformat    57. 25.100 / 57. 25.100
  libavdevice    57.  0.101 / 57.  0.101
  libavfilter     6. 31.100 /  6. 31.100
  libswscale      4.  0.100 /  4.  0.100
  libswresample   2.  0.101 /  2.  0.101
  libpostproc    54.  0.100 / 54.  0.100
Hyper fast Audio and Video encoder
usage: ffmpeg [options] [[infile options] -i infile]... {[outfile options] outfile}...
```
## 7.在ffmpeg目录下执行拉流转推命令
```
ffmpeg -i 要拉取的URL -acodec copy -vcodec copy -f flv 腾讯云／阿里云／金山云／七牛云rtmp地址
```
##  拉流转推工作完成。在腾讯云／阿里云／金山云／七牛云rtmp地址对应的播放地址中可以观看直播。
