# 近红外活体识别器

## **1. 接口简介** <br>

近红外活体识别根据输入的近红外图像数据、人脸位置和人脸特征点，对输入人脸进行活体的判断，并返回人脸活体的状态。<br>

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

### **2.3 struct SeetaPointF**<br>


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


## 3 class SeetaNirFaceAntiSpoofing

近红外活体识别器。<br>

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

构造活体识别器需要传入的结构体参数。<br>

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
        <th> 识别器模型</th>
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

#### SeetaNirFaceAntiSpoofing

构造活体识别器，需要在构造的时候传入识别器结构参数。<br>

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
        <th> 识别器接口参数</th>
    </tr>
<table>
<br>

### 3.4 成员函数

#### predict

基于单帧图像对人脸是否为活体进行判断。<br>

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
        <th> face</th>
        <th> const SeetaRect&</th>
        <th> </th>
        <th>  人脸位置</th>
    </tr>
    <tr>
        <th> points</th>
        <th> const SeetaPointF*</th>
        <th> </th>
        <th> 人脸特征点数组</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> Status</th>
        <th> </th>
        <th> 人脸活体的状态</th>
    </tr>
<table>
<br>

说明：Status 活体状态可取值为REAL(真人)、SPOOF(假体)、FUZZY（由于图像质量问题造成的无法判断）和 DETECTING（正在检测），DETECTING 状态针对于 PredicVideo 模式。<br>


#### predictVideo

基于连续视频序列对人脸是否为活体进行判断。<br>

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
        <th> face</th>
        <th> const SeetaRect&</th>
        <th> </th>
        <th>  人脸位置</th>
    </tr>
    <tr>
        <th> points</th>
        <th> const SeetaPointF*</th>
        <th> </th>
        <th> 人脸特征点数组</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> Status</th>
        <th> </th>
        <th> 人脸活体的状态</th>
    </tr>
<table>
<br>

说明：Status 活体状态可取值为REAL(真人)、SPOOF(假体)、FUZZY（由于图像质量问题造成的无法判断）和 DETECTING（正在检测），DETECTING 状态针对于 PredicVideo 模式。<br>


#### resetVideo

重置活体识别结果，开始下一次 predictVideo 识别过程。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> void</th>
        <th> </th>
        <th> </th>
    </tr>
<table>
<br>

#### GetPreFasScore

获取活体检测内部分数。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> double</th>
        <th> </th>
        <th> 活体分数</th>
    </tr>
<table>
<br>

#### SetFrameNumForVideo

设置 Video 模式中识别视频帧数，当输入帧数为该值以后才会有活体的
真假结果。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> frameNumForVideo</th>
        <th> const int</th>
        <th> </th>
        <th> video模式下活体需求帧数</th>
    </tr>
<table>
<br>

#### GetVideoFrameCount

获取video模式下活体需求帧数。<br>

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
        <th> video模式下活体需求帧数</th>
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