# 人脸检测器

## **1. 接口简介** <br>

人脸检测器会对输入的彩色图片或者灰度图像进行人脸检测，并返回所有检测到的人脸位置。<br>

## **2. 类型说明**<br>

### **2.1 struct SeetaImageData**<br>

<table border="1">
        <tr>
            <th>名称</th>
            <th>类型</th>
            <th>说明</th>
        </tr>
         <tr>
            <th>data</th>
            <th>uint8_t*</th>
            <th>图像数据</th>
        </tr>
         <tr>
            <th>width</th>
            <th>int32_t</th>
            <th>图像的宽度</th>
        </tr>
       <tr>
            <th>height</th>
            <th>int32_t</th>
            <th>图像的高度</th>
        </tr>
        <tr>
            <th>channels</th>
            <th>int32_t</th>
            <th>图像的通道数</th>
        </tr>
</table>
<br>

说明：存储彩色（三通道）或灰度（单通道）图像，像素连续存储，行优先，采用 BGR888 格式存放彩色图像，单字节灰度值存放灰度图像。<br>

### **2.2 struct SeetaRect**<br>

<table border="1">
        <tr>
            <th>名称</th>
            <th>类型</th>
            <th>说明</th>
        </tr>
        <tr>
            <th>x</th>
            <th>int32_t</th>
            <th>人脸区域左上角横坐标</th>
        </tr>
        <tr>
            <th>y</th>
            <th>int32_t</th>
            <th>人脸区域左上角纵坐标</th>
        </tr>
        <tr>
            <th>width</th>
            <th>int32_t</th>
            <th>人脸区域宽度</th>
        </tr>
        <tr>
            <th>height</th>
            <th>int32_t</th>
            <th>人脸区域高度</th>
        </tr>
</table>
<br>

### **2.3 struct SeetaFaceInfo**<br>

<table border="1">
    <tr>
        <th>名称</th>
        <th>类型</th>
        <th>说明</th>
    </tr>
    <tr>
        <th>pos</th>
        <th>SeetaRect</th>
        <th>人脸位置</th>
    </tr>
    <tr>
        <th>score</th>
        <th>float</th>
        <th>人脸置信分数</th>
    </tr>
 </table>
 <br>
 
 ### **2.4 struct SeetaFaceInfoArray**<br>
 
 <table border="1">
    <tr>
        <th>名称</th>
        <th>类型</th>
        <th>说明</th>
    </tr>
    <tr>
        <th>data</th>
        <th>const SeetaFaceInfo*</th>
        <th>人脸信息数组</th>
    </tr>
    <tr>
        <th>size</th>
        <th>int</th>
        <th>人脸信息数组长度</th>
    </tr>
<table>
<br>

## 3 class FaceDetector

人脸检测器。<br>

### 3.1 Enum SeetaDevice

模型运行的计算设备。<br>

<table border="1">
    <tr>
        <th>名称</th>
        <th>说明</th>
    </tr>
    <tr>
        <th>SEETA_DEVICE_AUTO</th>
        <th>自动检测，会优先使用 GPU</th>
    </tr>
    <tr>
        <th>SEETA_DEVICE_CPU</th>
        <th>使用CPU计算</th>
    </tr>
    <tr>
        <th>SEETA_DEVICE_GPU</th>
        <th>使用GPU计算</th>
    </tr>
<table>
<br>

### 3.2 struct SeetaModelSetting

构造人脸检测器需要传入的结构体参数。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th>model</th>
        <th>const char**</th>
        <th></th>
        <th>检测器模型</th>
    </tr>
    <tr>
        <th>id</th>
        <th>int</th>
        <th></th>
        <th>GPU id</th>
    </tr>
    <tr>
        <th>device</th>
        <th>SeetaDevice</th>
        <th>AUTO</th>
        <th>计算设备(CPU 或者 GPU)</th>
    </tr>
<table>
<br>

### 3.3 构造函数

#### FaceDetector

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th>setting </th>
        <th>const SeetaModelSetting& </th>
        <th> </th>
        <th>检测器结构参数 </th>
    </tr>
<table>
<br>

### 3.4 成员函数

#### detect

输入彩色图像，检测其中的人脸。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th>image </th>
        <th> const SeetaImageData&</th>
        <th> </th>
        <th> 输入的图像数据</th>
    </tr>
    <tr>
        <th>返回值 </th>
        <th>SeetaFaceInfoArray </th>
        <th> </th>
        <th> 人脸信息数组 </th>
    </tr>
<table>
<br>

#### set

设置人脸检测器相关属性值。其中<br>
**PROPERTY_MIN_FACE_SIZE**: 表示人脸检测器可以检测到的最小人脸，该值越小，支持检测到的人脸尺寸越小，检测速度越慢，默认值为20；<br>
**PROPERTY_THRESHOLD**:
表示人脸检测器过滤阈值，默认为 0.90；<br>
**PROPERTY_MAX_IMAGE_WIDTH** 和 **PROPERTY_MAX_IMAGE_HEIGHT**:
分别表示支持输入的图像的最大宽度和高度；<br>
**PROPERTY_NUMBER_THREADS**: 
表示人脸检测器计算线程数，默认为 4.<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th>property </th>
        <th>Property </th>
        <th> </th>
        <th> 人脸检测器属性类别</th>
    </tr>
    <tr>
        <th> value</th>
        <th> double</th>
        <th> </th>
        <th> 设置的属性值 </th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th>void </th>
        <th> </th>
        <th> </th>
    </tr>
<table>
<br>

#### get

获取人脸检测器相关属性值。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th>property </th>
        <th> Property</th>
        <th> </th>
        <th> 人脸检测器属性类别</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th>double </th>
        <th> </th>
        <th> 对应的人脸属性值 </th>
    </tr>
<table>
<br>