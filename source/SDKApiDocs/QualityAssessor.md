# 质量评估器

## **1. 接口简介** <br>

质量评估器包含不同的质量评估模块，包括人脸亮度、人脸清晰度（非深度方法）、人脸清晰度（深度方法）、人脸姿态（非深度方法）、人脸姿态（深度方法）、人脸分辨率评估模块。<br>

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

### 2.4 enum QualityLevel


<table border="1">
    <tr>
        <th>名称</th>
        <th>类型</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> LOW</td>
        <td> </td>
        <td> 表示人脸质量为低</td>
    </tr>
    <tr>
        <td> MEDIAUM</td>
        <td> </td>
        <td> 表示人脸质量为中</td>
    </tr>
    <tr>
        <td> HIGH</td>
        <td> </td>
        <td> 表示人脸质量为高</td>
    </tr>
</table>
<br>

### 2.5 class QualityResult


<table border="1">
    <tr>
        <th>名称</th>
        <th>类型</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> level</td>
        <td> QualityLevel</td>
        <td> 人脸质量等级</td>
    </tr>
    <tr>
        <td> score</td>
        <td> float</td>
        <td> 人脸质量分数</td>
    </tr>
</table>
<br>

## 3 class QualityOfBrightness

非深度的人脸亮度评估器。<br>

### 3.1 构造函数

#### QualityOfBrightness

人脸亮度评估器构造函数。<br>

#### QualityOfBrightness

人脸亮度评估器构造函数。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> v0</td>
        <td> float</td>
        <td> </td>
        <td> 分级参数一</td>
    </tr>
    <tr>
        <td> v1</td>
        <td> float</td>
        <td> </td>
        <td>  分级参数二</td>
    </tr>
    <tr>
        <td> v2</td>
        <td> float</td>
        <td> </td>
        <td> 分级参数三</td>
    </tr>
    <tr>
        <td> v3</td>
        <td> float</td>
        <td> </td>
        <td> 分级参数三</td>
    </tr>
<table>
<br>

说明：说明：分类依据为[0, v0) and [v3, ~) => LOW；[v0, v1) and [v2, v3) =>MEDIUM；[v1, v2) => HIGH。<br>

### 3.2 成员函数

#### check

检测人脸亮度。<br>

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
        <td> face</td>
        <td> const SeetaRect&</td>
        <td> </td>
        <td>  人脸位置</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF*</td>
        <td> </td>
        <td> 人脸5个特征点数组</td>
    </tr>
    <tr>
        <td> N</td>
        <td> const int32_t</td>
        <td> </td>
        <td> 人脸特征点数组长度</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> QualityResult</td>
        <td> </td>
        <td> 人脸亮度检测结果</td>
    </tr>
<table>
<br>

##  4 class QualityOfClarity

非深度学习的人脸清晰度评估器。<br>

### 4.1 构造函数

#### QualityOfClarity

人脸清晰度评估器构造函数。<br>

#### QualityOfClarity

人脸清晰度评估器构造函数。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> low</td>
        <td> float</td>
        <td> </td>
        <td> 分级参数一</td>
    </tr>
    <tr>
        <td> high</td>
        <td> float</td>
        <td> </td>
        <td>  分级参数二</td>
    </tr>
<table>
<br>

说明：分类依据为[0, low)=> LOW; [low, high)=> MEDIUM; [high, ~)=> HIGH。<br>

### 4.2 成员函数

#### check

检测人脸清晰度。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> image</td>
        <td> const SeetaImageData &</td>
        <td> </td>
        <td>原始图像数据 </td>
    </tr>
    <tr>
        <td> face</td>
        <td> const SeetaRect&</td>
        <td> </td>
        <td>  人脸位置</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF*</td>
        <td> </td>
        <td> 人脸5个特征点数组</td>
    </tr>
    <tr>
        <td> N</td>
        <td> const int32_t</td>
        <td> </td>
        <td> 人脸特征点数组长度</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> QualityResult</td>
        <td> </td>
        <td>人脸清晰度检测结果 </td>
    </tr>
<table>
<br>

## 5 class QualityOfLBN

深度学习的人脸清晰度评估器。<br>

### 5.1 Enum SeetaDevice

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

### 5.2 struct SeetaModelSetting

构造评估器需要传入的结构体参数。<br>

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

### 5.3 构造函数

人脸清晰度评估器构造函数。<br>

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
        <td> 对象构造结构体参数</td>
    </tr>
<table>
<br>

### 5.4 成员函数

#### Detect

检测人脸清晰度。<br>

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
        <td> 人脸68个特征点数组 </td>
    </tr>
    <tr>
        <td> light</td>
        <td> int*</td>
        <td> </td>
        <td> 亮度返回结果，暂不推荐使用该返回结果</td>
    </tr>
    <tr>
        <td> blur</td>
        <td> int*</td>
        <td> </td>
        <td> 模糊度返回结果</td>
    </tr>
    <tr>
        <td> noise</td>
        <td> int*</td>
        <td> </td>
        <td> 是否有噪声返回结果，暂不推荐使用该返回结果</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> void</td>
        <td> </td>
        <td> </td>
    </tr>
<table>
<br>

说明：blur 结果返回 0 说明人脸是清晰的，blur 为 1 说明人脸是模糊的。<br>

#### set

设置人脸检测器相关属性值。其中<br>
**PROPERTY_NUMBER_THREADS**: 
表示计算线程数，默认为 4。<br>
**PROPERTY_ARM_CPU_MODE**：针对于移动端，表示设置的 cpu 计算模式。0 表示
大核计算模式，1 表示小核计算模式，2 表示平衡模式，为默认模式。<br>
**PROPERTY_BLUR_THRESH**：表示人脸模糊阈值，默认值大小为 0.80。<br>

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

## 6 class QualityOfPose

非深度学习的人脸姿态评估器。<br>

### 6.1 构造函数

#### QualityOfPose

人脸姿态评估器构造函数。<br>

### 6.2 成员函数

#### check

检测人脸姿态。<br>

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
        <td> face</td>
        <td> const SeetaRect&</td>
        <td> </td>
        <td>  人脸位置</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF*</td>
        <td> </td>
        <td> 人脸5个特征点数组</td>
    </tr>
    <tr>
        <td> N</td>
        <td> const int32_t</td>
        <td> </td>
        <td>人脸特征点数组长度 </td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> QualityResult</td>
        <td> </td>
        <td>人脸姿态检测结果 </td>
    </tr>
<table>
<br>

## 7 class QualityOfPoseEx

深度学习的人脸姿态评估器。<br>

### 7.1 Enum SeetaDevice

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

### 7.2 struct SeetaModelSetting

构造评估器需要传入的结构体参数。<br>

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

### 7.3 构造函数

#### QualityOfPoseEx

人脸姿态评估器构造函数。<br>

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
        <td> 对象结构体参数</td>
    </tr>
<table>
<br>

### 7.4 成员函数

#### check

检测人脸姿态。<br>

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
        <td> face</td>
        <td> const SeetaRect&</td>
        <td> </td>
        <td>  人脸位置</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF*</td>
        <td> </td>
        <td> 人脸5个特征点数组</td>
    </tr>
    <tr>
        <td> N</td>
        <td> const int32_t</td>
        <td> </td>
        <td> 人脸特征点数组长度</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> QualityResult</td>
        <td> </td>
        <td> 人脸姿态检测结果</td>
    </tr>
<table>
<br>

#### check

检测人脸姿态。<br>

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
        <td> face</td>
        <td> const SeetaRect&</td>
        <td> </td>
        <td>  人脸位置</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF*</td>
        <td> </td>
        <td> 人脸5个特征点数组</td>
    </tr>
    <tr>
        <td> N</td>
        <td> const int32_t</td>
        <td> </td>
        <td> 人脸特征点数组长度</td>
    </tr>
    <tr>
        <td> yaw</td>
        <td> float&</td>
        <td> </td>
        <td> yaw方向上的角度</td>
    </tr>
    <tr>
        <td> pitch</td>
        <td> float&</td>
        <td> </td>
        <td> pitch方向上的角度</td>
    </tr>
    <tr>
        <td> roll</td>
        <td> float&</td>
        <td> </td>
        <td> roll方向上的角度</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> bool</td>
        <td> </td>
        <td> true为检测成功</td>
    </tr>
<table>
<br>

#### set
设置相关属性值。其中<br>
**YAW_HIGH_THRESHOLD**: 
yaw方向的分级参数一。<br>
**YAW_LOW_THRESHOLD**: 
yaw方向的分级参数二。<br>
**PITCH_HIGH_THRESHOLD**：
pitch方向的分级参数一。<br>
**PITCH_LOW_THRESHOLD**：
pitch方向的分级参数二。<br>
**ROLL_HIGH_THRESHOLD**：
roll方向的分级参数一。<br>
**ROLL_LOW_THRESHOLD**：
roll方向的分级参数二。<br>

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

## 8 class QualityOfResolution

非深度学习的人脸尺寸评估器。<br>

### 8.1 构造函数

#### QualityOfResolution

人脸尺寸评估器构造函数。<br>

#### QualityOfResolution

人脸尺寸评估器构造函数。<br>

<table border="1">
    <tr>
        <th>参数</th>
        <th>类型</th>
        <th>缺省</th>
        <th>说明</th>
    </tr>
    <tr>
        <td> low</td>
        <td> float</td>
        <td> </td>
        <td> 分级参数一</td>
    </tr>
    <tr>
        <td> high</td>
        <td> float</td>
        <td> </td>
        <td>  分级参数二</td>
    </tr>
<table>
<br>

### 8.2 成员函数

#### check

评估人脸尺寸。<br>

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
        <td> face</td>
        <td> const SeetaRect&</td>
        <td> </td>
        <td>  人脸位置</td>
    </tr>
    <tr>
        <td> points</td>
        <td> const SeetaPointF*</td>
        <td> </td>
        <td> 人脸5个特征点数组</td>
    </tr>
    <tr>
        <td> N</td>
        <td> const int32_t</td>
        <td> </td>
        <td> 人脸特征点数组长度</td>
    </tr>
    <tr>
        <td> 返回值</td>
        <td> QualityResult</td>
        <td> </td>
        <td> 人脸尺寸评估结果</td>
    </tr>
<table>
<br>