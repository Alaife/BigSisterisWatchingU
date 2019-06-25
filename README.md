# BigSisterisWatchingU
Flame CV Feature Project &amp; Schedule

[TOC]

***


### 0. 当前进度【2019年6月24日 星期一】

- [ ] **图像预处理**
    - [x] 图像裁切
    - [x] 彩色视频转灰度
    - [x] 图像滤波处理
        - [x] 均值滤波
        - [x] 中值滤波
        - [x] 高斯滤波
        - [x] 高斯双边滤波
    - [ ] 对比度增强
        - [ ] 线性变换

        - [ ] 直方图正规化

        - [ ] 伽马变换

        - [ ] 全局直方图均衡化

        - [ ] 自适应直方图均衡化

        - [ ] 对比度拉伸
    - [x] 将图像逐帧保存问图片
- [ ] **图像特征提取（1）** ——*艾徐华*
    - [ ] 黑龙长度
    - [ ] 分形特征
    - [ ] 偏心距离
- [ ] **图像特征提取（2）**——*白晓静*
- [ ] **图像特征提取（3）**——*其他*
    - [x] 对图像灰度进行逐帧统计
***

### 1. 图像预处理部分

##### 1.1. 图像裁切

```python
import cv2 as cv
'''
这个程序用来裁剪视频（并不一定用得上）
'''
# 读入视频
video_capture = cv.VideoCapture('【郭琦毕业论文数据】/Video_1.mpg')

# 获取读入视频的参数
fps = video_capture.get(cv.CAP_PROP_FPS)  ##获取帧率
width = video_capture.get(cv.CAP_PROP_FRAME_WIDTH)  ##获取宽度
height = video_capture.get(cv.CAP_PROP_FRAME_HEIGHT)  ##获取高度
print('视频帧率（FPS）:', fps)
print('视频宽度（Width）:', width)
print('视频高度（Height）:', height)

# 定义截取的窗口尺寸
size = (int(600), int(100))  # (height, width)

# 创建视频写入对象
video_writer = cv.VideoWriter("【郭琦毕业论文数据】/test.avi",
                               cv.VideoWriter_fourcc('M', 'J', 'P', 'G'),
                               fps,
                              size)

# 读取视频帧，然后写入文件并在窗口显示
success, frame = video_capture.read()
while success and not cv.waitKey(1) == 27:
    # 定义截取框的位置
    frame = frame[100:200, 100:700]  # [width, height] 要与上面定义的分辨率一致
    video_writer.write(frame)  # 写入视频文件
    cv.imshow("video", frame)
    success, frame = video_capture.read()

# 回收资源
cv.destroyAllWindows()
video_writer.release()
video_capture.release()
```



##### 1.2. 将彩色视频转换为灰度视频

$$
Gray = R×0.299 + G×0.587+B×0.114
$$

```python
import cv2 as cv
'''
这个程序将彩色转换成灰度
'''
# 读入视频
video_capture = cv.VideoCapture('F:\BigSisterisWatchingU\【郭琦毕业论文数据】\Video_1.mpg')

isOpened = video_capture.isOpened()  ##判断视频是否打开
print(isOpened)

# 获取读入视频的参数
fps = video_capture.get(cv.CAP_PROP_FPS)  ##获取帧率
width = video_capture.get(cv.CAP_PROP_FRAME_WIDTH)  ##获取宽度
height = video_capture.get(cv.CAP_PROP_FRAME_HEIGHT)  ##获取高度
print('视频帧率（FPS）:', fps)
print('视频宽度（Width）:', width)
print('视频高度（Height）:', height)

size = (int(height),int(width))

video_writer = cv.VideoWriter('【郭琦毕业论文数据】/test.avi',
                     cv.VideoWriter_fourcc('M', 'J', 'P', 'G'),
                     fps,
                     size,
                     False)
#（写出的文件，编码，帧率，（分辨率），是否彩色）  非彩色要把每一帧图像装换成灰度图
while(video_capture.isOpened()):
    ret, frame = video_capture.read()
    if ret==True:
        frame = cv.cvtColor(frame, cv.COLOR_BGR2GRAY) #转换成灰度视频
        video_writer.write(frame)
        cv.imshow('frame',frame)
        if cv.waitKey(1) & 0xFF == ord('q'):
            break
    else:
        break
# 回收资源
video_capture.release()
video_writer.release()
cv.destroyAllWindows()
```



##### 1.3. 对图像（视频）进行各种滤波处理

###### 1.3.1. 均值滤波

###### 1.3.2. 中值滤波

###### 1.3.3. 高斯滤波

###### 1.3.4. 高斯双边滤波

```python
import cv2 as cv

# 读入视频
video_capture = cv.VideoCapture('【郭琦毕业论文数据】/Video_1_Grey.avi')

isOpened = video_capture.isOpened()  ##判断视频是否打开
print(isOpened)

# 获取读入视频的参数
fps = video_capture.get(cv.CAP_PROP_FPS)  ##获取帧率
width = video_capture.get(cv.CAP_PROP_FRAME_WIDTH)  ##获取宽度
height = video_capture.get(cv.CAP_PROP_FRAME_HEIGHT)  ##获取高度
print('视频帧率（FPS）:', fps)
print('视频宽度（Width）:', width)
print('视频高度（Height）:', height)

size = (int(width),int(height))

video_writer = cv.VideoWriter('【郭琦毕业论文数据】/Video_1_XXXFilter.avi',
                     cv.VideoWriter_fourcc('M', 'J', 'P', 'G'),
                     fps,
                     size,
                     1)

while( video_capture.isOpened()):
    ret, frame = video_capture.read()
    if ret == True:
        '''
        几种滤波自选
        '''
        #frame = cv.blur(frame, (5, 5)) ###均值滤波，（输入图像,核与源图像的对齐方式）
        #frame = cv.medianBlur(frame, 5) ###中值滤波，（输入图像,滤波模板的尺寸大小）
        #frame = cv.GaussianBlur(frame,(7,7),0) ###高斯滤波，（输入图像,（高斯滤波x，高斯滤波y），bordertype参数）
        frame = cv.bilateralFilter(frame,40,75,75) ###高斯双边滤波，(输入图像，)
        video_writer.write(frame)
        cv.imshow('frame', frame)
        if cv.waitKey(1) & 0xFF == ord('q'):
            break
    else:
        break
# 回收资源
video_capture.release()
video_writer.release()
cv.destroyAllWindows()
```



##### 1.4. 对比度增强

###### 1.4.1. 线性变换

###### 1.4.2. 直方图正规化

###### 1.4.3.伽马变换

###### 1.4.4. 全局直方图均衡化

###### 1.4.5.自适应直方图均衡化—— ==白晓静==

###### 1.4.6.对比度拉伸——==艾徐华==

##### 1.5. 将图像逐帧保存为图片

```python 
import cv2 as cv
'''
这个程序用来把视频按帧保存
'''
# 读入视频
video_capture = cv.VideoCapture("F:\BigSisterisWatchingU\【郭琦毕业论文数据】\Video_3_Grey.avi")

# 获取读入视频的参数
fps = video_capture.get(cv.CAP_PROP_FPS)  ##获取帧率
width = video_capture.get(cv.CAP_PROP_FRAME_WIDTH)  ##获取宽度
height = video_capture.get(cv.CAP_PROP_FRAME_HEIGHT)  ##获取高度
print('视频帧率（FPS）:', fps)
print('视频宽度（Width）:', width)
print('视频高度（Height）:', height)

i = 0
while(video_capture.isOpened()):
    ret,frame = video_capture.read()
    i += 1
    if ret == True:
        cv.imwrite("E:\DataSet\Video_3_" + str(i) + ".jpg", frame, [cv.IMWRITE_JPEG_CHROMA_QUALITY, 100])
        fileName = "image" + str(i) + ".jpg"
        print(fileName)
        if cv.waitKey(1) & 0xFF == ord('q'):
            break
    else:
        break

# 回收资源
video_capture.release()
cv.destroyAllWindows()
```

---

### 2. 图像特征提取（1）——via.艾徐华论文

##### 2.1. "黑龙"长度计算-BlackDragonLength

##### 2.2. 分形特征检测-Fractal

##### 2.3. 偏心距离-Centroid

***

### 3.图像特征提取（2）——via.白晓静论文

##### 3.1. 

##### 3.2. 

##### 3.3. 

***

### 4.图像特征提取（3）——via.其他来源

##### 4.1. 对图像灰度进行逐帧统计

**输出形式：**

| 灰度值（GS） | t=0                              | t=1  | ...  | t=T~max~ |
| ------------ | -------------------------------- | ---- | ---- | -------- |
| 0            | T~0~时刻，灰度0的像素点个数/频率 |      |      |          |
| 1            |                                  |      |      |          |
| ...          |                                  |      |      |          |
| 255          |                                  |      |      |          |



***

### 5. 图像分割——via.白晓静论文

##### 5.0 目标特点&采集硬件特点

1. 高速相机，图像数量大

2. 火焰的图像相对简单

##### 5.1. 最大类间方差动态选取阈值（Otsu）

1. 背景亮度高、亮度分布不均匀，Otsu选取的阈值较大，无法准确分割。
2. 在测试的四种方法中，对于椒盐噪声最敏感。

##### 5.2. K-Means

结果上优于Otsu

1. 火焰有局部的亮度比较低，在分割过程中会被分为背景，从而出现孔洞（需要进行额外的孔洞填充处理）。
2. 相对Otsu，对椒盐噪声的敏感程度最低。

##### 5.3. FCM

结果上与K-Means相类似。

##### 5.4.  MCWT

1. 由于综合了颜色和纹理特征两种因素，避免了由于火焰的低亮度区和背景的高亮度区两方面的影响。
2. 在测试的四种方法中对椒盐噪声最不敏感。



   

