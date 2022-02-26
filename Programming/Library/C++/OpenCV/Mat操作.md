---
tags: 机器视觉 OpenCV C++
---

# Mat操作

## 基本操作

```c
Mat A(rowNum, columNum, Type);
// 构造函数
Mat B = A.row(int num);
//复制A的某一行
Mat C = A.clone(); 
//克隆A
Mat c;
A.copyTo(c);
//把A复制到c中
A.release();
//释放A内存
Mat D(A, Rect(0, 0, 100, 100))
//A中的某一范围
Mat E = A(Range::all(), Range(1, 3))
//效果同上
cout << M
//打印Mat，Mat只能为二维
Mat E = Mat::eye(4, 4, CV_64F);
//单位矩阵
Mat O = Mat::ones(2, 2, CV_32F);
//全1矩阵
Mat Z = Mat::zeros(3, 3, CV_8UC1);
//全0矩阵
Mat C = (Mat_<double>(3, 3) << 0, 1, 0, -1, 5, -1, 0, -1, 0);
C = (Mat_<double>({0, -1, 0, 1))).reshape(3);
//使用其他初始化
format(R, Formatter::FMT_PYTHON)
//格式化为Python多维列表
format(R, Formatter::FMT_CSV)
//格式化为CSV
format(R, Formatter::FMT_NUMPY)
//格式化为NUMPY
format(R, Formatter::FMT_C)
//格式化为C
Point2f P(5, 1);
//生成二维点
Point3f P3f(2, 6, 7);
//生成三维点
vector<float> v;
//生成float向量
vector<Point2f> vPoints(20);
//生成二维点组成的向量

convertScaleAbs(grad_x, abs_grad_x);
//取所有像素的绝对值

vector<Mat> bgr_planes;
split( src, bgr_planes );
//将一副RGB图像分离成三个通道的vector

normalize(b_hist, b_hist, 0, histImage.rows, NORM_MINMAX, -1, Mat() );
//归一化数组,将数组中值归一化到给定区间

minMaxLoc( result, &minVal, &maxVal, &minLoc, &maxLoc, Mat() );
//找到像素值最大的点

void add(InputArray src1, InputArray src2, OutputArray dst,InputArray mask=noArray(), int dtype= -1); //dst = src1 + src2
void subtract(InputArray src1, InputArray src2, OutputArray dst,InputArray mask=noArray(), int dtype= -1); //dst = src1 - src2
void multiply(InputArray src1, InputArray src2,OutputArray dst, double scale=1, int dtype=-1); //dst = scale*src1*src2
void divide(InputArray src1, InputArray src2, OutputArray dst,double scale=1, int dtype=-1); //dst = scale*src1/src2
void divide(double scale, InputArray src2,OutputArray dst, int dtype=-1); //dst = scale/src2
void scaleAdd(InputArray src1, double alpha, InputArray src2, OutputArray dst); //dst = alpha*src1 + src2
void addWeighted(InputArray src1, double alpha, InputArray src2,double beta, double gamma, OutputArray dst, int dtype=-1); //dst = alpha*src1 + beta*src2 + gamma
void sqrt(InputArray src, OutputArray dst); //计算每个矩阵元素的平方根
void pow(InputArray src, double power, OutputArray dst); //src的power次幂
void exp(InputArray src, OutputArray dst); //dst = e**src（**表示指数的意思）
void log(InputArray src, OutputArray dst); //dst = log(abs(src))

void bitwise_and(InputArray src1, InputArray src2,OutputArray dst, InputArray mask=noArray()); //dst = src1 & src2
void bitwise_or(InputArray src1, InputArray src2,OutputArray dst, InputArray mask=noArray()); //dst = src1 | src2
void bitwise_xor(InputArray src1, InputArray src2,OutputArray dst, InputArray mask=noArray()); //dst = src1 ^ src2
void bitwise_not(InputArray src, OutputArray dst,InputArray mask=noArray()); //dst = ~src
//MASK中有值像素才会保留，否则直接置0
```

## 扫描图像

```c
Mat::isContinuous()
//返回图像矩阵储存是否连续
m.rows;
//行数
m.cols;
//列数
m.channels();
//通道数
uchar *p = m.ptr<uchar>(i);
Vec3b *p = m.ptr<Vec3b>(i);
//使用Mat指针重定向Mat数据区,指针在第i行行首

uchar* p = I.data;
for( unsigned int i =0; i < ncol*nrows; ++i)
    *p++ = table[*p];
//使用指向第一行第一列的指针遍历

```

[OpenCV: Mat - The Basic Image Container](https://docs.opencv.org/4.1.0/d6/d6d/tutorial_mat_the_basic_image_container.html)

[OpenCV: Operations with images](https://docs.opencv.org/4.1.0/d5/d98/tutorial_mat_operations.html)

![[Programming/Library/C++/OpenCV/Asset For Mat操作/Untitled.png]]

## 对图像应用查找表

### 使用指针访问（Effective）

```c
Mat& ScanImageAndReduceC(Mat& I, const uchar* const table)
{
    // accept only char type matrices
    CV_Assert(I.depth() == CV_8U);
    int channels = I.channels();
    int nRows = I.rows;
    int nCols = I.cols * channels;
    if (I.isContinuous())
    {
        nCols *= nRows;
        nRows = 1;
    }
    int i,j;
    uchar* p;
    for( i = 0; i < nRows; ++i)
    {
        p = I.ptr<uchar>(i);
        for ( j = 0; j < nCols; ++j)
        {
            p[j] = table[p[j]];
        }
    }
    return I;
}
//遍历矩阵，由于有三个通道，故在列循环时要多循环三次
```

### 使用迭代器访问

```c
Mat& ScanImageAndReduceIterator(Mat& I, const uchar* const table)
{
    // accept only char type matrices
    CV_Assert(I.depth() == CV_8U);
    const int channels = I.channels();
    switch(channels)
    {
    case 1:
        {
            MatIterator_<uchar> it, end;
            for( it = I.begin<uchar>(), end = I.end<uchar>(); it != end; ++it)
                *it = table[*it];
            break;
        }
    case 3:
        {
            MatIterator_<Vec3b> it, end;
            for( it = I.begin<Vec3b>(), end = I.end<Vec3b>(); it != end; ++it)
            {
                (*it)[0] = table[(*it)[0]];
                (*it)[1] = table[(*it)[1]];
                (*it)[2] = table[(*it)[2]];
            }
        }
    }
    return I;
}
```

### 使用At函数访问

```c
Mat& ScanImageAndReduceRandomAccess(Mat& I, const uchar* const table)
{
    // accept only char type matrices
    CV_Assert(I.depth() == CV_8U);
    const int channels = I.channels();
    switch(channels)
    {
    case 1:
        {
            for( int i = 0; i < I.rows; ++i)
                for( int j = 0; j < I.cols; ++j )
                    I.at<uchar>(i,j) = table[I.at<uchar>(i,j)];
            break;
        }
    case 3:
        {
         Mat_<Vec3b> _I = I;
         for( int i = 0; i < I.rows; ++i)
            for( int j = 0; j < I.cols; ++j )
               {
                   _I(i,j)[0] = table[_I(i,j)[0]];
                   _I(i,j)[1] = table[_I(i,j)[1]];
                   _I(i,j)[2] = table[_I(i,j)[2]];
            }
         I = _I;
         break;
        }
    }
    return I;
}
//M_ 可以使用(i, j)快速引用位置
```

### 通过Lut函数访问

```c
LUT(InputArray src, InputArray lut, OutputArray dst);
//src表示源图像
//lut表示查找表
//dst表示输出图像
```

[OpenCV: How to scan images, lookup tables and time measurement with OpenCV](https://docs.opencv.org/4.1.0/db/da5/tutorial_how_to_scan_images.html)