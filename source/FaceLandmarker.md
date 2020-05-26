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

构造人脸特征点检测器需要传入的结构体参数。<br>

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

#### FaceLandmarker


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
        <th> 检测器结构参数</th>
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
        <th> 返回值</th>
        <th> int</th>
        <th> </th>
        <th> 模型特征点数组长度</th>
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
        <th> image</th>
        <th> const SeetaImageData&</th>
        <th> </th>
        <th> 图像原始数据</th>
    </tr>
    <tr>
        <th> face</th>
        <th> const SeetaRect&</th>
        <th> </th>
        <th>  人脸位置</th>
    </tr>
    <tr>
        <th> points</th>
        <th> SeetaPointF*</th>
        <th> </th>
        <th> 获取的人脸特征点数组(需预分配好数组长度，长度为number()返回的值)</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> void</th>
        <th> </th>
        <th> </th>
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
        <th> image</th>
        <th> const SeetaImageData&</th>
        <th> </th>
        <th> 图像原始数据</th>
    </tr>
    <tr>
        <th> face</th>
        <th> const SeetaRect&</th>
        <th> </th>
        <th>  人脸位置</th>
    </tr>
    <tr>
        <th> points</th>
        <th> SeetaPointF*</th>
        <th> 获取的人脸特征点数组(需预分配好数组长度，长度为number()返回的值)</th>
        <th> </th>
    </tr>
    <tr>
        <th>mask </th>
        <th> int32_t</th>
        <th> </th>
        <th> 获取人脸特征点位置对应的遮挡信息数组(需预分配好数组长度，长度为number()返回的值), 其中值为1表示被遮挡，0表示未被遮挡</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> void</th>
        <th> </th>
        <th> </th>
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
        <th> image</th>
        <th> const SeetaImageData&</th>
        <th> </th>
        <th> 图像原始数据</th>
    </tr>
    <tr>
        <th> face</th>
        <th> const SeetaRect&</th>
        <th> </th>
        <th>  人脸位置</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> std::vector<SeetaPointF> </th>
        <th> </th>
        <th> 获取的人脸特征点数组</th>
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
        <th> image</th>
        <th> const SeetaImageData&</th>
        <th> </th>
        <th> 图像原始数据</th>
    </tr>
    <tr>
        <th> face</th>
        <th> const SeetaRect&</th>
        <th> </th>
        <th>  人脸位置</th>
    </tr>
    <tr>
        <th> 返回值</th>
        <th> std::vector<PointWithMask> </th>
        <th> </th>
        <th> 获取人脸特征点和是否遮挡数组</th>
    </tr>
<table>
<br>