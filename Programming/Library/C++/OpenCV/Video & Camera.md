---
tags: 机器视觉 OpenCV C++
---
# Video & Camera

## 标准读取摄像头操作

```c
VideoCapture cap(0);
if(!cap.isOpened()) return -1;
Mat frame;
namedWindow("edges", WINDOW_AUTOSIZE);
for(;;)
{
    cap >> frame;
    imshow("edges", frame);
    if(waitKey(30) >= 0) break;
}
return 0;
```