# 人脸跟踪器

## **1. 接口简介** <br>

人脸跟踪器会对输入的彩色图像或者灰度图像中的人脸进行跟踪，并返回所有跟踪到的人脸信息。<br>

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

### **2.3 struct SeetaTrackingFaceInfo**<br>

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
    <tr>
        <th>frame_no</th>
        <th>int</th>
        <th>视频帧的索引</th>
    </tr>
    <tr>
        <th>PID</th>
        <th>int</th>
        <th>跟踪的人脸标识id</th>
    </tr>
 </table>
 <br>
 
 ### **2.4 struct SeetaTrackingFaceInfoArray**<br>
 
 <table border="1">
    <tr>
        <th>名称</th>
        <th>类型</th>
        <th>说明</th>
    </tr>
    <tr>
        <th>data</th>
        <th>const SeetaTrackingFaceInfo*</th>
        <th>人脸信息数组</th>
    </tr>
    <tr>
        <th>size</th>
        <th>int</th>
        <th>人脸信息数组长度</th>
    </tr>
<table>
<br>

## 3 class FaceTracker

人脸跟踪器。<br>

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

构造人脸跟踪器需要传入的结构体参数。<br>

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

#### FaceTrakcer

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
    <tr>
        <th>video_width </th>
        <th>int </th>
        <th> </th>
        <th>视频的宽度 </th>
    </tr>
    <tr>
        <th>video_height </th>
        <th>int </th>
        <th> </th>
        <th>视频的高度 </th>
    </tr>
<table>
<br>

### 3.4 成员函数

#### SetSingleCalculationThreads

设置底层的计算线程数量。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> num</th>
        <th> int</th>
        <th> </th>
        <th> 线程数量</th>
    </tr>
<table>
<br>

#### Track

对视频帧中的人脸进行跟踪。<br>

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
        <th>SeetaTrackingFaceInfoArray </th>
        <th> </th>
        <th> 跟踪到的人脸信息数组 </th>
    </tr>
<table>
<br>

#### Track

对视频帧中的人脸进行跟踪。<br>

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
        <th>frame_no </th>
        <th> int</th>
        <th> </th>
        <th> 视频帧索引</th>
    </tr>
    <tr>
        <th>返回值 </th>
        <th>SeetaTrackingFaceInfoArray </th>
        <th> </th>
        <th> 跟踪到的人脸信息数组 </th>
    </tr>
<table>
<br>

#### SetMinFaceSize

设置检测器的最小人脸大小。<br>


<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th>size </th>
        <th>int32_t </th>
        <th> </th>
        <th> 最小人脸大小</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> void</th>
        <th> </th>
        <th>  </th>
    </tr>
<table>
<br>

说明：size 的大小保证大于等于20，size的值越小，能够检测到的人脸的尺寸越小，检测速度越慢。<br>

#### GetMinFaceSize

获取最小人脸的大小。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> int32_t</th>
        <th> </th>
        <th>  最小人脸大小</th>
    </tr>
<table>
<br>


#### SetThreshold

设置检测器的检测阈值。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> thresh</th>
        <th> float</th>
        <th> </th>
        <th>  检测阈值</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> void</th>
        <th> </th>
        <th>  </th>
    </tr>
<table>
<br>

#### GetScoreThreshold

获取检测器检测阈值。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> float</th>
        <th> </th>
        <th>  检测阈值</th>
    </tr>
<table>
<br>

#### SetVideoStable

设置以稳定模式输出人脸跟踪结果。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> stable</th>
        <th> bool</th>
        <th> </th>
        <th>  是否是稳定模式</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> void</th>
        <th> </th>
        <th>  </th>
    </tr>
<table>
<br>

说明：只有在视频中连续跟踪时，才使用此方法。<br>

#### GetVideoStable

获取当前是否是稳定工作模式。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> bool</th>
        <th> </th>
        <th>  是否是稳定模式</th>
    </tr>
<table>
<br>