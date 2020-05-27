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
            <td>data</td>
            <td>uint8_t*</td>
            <td>图像数据</td>
        </tr>
         <tr>
            <td>widtd</td>
            <td>int32_t</td>
            <td>图像的宽度</td>
        </tr>
       <tr>
            <td>height</td>
            <td>int32_t</td>
            <td>图像的高度</td>
        </tr>
        <tr>
            <td>channels</td>
            <td>int32_t</td>
            <td>图像的通道数</td>
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
            <td>x</td>
            <td>int32_t</td>
            <td>人脸区域左上角横坐标</td>
        </tr>
        <tr>
            <td>y</td>
            <td>int32_t</td>
            <td>人脸区域左上角纵坐标</td>
        </tr>
        <tr>
            <td>widtd</td>
            <td>int32_t</td>
            <td>人脸区域宽度</td>
        </tr>
        <tr>
            <td>height</td>
            <td>int32_t</td>
            <td>人脸区域高度</td>
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
        <td>pos</td>
        <td>SeetaRect</td>
        <td>人脸位置</td>
    </tr>
    <tr>
        <td>score</td>
        <td>float</td>
        <td>人脸置信分数</td>
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
        <td>data</td>
        <td>const SeetaFaceInfo*</td>
        <td>人脸信息数组</td>
    </tr>
    <tr>
        <td>size</td>
        <td>int</td>
        <td>人脸信息数组长度</td>
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
        <td>SEETA_DEVICE_AUTO</td>
        <td>自动检测，会优先使用 GPU</td>
    </tr>
    <tr>
        <td>SEETA_DEVICE_CPU</td>
        <td>使用CPU计算</td>
    </tr>
    <tr>
        <td>SEETA_DEVICE_GPU</td>
        <td>使用GPU计算</td>
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
        <td>model</td>
        <td>const char**</td>
        <td></td>
        <td>检测器模型</td>
    </tr>
    <tr>
        <td>id</td>
        <td>int</td>
        <td></td>
        <td>GPU id</td>
    </tr>
    <tr>
        <td>device</td>
        <td>SeetaDevice</td>
        <td>AUTO</td>
        <td>计算设备(CPU 或者 GPU)</td>
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
        <td>setting </td>
        <td>const SeetaModelSetting& </td>
        <td> </td>
        <td>检测器结构参数 </td>
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
        <td>image </td>
        <td> const SeetaImageData&</td>
        <td> </td>
        <td> 输入的图像数据</td>
    </tr>
    <tr>
        <td>返回值 </td>
        <td>SeetaFaceInfoArray </td>
        <td> </td>
        <td> 人脸信息数组 </td>
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
        <td>property </td>
        <td>Property </td>
        <td> </td>
        <td> 人脸检测器属性类别</td>
    </tr>
    <tr>
        <td> value</td>
        <td> double</td>
        <td> </td>
        <td> 设置的属性值 </td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td>void </td>
        <td> </td>
        <td> </td>
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
        <td>property </td>
        <td> Property</td>
        <td> </td>
        <td> 人脸检测器属性类别</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td>double </td>
        <td> </td>
        <td> 对应的人脸属性值 </td>
    </tr>
<table>
<br>