# 人脸特征点检测器

## **1. 接口简介** <br>

人脸特征点检测器要求输入原始图像数据和人脸位置，返回人脸 5 个或者其他数量的的特征点的坐标（特征点的数量和加载的模型有关）。<br>

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


## 3 class FaceLandmarker

人脸特征点检测器。<br>

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

构造人脸特征点检测器需要传入的结构体参数。<br>

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

#### FaceLandmarker


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
        <td> 检测器结构参数</td>
    </tr>
<table>
<br>

### 3.4 成员函数

#### number

获取模型对应的特征点数组长度。<br>


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
        <td> 模型特征点数组长度</td>
    </tr>
<table>
<br>

#### mark

获取人脸特征点。<br>


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
        <td> 图像原始数据</td>
    </tr>
    <tr>
        <td> face</td>
        <td> const SeetaRect&</td>
        <td> </td>
        <td>  人脸位置</td>
    </tr>
    <tr>
        <td> points</td>
        <td> SeetaPointF*</td>
        <td> </td>
        <td> 获取的人脸特征点数组(需预分配好数组长度，长度为number()返回的值)</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void</td>
        <td> </td>
        <td> </td>
    </tr>
<table>
<br>

#### mark

获取人脸特征点和遮挡信息。<br>


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
        <td> 图像原始数据</td>
    </tr>
    <tr>
        <td> face</td>
        <td> const SeetaRect&</td>
        <td> </td>
        <td>  人脸位置</td>
    </tr>
    <tr>
        <td> points</td>
        <td> SeetaPointF*</td>
        <td> 获取的人脸特征点数组(需预分配好数组长度，长度为number()返回的值)</td>
        <td> </td>
    </tr>
    <tr>
        <td>mask </td>
        <td> int32_t</td>
        <td> </td>
        <td> 获取人脸特征点位置对应的遮挡信息数组(需预分配好数组长度，长度为number()返回的值), 其中值为1表示被遮挡，0表示未被遮挡</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void</td>
        <td> </td>
        <td> </td>
    </tr>
<table>
<br>

#### mark

获取人脸特征点。<br>

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
        <td> 图像原始数据</td>
    </tr>
    <tr>
        <td> face</td>
        <td> const SeetaRect&</td>
        <td> </td>
        <td>  人脸位置</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> std::vector<SeetaPointF> </td>
        <td> </td>
        <td> 获取的人脸特征点数组</td>
    </tr>
<table>
<br>

#### mark_v2

获取人脸特征点和遮挡信息。<br>

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
        <td> 图像原始数据</td>
    </tr>
    <tr>
        <td> face</td>
        <td> const SeetaRect&</td>
        <td> </td>
        <td>  人脸位置</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> std::vector<PointWitdMask> </td>
        <td> </td>
        <td> 获取人脸特征点和是否遮挡数组</td>
    </tr>
<table>
<br>