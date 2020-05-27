# 人脸识别器

## **1. 接口简介** <br>

人脸识别器要求输入原始图像数据和人脸特征点（或者裁剪好的人脸数据），对输入的人脸提取特征值数组，根据提取的特征值数组对人脸进行相似度比较。<br>

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

### **2.2 struct SeetaPointF**<br>


<table border="1">
    <tr>
        <th>名称</th>
        <th>类型</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> x</td>
        <td> double</td>
        <td>人脸特征点横坐标 </td>
    </tr>
    <tr>
        <td> y</td>
        <td> double</td>
        <td> 人脸特征点纵坐标</td>
    </tr>
</table>
<br>


## 3 class FaceRecognizer

人脸识别器。<br>

### 3.1 Enum SeetaDevice

模型运行的计算设备。<br>

<table border="1">
    <tr>
        <th>名称</th>
        <th>说明</th>
    </tr>
    <tr>
        <td>SEETA_DEVICE_AUTO </td>
        <td>自动检测，会优先使用 GPU </td>
    </tr>
    <tr>
        <td> SEETA_DEVICE_CPU</td>
        <td> 使用CPU计算</td>
    </tr>
    <tr>
        <td>SEETA_DEVICE_GPU </td>
        <td>使用GPU计算 </td>
    </tr>
</table>
<br>

### 3.2 struct SeetaModelSetting

构造人脸识别器需要传入的结构体参数。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> model</td>
        <td> const char**</td>
        <td> </td>
        <td> 检测器模型</td>
    </tr>
    <tr>
        <td> id</td>
        <td> int</td>
        <td> </td>
        <td>  GPU id</td>
    </tr>
    <tr>
        <td> device</td>
        <td> SeetaDevice</td>
        <td> AUTO</td>
        <td> 计算设备(CPU 或者 GPU)</td>
    </tr>
<table>
<br>

### 3.3 构造函数

#### FaceRecognizer


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> setting</td>
        <td> const SeetaModelSetting&</td>
        <td> </td>
        <td> 识别器结构参数</td>
    </tr>
<table>
<br>

### 3.4 成员函数

#### GetCropFaceWidtd

获取裁剪人脸的宽度。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> int</td>
        <td> </td>
        <td> 返回的人脸宽度</td>
    </tr>
<table>
<br>

#### GetCropFaceHeight

获取裁剪的人脸高度。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> int</td>
        <td> </td>
        <td> 返回的人脸高度</td>
    </tr>
<table>
<br>

#### GetCropFaceChannels

获取裁剪的人脸数据通道数。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> int</td>
        <td> </td>
        <td> 返回的人脸数据通道数</td>
    </tr>
<table>
<br>

#### CropFace

裁剪人脸。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> image</td>
        <td> const SeetaImageData&</td>
        <td> </td>
        <td> 原始图像数据</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF*</td>
        <td> </td>
        <td>  人脸特征点数组</td>
    </tr>
    <tr>
        <td> face</td>
        <td> SeetaImageData&</td>
        <td> </td>
        <td> 返回的裁剪人脸</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> bool</td>
        <td> </td>
        <td> true表示人脸裁剪成功</td>
    </tr>
<table>
<br>

#### GetCropFaceWidtdV2

获取裁剪人脸的宽度。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> int</td>
        <td> </td>
        <td> 返回的人脸宽度</td>
    </tr>
<table>
<br>

#### GetCropFaceHeightV2

获取裁剪的人脸高度。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> int</td>
        <td> </td>
        <td> 返回的人脸高度</td>
    </tr>
<table>
<br>

#### GetCropFaceChannelsV2

获取裁剪的人脸数据通道数。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> int</td>
        <td> </td>
        <td> 返回的人脸数据通道数</td>
    </tr>
<table>
<br>

#### CropFaceV2

裁剪人脸。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> image</td>
        <td> const SeetaImageData&</td>
        <td> </td>
        <td> 原始图像数据</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF*</td>
        <td> </td>
        <td>  人脸特征点数组</td>
    </tr>
    <tr>
        <td> face</td>
        <td> SeetaImageData&</td>
        <td> </td>
        <td> 返回的裁剪人脸</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> bool</td>
        <td> </td>
        <td> true表示人脸裁剪成功</td>
    </tr>
<table>
<br>

#### CropFaceV2

裁剪人脸。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> image</td>
        <td> const SeetaImageData&</td>
        <td> </td>
        <td> 原始图像数据</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF*</td>
        <td> </td>
        <td>  人脸特征点数组</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> seeta::ImageData</td>
        <td> </td>
        <td> 返回的裁剪人脸</td>
    </tr>
<table>
<br>

#### GetExtractFeatureSize

获取特征值数组的长度。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> int</td>
        <td> </td>
        <td> 特征值数组的长度</td>
    </tr>
<table>
<br>

#### ExtractCroppedFace

输入裁剪后的人脸图像，提取人脸的特征值数组。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> face</td>
        <td> const SeetaImageData&</td>
        <td> </td>
        <td> 裁剪后的人脸图像数据</td>
    </tr>
    <tr>
        <td> features</td>
        <td> float*</td>
        <td> </td>
        <td>  返回的人脸特征值数组</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> bool</td>
        <td> </td>
        <td> true表示提取特征成功</td>
    </tr>
<table>
<br>

#### Extract

输入原始图像数据和人脸特征点数组，提取人脸的特征值数组。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> image</td>
        <td> const SeetaImageData&</td>
        <td> </td>
        <td> 原始的人脸图像数据</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF*</td>
        <td> </td>
        <td> 人脸的特征点数组</td>
    </tr>
    <tr>
        <td> features</td>
        <td> float*</td>
        <td> </td>
        <td>  返回的人脸特征值数组</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> bool</td>
        <td> </td>
        <td> true表示提取特征成功</td>
    </tr>
<table>
<br>

#### CalculateSimilarity

比较两人脸的特征值数据，获取人脸的相似度值。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> features1</td>
        <td> const float*</td>
        <td> </td>
        <td> 特征数组一</td>
    </tr>
    <tr>
        <td> features2</td>
        <td> const float*</td>
        <td> </td>
        <td> 特征数组二</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> float</td>
        <td> </td>
        <td> 相似度值</td>
    </tr>
<table>
<br>

#### set

设置人脸检测器相关属性值。其中<br>
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