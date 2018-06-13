---
layout: post
title: 安装caffe和faster-rcnn
description: 解决安装caffe和faster-rcnn遇到的问题
categories: Linux
keywords: linux, faster-rcnn 
---

解决安装caffe和faster-rcnn遇到的问题

# 安装caffe和faster-rcnn中注意事项
1. anaconda2/lib中protobuf、opencv和hdf5动态链接库与系统中usr/lib中冲突，因此，不能将anaconda2/lib加入LD_LIBRARY_PATH中，或者加入了也需要将这些库备份删除；
1. caffe编译中使用了protoc（版本>=2.5.0），通常ubutnu下使用apt-get安装后，与anaconda2/bin中冲突，因此，需要备份删除anaconda2/bin中的protoc；使用protoc --version查看其版本号；
1. 系统中安装了cuda8.0，其动态链接库位于：/usr/local/lib/cuda-8.0中，需要加入到LD_LIBRARY_PATH环境变量中；
1. caffe如果使用opencv3，则需要在Makefile.config中反注释；
1. Makefile.config中INCLUDE_PATH和LIB_PATH要注意完整；
1. 如果opencv动态链接库是使用/usr/lib中的，则需要卸载anaconda2中：conda remove opencv；
但是这样导致使用pycaffe时报错：eeror，import cv2；则需要使用：**pip install opencv-python**
1. FasterR-CNN和最新的版本cuDNNV5.0不兼容问题：
- cd py-faster-rcnn/caffe-fast-rcnn  
- git remote add caffe https://github.com/BVLC/caffe.git  
- git fetch caffe  
- git merge caffe/master
- 更新最新的caffe，并手动解决冲突，最后重新编译；
- 在合并之后注释掉include/caffe/layers/python_layer.hpp文件里： self_.attr(“phase”) = static_cast(this->phase_)