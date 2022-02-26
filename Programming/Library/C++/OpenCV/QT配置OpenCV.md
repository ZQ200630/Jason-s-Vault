---
tags: 机器视觉 OpenCV C++
---
# QT配置OpenCV

## Linux已配好OpenCV环境

```c
CONFIG += link_pkgconfig
PKGCONFIG += opencv
```

## Windows

```c
INCLUDEPATH += F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/include \
               F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/include/opencv2

LIBS += F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_calib3d411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_core411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_dnn411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_features2d411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_flann411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_gapi411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_highgui411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_imgcodecs411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_imgproc411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_ml411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_objdetect411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_photo411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_stitching411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_video411.dll.a \
F:/OpenCV/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/OpenCV-MinGW-Build-OpenCV-4.1.1-x64/x64/mingw/lib/libopencv_videoio411.dll.a
```