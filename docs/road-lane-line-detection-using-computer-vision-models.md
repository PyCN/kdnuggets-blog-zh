# 使用计算机视觉模型进行道路车道线检测

> 原文：[`www.kdnuggets.com/2017/07/road-lane-line-detection-using-computer-vision-models.html/2`](https://www.kdnuggets.com/2017/07/road-lane-line-detection-using-computer-vision-models.html/2)

![](img/ea9c916ec7097bd41c50b754c209edfd.png)

***图. 霍夫变换识别图像中的车道线***

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的快车道

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

**步骤 5：外推各个小线段并构建左侧和右侧车道线**

霍夫变换根据霍夫空间中的交点给我们小线段。现在，我们可以利用这些信息来构建一个全局的左车道线和右车道线。

我们通过将小线段分成两组来实现这一点，一组具有正梯度，另一组具有负梯度。由于摄像机视角的深度，车道线在深度方向上会向彼此倾斜，因此它们应该具有相反的梯度。

然后，我们取每组的平均梯度和截距值，并从中构建我们的全局车道线。车道线随后被外推到具有最小 y 轴坐标和具有最大 y 轴坐标的边缘检测像素。

要实现这个算法，我们只需将之前声明的 `draw_lines` 函数更改为外推这些线条。

```py
---------------------------------------------------------------------------
```

```py
def draw_lines(img, lines, color=[255, 0, 0], thickness=2):
    """
    This function draws `lines` with `color` and `thickness`.    
    """
    imshape = img.shape

    # these variables represent the y-axis coordinates to which 
    # the line will be extrapolated to
    ymin_global = img.shape[0]
    ymax_global = img.shape[0]

    # left lane line variables
    all_left_grad = []
    all_left_y = []
    all_left_x = []

    # right lane line variables
    all_right_grad = []
    all_right_y = []
    all_right_x = []

    for line in lines:
        for x1,y1,x2,y2 in line:
            gradient, intercept = np.polyfit((x1,x2), (y1,y2), 1)
            ymin_global = min(min(y1, y2), ymin_global)

            if (gradient > 0):
                all_left_grad += [gradient]
                all_left_y += [y1, y2]
                all_left_x += [x1, x2]
            else:
                all_right_grad += [gradient]
                all_right_y += [y1, y2]
                all_right_x += [x1, x2]

    left_mean_grad = np.mean(all_left_grad)
    left_y_mean = np.mean(all_left_y)
    left_x_mean = np.mean(all_left_x)
    left_intercept = left_y_mean - (left_mean_grad * left_x_mean)

    right_mean_grad = np.mean(all_right_grad)
    right_y_mean = np.mean(all_right_y)
    right_x_mean = np.mean(all_right_x)
    right_intercept = right_y_mean - (right_mean_grad * right_x_mean)

    # Make sure we have some points in each lane line category
    if ((len(all_left_grad) > 0) and (len(all_right_grad) > 0)):
        upper_left_x = int((ymin_global - left_intercept) / left_mean_grad)
        lower_left_x = int((ymax_global - left_intercept) / left_mean_grad)
        upper_right_x = int((ymin_global - right_intercept) / right_mean_grad)
        lower_right_x = int((ymax_global - right_intercept) / right_mean_grad)

        cv2.line(img, (upper_left_x, ymin_global), 
                      (lower_left_x, ymax_global), color, thickness)
        cv2.line(img, (upper_right_x, ymin_global), 
                      (lower_right_x, ymax_global), color, thickness)

```

```py
---------------------------------------------------------------------------
```

![](img/16dc937a2130155cb07452636d69d1b1.png)

***图. 外推车道线***

**步骤 6：将外推的线条添加到输入图像中**

然后，我们将外推的线条叠加到输入图像上。我们通过根据检测到的车道线坐标将权重值添加到原始图像来实现这一点。

```py
---------------------------------------------------------------------------
```

```py
def weighted_img(img, initial_img, α=0.8, β=1., λ=0.):
    """
    `img` is the output of the hough_lines(), An image with lines drawn on it.
    Should be a blank image (all black) with lines drawn on it.

    `initial_img` should be the image before any processing.

    The result image is computed as follows:

    initial_img * α + img * β + λ
    NOTE: initial_img and img must be the same shape!
    """
    return cv2.addWeighted(initial_img, α, img, β, λ)

# outline the input image
colored_image = weighted_img(houged, image)

```

```py
---------------------------------------------------------------------------
```

![](img/33819c112302ac07f6491d7a0c59d324.png)

***图. 叠加在输入图像上的车道线***

**步骤 7：将管道添加到视频中**

一旦我们设计并实现了整个车道检测管道，我们就会逐帧应用变换。我们需要一段车辆在车道线上的视频来完成这项工作。

```py
---------------------------------------------------------------------------
```

```py
from moviepy.editor import VideoFileClip
from IPython.display import HTML

def process_image(image):
    # grayscale the image
    grayscaled = grayscale(image)

    # apply gaussian blur
    kernelSize = 5
    gaussianBlur = gaussian_blur(grayscaled, kernelSize)

    # canny
    minThreshold = 100
    maxThreshold = 200
    edgeDetectedImage = canny(gaussianBlur, minThreshold, maxThreshold)

    # apply mask
    lowerLeftPoint = [130, 540]
    upperLeftPoint = [410, 350]
    upperRightPoint = [570, 350]
    lowerRightPoint = [915, 540]

    pts = np.array([[lowerLeftPoint, upperLeftPoint, upperRightPoint, 
                    lowerRightPoint]], dtype=np.int32)
    masked_image = region_of_interest(edgeDetectedImage, pts)

    # hough lines
    rho = 1
    theta = np.pi/180
    threshold = 30
    min_line_len = 20 
    max_line_gap = 20

    houged = hough_lines(masked_image, rho, theta, threshold, min_line_len, 
                         max_line_gap)

    # outline the input image
    colored_image = weighted_img(houged, image)
    return colored_image

output = 'car_lane_detection.mp4'
clip1 = VideoFileClip("insert_car_lane_video.mp4")
white_clip = clip1.fl_image(process_image)
%time white_clip.write_videofile(output, audio=False)

```

```py
---------------------------------------------------------------------------
```

就这样：一个端到端的 Python 车道检测实现！

[原文](https://github.com/vijay120/KDNuggets/blob/master/2016-12-04-detecting-car-lane-lines-using-computer-vision.md)。经许可转载。

**个人简介：** [Vijay Ramakrishnan](https://www.linkedin.com/in/viramakrishnan/) 是 Mindmeld Inc 的机器学习工程师，该公司是一家 Cisco 于 2017 年收购的对话 AI 公司。曾担任 Qualcomm 的项目经理和 Google 的数据工程师。喜欢扩展机器学习系统、开发自动驾驶技术和对话 AI 产品。

**相关内容：**

+   理解计算机视觉的 7 个步骤

+   漫画：史上首个自动驾驶深度学习烧烤架

+   获取自动驾驶车辆入门的 5 个免费资源

### 更多相关话题

+   [DINOv2：Meta AI 的自监督计算机视觉模型](https://www.kdnuggets.com/2023/05/dinov2-selfsupervised-computer-vision-models-meta-ai.html)

+   [TensorFlow 用于计算机视觉 - 转移学习变得简单](https://www.kdnuggets.com/2022/01/tensorflow-computer-vision-transfer-learning-made-easy.html)

+   [探索计算机视觉的世界：介绍 MLM 的最新…](https://www.kdnuggets.com/2024/01/mlm-discover-the-world-of-computer-vision-ebook)

+   [计算机视觉的 5 个应用](https://www.kdnuggets.com/2022/03/5-applications-computer-vision.html)

+   [关于数据管理和其重要性你需要知道的 6 件事…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)

+   [KDnuggets 新闻 2022 年 3 月 9 日：5 步构建机器学习 Web 应用](https://www.kdnuggets.com/2022/n10.html)
