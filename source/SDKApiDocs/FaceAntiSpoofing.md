# 静默活体识别器

## **1. 接口简介** <br>

静默活体识别根据输入的图像数据、人脸位置和人脸特征点，对输入人脸进行活体的判断，并返回人脸活体的状态。<br>

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

### **2.3 struct SeetaPointF**<br>


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

## 3 class FaceAntiSpoofing
活体识别器。<br>

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

构造活体识别器需要传入的结构体参数。<br>

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
        <td> 识别器模型</td>
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

#### FaceAntiSpoofing

构造活体识别器，需要在构造的时候传入识别器结构参数。<br>

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
        <td> 识别器接口参数</td>
    </tr>
<table>
<br>

说明：活体对象创建可以出入一个模型文件（局部活体模型）和两个模型文件（局部活体模型和全局活体模型，顺序不可颠倒），传入一个模型文件时活体识别速度快于传入两个模型文件的识别速度，传入一个模型文件时活体识别精度低于传入两个模型文件的识别精度。<br>

### 3.4 成员函数

#### Predict

基于单帧图像对人脸是否为活体进行判断。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> image</td>
        <td> const SeetaImageData& </td>
        <td> </td>
        <td> 原始图像数据</td>
    </tr>
    <tr>
        <td> face</td>
        <td> const SeetaRect& </td>
        <td> </td>
        <td> 人脸位置</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF* </td>
        <td> </td>
        <td> 人脸特征点数组</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> Status </td>
        <td> </td>
        <td> 活体状态</td>
    </tr>
<table>
<br>

说明：Status 活体状态可取值为REAL(真人)、SPOOF(假体)、FUZZY（由于图像质量问题造成的无法判断）和 DETECTING（正在检测），DETECTING 状态针对于 PredicVideo 模式。<br>

#### PredictVideo

基于连续视频序列对人脸是否为活体进行判断。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> image</td>
        <td> const SeetaImageData& </td>
        <td> </td>
        <td> 原始图像数据</td>
    </tr>
    <tr>
        <td> face</td>
        <td> const SeetaRect& </td>
        <td> </td>
        <td> 人脸位置</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF* </td>
        <td> </td>
        <td> 人脸特征点数组</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> Status </td>
        <td> </td>
        <td> 活体状态</td>
    </tr>
<table>
<br>

说明：Status 活体状态可取值为REAL(真人)、SPOOF(假体)、FUZZY（由于图像质量问题造成的无法判断）和 DETECTING（正在检测），DETECTING 状态针对于 PredicVideo 模式。<br>

#### ResetVideo

重置活体识别结果，开始下一次 PredictVideo 识别过程。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void </td>
        <td> </td>
        <td> </td>
    </tr>
<table>
<br>

#### GetPreFrameScore

获取活体检测内部分数。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> clarity</td>
        <td> float* </td>
        <td> </td>
        <td> 人脸清晰度分数</td>
    </tr>
    <tr>
        <td> reality</td>
        <td> float* </td>
        <td> </td>
        <td> 活体分数</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void </td>
        <td> </td>
        <td> </td>
    </tr>
<table>
<br>

#### SetVideoFrameCount

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
        <td> number</td>
        <td> int32_t </td>
        <td> </td>
        <td> video模式下活体需求帧数</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void </td>
        <td> </td>
        <td> </td>
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
        <td> 返回值</td>
        <td> int </td>
        <td> </td>
        <td> </td>
    </tr>
<table>
<br>

#### SetThreshold

设置阈值。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> clarity</td>
        <td> float </td>
        <td> </td>
        <td>人脸清晰度阈值 </td>
    </tr>
    <tr>
        <td> reality</td>
        <td> float </td>
        <td> </td>
        <td>人脸活体阈值 </td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void </td>
        <td> </td>
        <td> </td>
    </tr>
<table>
<br>

说明：人脸清晰度阈值默认为0.3，人脸活体阈值为0.8。<br>

#### GetThreshold

获取阈值。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> clarity</td>
        <td> float* </td>
        <td> </td>
        <td>人脸清晰度阈值 </td>
    </tr>
    <tr>
        <td> reality</td>
        <td> float* </td>
        <td> </td>
        <td>人脸活体阈值 </td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void </td>
        <td> </td>
        <td> </td>
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