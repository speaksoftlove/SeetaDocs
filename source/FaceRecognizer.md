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

### **2.2 struct SeetaPointF**<br>


<table border="1">
    <tr>
        <th>名称</th>
        <th>类型</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> x</th>
        <th> double</th>
        <th>人脸特征点横坐标 </th>
    </tr>
    <tr>
        <th> y</th>
        <th> double</th>
        <th> 人脸特征点纵坐标</th>
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
        <th>SEETA_DEVICE_AUTO </th>
        <th>自动检测，会优先使用 GPU </th>
    </tr>
    <tr>
        <th> SEETA_DEVICE_CPU</th>
        <th> 使用CPU计算</th>
    </tr>
    <tr>
        <th>SEETA_DEVICE_GPU </th>
        <th>使用GPU计算 </th>
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
        <th> model</th>
        <th> const char**</th>
        <th> </th>
        <th> 检测器模型</th>
    </tr>
    <tr>
        <th> id</th>
        <th> int</th>
        <th> </th>
        <th>  GPU id</th>
    </tr>
    <tr>
        <th> device</th>
        <th> SeetaDevice</th>
        <th> AUTO</th>
        <th> 计算设备(CPU 或者 GPU)</th>
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
        <th> setting</th>
        <th> const SeetaModelSetting&</th>
        <th> </th>
        <th> 识别器结构参数</th>
    </tr>
<table>
<br>

### 3.4 成员函数

#### GetCropFaceWidth

获取裁剪人脸的宽度。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> int</th>
        <th> </th>
        <th> 返回的人脸宽度</th>
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
        <th> 返回值</th>
        <th> int</th>
        <th> </th>
        <th> 返回的人脸高度</th>
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
        <th> 返回值</th>
        <th> int</th>
        <th> </th>
        <th> 返回的人脸数据通道数</th>
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
        <th> image</th>
        <th> const SeetaImageData&</th>
        <th> </th>
        <th> 原始图像数据</th>
    </tr>
    <tr>
        <th> points</th>
        <th> const SeetaPointF*</th>
        <th> </th>
        <th>  人脸特征点数组</th>
    </tr>
    <tr>
        <th> face</th>
        <th> SeetaImageData&</th>
        <th> </th>
        <th> 返回的裁剪人脸</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> bool</th>
        <th> </th>
        <th> true表示人脸裁剪成功</th>
    </tr>
<table>
<br>

#### GetCropFaceWidthV2

获取裁剪人脸的宽度。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> int</th>
        <th> </th>
        <th> 返回的人脸宽度</th>
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
        <th> 返回值</th>
        <th> int</th>
        <th> </th>
        <th> 返回的人脸高度</th>
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
        <th> 返回值</th>
        <th> int</th>
        <th> </th>
        <th> 返回的人脸数据通道数</th>
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
        <th> image</th>
        <th> const SeetaImageData&</th>
        <th> </th>
        <th> 原始图像数据</th>
    </tr>
    <tr>
        <th> points</th>
        <th> const SeetaPointF*</th>
        <th> </th>
        <th>  人脸特征点数组</th>
    </tr>
    <tr>
        <th> face</th>
        <th> SeetaImageData&</th>
        <th> </th>
        <th> 返回的裁剪人脸</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> bool</th>
        <th> </th>
        <th> true表示人脸裁剪成功</th>
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
        <th> image</th>
        <th> const SeetaImageData&</th>
        <th> </th>
        <th> 原始图像数据</th>
    </tr>
    <tr>
        <th> points</th>
        <th> const SeetaPointF*</th>
        <th> </th>
        <th>  人脸特征点数组</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> seeta::ImageData</th>
        <th> </th>
        <th> 返回的裁剪人脸</th>
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
        <th> 返回值</th>
        <th> int</th>
        <th> </th>
        <th> 特征值数组的长度</th>
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
        <th> face</th>
        <th> const SeetaImageData&</th>
        <th> </th>
        <th> 裁剪后的人脸图像数据</th>
    </tr>
    <tr>
        <th> features</th>
        <th> float*</th>
        <th> </th>
        <th>  返回的人脸特征值数组</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> bool</th>
        <th> </th>
        <th> true表示提取特征成功</th>
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
        <th> image</th>
        <th> const SeetaImageData&</th>
        <th> </th>
        <th> 原始的人脸图像数据</th>
    </tr>
    <tr>
        <th> points</th>
        <th> const SeetaPointF*</th>
        <th> </th>
        <th> 人脸的特征点数组</th>
    </tr>
    <tr>
        <th> features</th>
        <th> float*</th>
        <th> </th>
        <th>  返回的人脸特征值数组</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> bool</th>
        <th> </th>
        <th> true表示提取特征成功</th>
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
        <th> features1</th>
        <th> const float*</th>
        <th> </th>
        <th> 特征数组一</th>
    </tr>
    <tr>
        <th> features2</th>
        <th> const float*</th>
        <th> </th>
        <th> 特征数组二</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> float</th>
        <th> </th>
        <th> 相似度值</th>
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