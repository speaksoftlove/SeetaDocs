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

### **2.3 struct SeetaTrackingFaceInfo**<br>

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
    <tr>
        <td>frame_no</td>
        <td>int</td>
        <td>视频帧的索引</td>
    </tr>
    <tr>
        <td>PID</td>
        <td>int</td>
        <td>跟踪的人脸标识id</td>
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
        <td>data</td>
        <td>const SeetaTrackingFaceInfo*</td>
        <td>人脸信息数组</td>
    </tr>
    <tr>
        <td>size</td>
        <td>int</td>
        <td>人脸信息数组长度</td>
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

构造人脸跟踪器需要传入的结构体参数。<br>

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

#### FaceTrakcer

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
    <tr>
        <td>video_widtd </td>
        <td>int </td>
        <td> </td>
        <td>视频的宽度 </td>
    </tr>
    <tr>
        <td>video_height </td>
        <td>int </td>
        <td> </td>
        <td>视频的高度 </td>
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
        <td> num</td>
        <td> int</td>
        <td> </td>
        <td> 线程数量</td>
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
        <td>image </td>
        <td> const SeetaImageData&</td>
        <td> </td>
        <td> 输入的图像数据</td>
    </tr>
    <tr>
        <td>返回值 </td>
        <td>SeetaTrackingFaceInfoArray </td>
        <td> </td>
        <td> 跟踪到的人脸信息数组 </td>
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
        <td>image </td>
        <td> const SeetaImageData&</td>
        <td> </td>
        <td> 输入的图像数据</td>
    </tr>
    <tr>
        <td>frame_no </td>
        <td> int</td>
        <td> </td>
        <td> 视频帧索引</td>
    </tr>
    <tr>
        <td>返回值 </td>
        <td>SeetaTrackingFaceInfoArray </td>
        <td> </td>
        <td> 跟踪到的人脸信息数组 </td>
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
        <td>size </td>
        <td>int32_t </td>
        <td> </td>
        <td> 最小人脸大小</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void</td>
        <td> </td>
        <td>  </td>
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
        <td> 返回值</td>
        <td> int32_t</td>
        <td> </td>
        <td>  最小人脸大小</td>
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
        <td> tdresh</td>
        <td> float</td>
        <td> </td>
        <td>  检测阈值</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void</td>
        <td> </td>
        <td>  </td>
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
        <td> 返回值</td>
        <td> float</td>
        <td> </td>
        <td>  检测阈值</td>
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
        <td> stable</td>
        <td> bool</td>
        <td> </td>
        <td>  是否是稳定模式</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void</td>
        <td> </td>
        <td>  </td>
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
        <td> 返回值</td>
        <td> bool</td>
        <td> </td>
        <td>  是否是稳定模式</td>
    </tr>
<table>
<br>