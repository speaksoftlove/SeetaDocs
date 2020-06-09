# SDK整体介绍

此次放出的SDK模块包含**人脸检测(FaceDetector)**、**关键点定位(FaceLandmarker)**、**人脸识别(FaceRecognizer)**、**人脸跟踪(FaceTracker)**、**质量评估(QualityOfLBN等模块)**、**单目的静默活体(FaceAntiSpoofing)** 和 **近红外活体模块(SeetaNirFaceAntiSpoofing)**。

## 使用入门

请下载人脸识别批量测试Android Studio工程，参考人脸识别批量测试工具源码工程以及SDK接口文档熟悉SDK的使用方法。

为了更直观的了解各个模块的使用流程，在此以代码方式对各个场景进行讲解：<br>

    
    Bitmap readBitmap(String imagePath)
    {//读取图像数据 ARGB_8888格式

        BitmapFactory.Options options = new BitmapFactory.Options();
        options.inPreferredConfig = Bitmap.Config.ARGB_8888;
        Bitmap bitmap = BitmapFactory.decodeFile(imagePath);//读取图像数据1
        return bitmap;
    } 
    
    public byte[] getPixelsBGR(Bitmap image) 
    {//转换Bitmap格式的数据到 SDK需要的BGR 888 格式的数据，并返回转换好的图像数据byte[]
        int bytes = image.getByteCount();

        ByteBuffer buffer = ByteBuffer.allocate(bytes); // Create a new buffer
        image.copyPixelsToBuffer(buffer); // Move the byte data to the buffer

        byte[] temp = buffer.array(); // Get the underlying array containing the data.

        byte[] pixels = ImageDataFormatUtils.ARGBToBGR(temp, image.getWidth(), image.getHeight());
        //其中ImageDataFormatUtils为提供过去的图像数据格式转换类
        return pixels;
    }
    
    SeetaImageData seetaConvert(Bitmap bitmap)
    {//转换Bitmap到SDK需要输入的SeetaImageData图像格式

        SeetaImageData seetaImageData = new SeetaImageData(bitmap.getWidth(), bitmap.getHeight(), 3);
        seetaImageData.data = getPixelsBGR(bitmap);
        return seetaImageData;
    }

    SeetaImageData readSeetaImage(String imagePath)
    {
        Bitmap bitmap = readBitmap(imagePath);
        SeetaImageData image = seetaConvert(bitmap);
        return image;
    }

    int GetMaxFace(SeetaRect[] faceRects)
    {//获取其中最大尺寸的人脸
        int maxIndex = -1;

        int maxSize = 0;
        for(int i = 0; i < faceRects.length; ++i)
        {
            int currentSize = faceRects[i].width * faceRects[i].height;
            if(currentSize > maxSize)
            {
                maxSize = currentSize;
                maxIndex = i;
            }
        }

        return maxIndex;
    }
    
#### 人脸识别代码流程

    String imagePath1 = "/sdcard/seeta/test1.jpg";
    String imagePath2 = "/sdcard/seeta/test2.jpg";
    SeetaImageData image1 = readSeetaImage(imagePath1);//读取图像一
    SeetaImageData image2 = readSeetaImage(imagePath2);//读取图像二
    
    String modelPath = "/sdcard/seeta/";//模型文件路径
    String device = "rknn";//实际计算的设备
    int id = 0;//device id
    
    FaceDetector FD = new FaceDetector(modelPath + "lffd_v2_wwm_nobn_bgs_1.rk3399pro.[].[].no-compile.do-quantization.asymmetric_quantized-u8.sta", device, id);//人脸检测对象初始化
    
    FaceLandmarker FL = new FaceLandmarker(modelPath + "SeetaFaceLandmarker5.0.pts5.tsm.sta", "cpu", 0);//关键点定位对象初始化，注意对象计算使用CPU计算 
    
    FaceRecognizer FR = new FaceRecognizer(modelPath + "mobilefacenet-v1_arcface_lffd-plfd_0519.rk3399pro.[].[].no-compile.no-quantization.float16.sta", device, id);//人脸识别对象初始化

    SeetaRect[] faces1 = FD.Detect(image1);//检测图像一中的人脸
    SeetaRect[] faces2 = FD.Detect(image2);//检测图像二中的人脸
    if(faces1.length > 0 && faces2.length > 0)
    {
        int maxIndex1 = GetMaxFace(faces1);//图像一最大人脸索引
        int maxIndex2 = GetMaxFace(faces2);//图像二中最大人脸索引
        
        //关键点分配内存
        SeetaPointF[] landmarks1 = new SeetaPointF[FL.number()];
        SeetaPointF[] landmarks2 = new SeetaPointF[FL.number()];
        //关键点定位
        FL.mark(image1, faces1[maxIndex1], landmarks1);
        FL.mark(image2, faces2[maxIndex2], landmarks2);
        //人脸特征分配内存
        float[] feats1 = new float[FR.GetExtractFeatureSize()];
        float[] feats2 = new float[FR.GetExtractFeatureSize()];
        //人脸特征提取
        FR.Extract(image1, landmarks1, feats1);
        FR.Extract(image2, landmarks2, feats2);
        //获取两张最大人脸的相似度
        float similarity = FR.CalculateSimilarity(feats1, feats2);//相似度大于设定的阈值就认定为是同一个人，阈值推荐为0.4
    }
#### 单目静默活体流程

    String imagePath = "/sdcard/seeta/test.jpg";
    SeetaImageData image = readSeetaImage(imagePath);//读取图像
    
    String modelPath = "/sdcard/seeta/";//模型文件路径
    String device = "rknn";//实际计算的设备
    int id = 0;//device id
    
    FaceDetector FD = new FaceDetector(modelPath + "lffd_v2_wwm_nobn_bgs_1.rk3399pro.[].[].no-compile.do-quantization.asymmetric_quantized-u8.sta", device, id);//人脸检测对象初始化
    
    FaceLandmarker FL = new FaceLandmarker(modelPath + "SeetaFaceLandmarker5.0.pts5.tsm.sta", "cpu", 0);//关键点定位对象初始化，注意对象计算使用CPU计算
    
    FaceAntiSpoofing Fas = new FaceAntiSpoofing(modelPath + "_acc982_res18_longmao1th_1022_11000.rk3399pro.[].[].no-compile.do-quantization.asymmetric_quantized-u8.sta", modeoPath + "SeetaAntiSpoofing.plg.1.0.m01d29.rk3399pro.[].[].no-compile.do-quantization.asymmetric_quantized-u8.sta", device, id);
    //静默活体对象初始化，速度慢的话可以尝试只使用第一个模型文件进行初始化
    
    SeetaRect[] faces = FD.Detect(image);//检测图像中的人脸
    if(faces.length > 0)
    {
        int maxIndex = GetMaxFace(faces);//图像中最大人脸索引
        
        //关键点分配内存
        SeetaPointF[] landmarks = new SeetaPointF[FL.number()];
        //关键点定位
        FL.mark(image, faces[maxIndex], landmarks);
        //静默活体检测
        FaceAntiSpoofing.Status status = Fas.Predict(image, faces[maxIndex], landmarks);
    }

#### 近红外活体流程

    String imagePath = "/sdcard/seeta/rgb.jpg";
    String imageNirPath = "/sdcard/seeta/nir.jpg";
    SeetaImageData image = readSeetaImage(imagePath);//读取彩色图像
    SeetaImageData nirImage =readSeetaImage(imageNirPath);//读取近红外图像
    
    String modelPath = "/sdcard/seeta/";//模型文件路径
    String device = "rknn";//实际计算的设备
    int id = 0;//device id
    
    FaceDetector FD = new FaceDetector(modelPath + "lffd_v2_wwm_nobn_bgs_1.rk3399pro.[].[].no-compile.do-quantization.asymmetric_quantized-u8.sta", device, id);//人脸检测对象初始化
    
    FaceLandmarker FL = new FaceLandmarker(modelPath + "SeetaFaceLandmarker5.0.pts5.tsm.sta", "cpu", 0);//关键点定位对象初始化，注意对象计算使用CPU计算
    
   SeetaNirFaceAntiSpoofing nirFas = new SeetaNirFaceAntiSpoofing(modelPath + "nirFaceAntiSpoofingGrayV1.0.4.rk3399pro.[].[].no-compile.do-quantization.asymmetric_quantized-u8.sta", device ,id);//近红外对象初始化
    
    SeetaRect[] faces = FD.Detect(image);//检测图像中的人脸
    if(faces.length > 0)
    {
        int maxIndex = GetMaxFace(faces);//图像中最大人脸索引
        
        //关键点分配内存
        SeetaPointF[] landmarks = new SeetaPointF[FL.number()];
        //关键点定位
        FL.mark(image, faces[maxIndex], landmarks);
        //近红外活体检测
        SeetaNirFaceAntiSpoofing.Status status = nirFas.predict(nirImage, faces[maxIndex], landmarks);
    }

#### 质量评估
 质量评估模块能够对人脸的质量进行检测，通过此模块可以有效过滤掉质量较差的不符合要求的人脸，例如姿态过大、过暗过亮，模糊的人脸等，可以有效提高人脸识别的精度和活体检测的精度。这里以人脸的亮度检测、清晰度检测和姿态检测为例说明质量评估SDK的使用流程。

    String imagePath = "/sdcard/seeta/test.jpg";
    SeetaImageData image = readSeetaImage(imagePath);//读取图像
    
    String modelPath = "/sdcard/seeta/";//模型文件路径
    String device = "rknn";//实际计算的设备
    int id = 0;//device id
    
    FaceDetector FD = new FaceDetector(modelPath + "lffd_v2_wwm_nobn_bgs_1.rk3399pro.[].[].no-compile.do-quantization.asymmetric_quantized-u8.sta", device, id);//人脸检测对象初始化
    
    FaceLandmarker FL = new FaceLandmarker(modelPath + "SeetaFaceLandmarker5.0.pts5.tsm.sta", "cpu", 0);//5点关键点定位对象初始化，注意对象计算使用CPU计算
    
    FaceLandmarker FL68 = new FaceLandmarker(modelPath + "SeetaFaceLandmarker5.0.pts68.tsm.sta", "cpu", 0);//68点关键点定位对象初始化,注意对象计算使用CPU计算
    
    QualityOfBrightness bright = new QualityOfBrightness();//亮度检测对象初始化
    
    QualityOfLBN lbn = new QualityOfLBN(modelPath + "_loss034018_squeezenetV22_blur1_2th_relabel_18000_1209.rk3399pro.[].[].no-compile.do-quantization.asymmetric_quantized-u8.sta", device, id);//清晰度检测对象初始化
    
    QualityOfPoseEx poseEx = new QualityOfPoseEx(modelPath + "PoseEstimation1.1.0.rk3399pro.[].[].no-compile.do-quantization.asymmetric_quantized-u8.sta", device, id);//姿态估计对象初始化

     SeetaRect[] faces = FD.Detect(image);//检测图像中的人脸
    if(faces.length > 0)
    {
        int maxIndex = GetMaxFace(faces);//图像中最大人脸索引
        
        //关键点分配内存
        SeetaPointF[] points5 = new SeetaPointF[FL.number()];
        SeetaPoinF[] points68 = new SeetaPointF[FL68.number()];
        //关键点定位
        FL.mark(image, faces[maxIndex], points5);
        FL.mark(image, faces[maxIndex], points68);
        
        //亮度检测
        float[] brightScore = new float[1];
        QualityLevel brightLevel = bright.check(image, faces[maxIndex], points5, brightScore);
        
        //清晰度检测
        int[] light = new int[1];
        int[] blur = new int[1];
        int[] noise = new int[1];
        lbn.Detect(image, points68, light, blur, noise);//其中blur[0]==0表示图像中人脸清晰，blur[0]==1表示图像中人脸模糊
        
        //姿态估计
        float[] poseScore = new float[1];
        QualityLevel poseLevel = poseEx.check(image, faces[maxIndex], points5, poseScore);
    }
    
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
            <td> 单目静默活体(单模型)</td>
            <td> 56</td>
            <td> </td>
        </tr>
         <tr>
            <td> 单目静默活体(双模型)</td>
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

根据以上测试数据可以得到：<br>
**单次识别**时间为 人脸检测 + 关键点定位 + 人脸识别=(64 + 21 + 59)=144ms。<br>
**单次单目静默活体**时间为 人脸检测 + 关键点定位 + 单目静默活体(单模型)=(64 + 21 + 56)=141ms 或者 人脸检测 + 关键点定位 + 单目静默活体(双模型)=(64 + 21 + 144)=229ms。<br> 
**单次近红外活体**时间为 人脸检测 + 关键点定位 + 近红外活体=(64 + 21 + 32)=117ms。

## 识别精度

在自有识别数据集上测试，人脸识别精度可达到99%以上。

## 联系我们

如果需要更多的商业支持，可随时联系我们。