## 用途

本代码用于微信跳一跳的辅助工具，使用opencv库中的模板匹配和边缘检测，联合adb安卓调试工具，实现微信跳一跳的自动刷分。

## 主要使用的Python库及对应版本：
python 3.7.2 
opencv-python 4.1.0 
numpy 1.16.3  

## adb

下载链接：https://adbshell.com/downloads

解压后请将解压后的目录添加到path中保证每个目录都能运行adb.exe

连接手机后请将手机的开发者模式打开，打开usb调试。

## Opencv  
首先介绍下opencv，是一个计算机视觉库，本文将用到opencv里的模板匹配和边缘检测功能。  

### 模板匹配
模板匹配是在一幅图像中寻找一个特定目标的方法之一。这种方法的原理非常简单，遍历图像中的每一个可能的位置，比较各处与模板是否“相似”，当相似度足够高时，就认为找到了我们的目标。

使用OpenCV的matchTemplate函数，就能找到中小人的位置。小人的检测效果非常好，每次都能识别得很精确。
观察到小人跳到物块中心之后，下一个物块中心就会出现白色小圆点，同样可以匹配图中白色小圆点，从而获得跳跃目标点的坐标，计算跳跃的距离。

但是只匹配小圆点获得跳跃目标位置会出现问题，因为有些物块本身就是白色的，导致检测失败，所以我们在检测失败（模板匹配的相似度很低）的情况下采用边缘检测。

### 边缘检测
边缘检测顾名思义就是检测图片中的边缘，使用opencv中的cv2.Canny函数。
跳一跳的画面很简洁，所以边缘检测的效果很好。检测出边缘后，从上至下扫描图片就能找到下一个物块的大致位置。