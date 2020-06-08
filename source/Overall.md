# SDK整体介绍

此次放出的SDK模块包含**人脸检测(FaceDetector)**、**关键点定位(FaceLandmarker)**、**人脸识别(FaceRecognizer)**、**人脸跟踪(FaceTracker)**、**质量评估(QualityOfLBN等模块)**、**单目的静默活体(FaceAntiSpoofing)** 和 **近红外活体模块(SeetaNirFaceAntiSpoofing)**。

## 使用入门

请下载人脸识别批量测试工具Android Studio，参考人脸识别批量测试工具工程以及SDK接口文档熟悉SDK的使用方法。

## 接口文档

各个模块接口和参数含义请参考接口文档。

## 模块速度

各个模块的速度测试都是基于硬件平台RK3399Pro，其中输入的图像大小为640x480。

<table border="1">
        <tr>
            <th>模块名称</th>
            <th>速度（ms）</th>
            <th>备注</th>
        </tr>
         <tr>
            <td>人脸检测 </td>
            <td> 64 </td>
            <td> 最小人脸属性40 </td>
        </tr>
         <tr>
            <td> 关键点定位</td>
            <td> 21</td>
            <td> </td>
        </tr>
         <tr>
            <td> 人脸识别</td>
            <td> 59 </td>
            <td> </td>
        </tr>
         <tr>
            <td> 姿态估计(深度方法)</td>
            <td> 7</td>
            <td> </td>
        </tr>
         <tr>
            <td> 清晰度检测(深度方法)</td>
            <td> 33</td>
            <td> </td>
        </tr>
         <tr>
            <td> 单目静默活体(加载一个模型)</td>
            <td> 56</td>
            <td> </td>
        </tr>
         <tr>
            <td> 单目静默活体(加载两个模型)</td>
            <td> 144</td>
            <td> 加载两个模型活体检测精度更高，但速度较慢</td>
        </tr>
         <tr>
            <td> 近红外活体</td>
            <td> 32</td>
            <td> </td>
        </tr>
</table>
<br>

## 识别精度

在自有数据集上测试，人脸识别精度可达到99%以上。

## 联系我们

如果需要更多的商业支持，可随时联系我们。