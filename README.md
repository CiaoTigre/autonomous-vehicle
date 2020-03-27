# 自动驾驶小车课程设计

## （一）项目描述
该项目旨在设计并实现一种自主驾驶小车，通过在小车正前方搭载摄像头设备，使得小车能够识别出一些既定的物体（此处视为障碍物），进而小车能够规划出一条路径绕过前面的障碍物，最终实现小车的自动驾驶。


## （二）具体实现
* **硬件配置**：Tank移动平台、RPI（主控制器）、Arduino MEGA（协控制器）、摄像头等
* **软件设计**：TensoFlow-Lite + OpenCV实现目标检测算法、规则制定、导航控制

系统框架如下：



项目中使用树莓派RPI作为主控制器，上面运行着目标检测算法，目前使用的模型来源于Google提供的示例TFLite模型，存放在项目文件夹的\autonomous-vehicle\coco_ssd_mobilenet_v1_1.0_quant_2018_06_29\目录下，其中包含detect.tflite模型以及包含模型能够检测的常见80种物体的标签。
微控制器Arduino作为协处理器，与RPI通过串口进行通讯，主要用于接收主控制器的决策数据，进而生成控制信号以驱动小车执行相应的运动。

-------------------------------------------------------------------------------------------------------------------
COCO数据集是微软团队发布的一个可以用来图像recognition+segmentation+captioning的数据集，该数据集收集了大量包含常见物体的日常场景图片，并提供像素级的实例标注以更精确地评估检测和分割算法的效果，致力于推动场景理解的研究进展

量化模型使用8位整数值而不是神经网络中的32位浮点值，从而使它们可以在GPU或专用TPU（TensorFlow处理单元）上更高效地运行。

下载谷歌提供量化的模型

您也可以使用标准的SSD-MobileNet模型（V1或V2），但运行速度不如量化模型快。

练习将模型转换为TensorFlow Lite的过程，则可以下载量化的MobileNet-SSD模型

注意：TensorFlow Lite不支持诸如Faster-RCNN之类的RCNN模型！它仅支持SSD型号。


## （三）未来工作

将使用SSD-MobileNet-V2-Quantized-COCO模型

There are three primary steps to training and deploying a TensorFlow Lite model:

* Train a quantized SSD-MobileNet model using TensorFlow, and export frozen graph for TensorFlow Lite

I’ll use transfer learning to train a “quantized” SSD-MobileNet model. Quantized models use 8-bit integer values instead of 32-bit floating values within the neural network, allowing them to run much more efficiently
* Build TensorFlow from source on my laptop

To convert the frozen graph we just exported into a model that can be used by TensorFlow Lite, it has to be run through the TensorFlow Lite Optimizing Converter (TOCO). Unfortunately, to use TOCO, we have to build TensorFlow from source on our computer.
* Use TensorFlow Lite Optimizing Converter (TOCO) to create optimzed TensorFlow Lite model

we still need run it through the TensorFlow Lite Optimizing Converter (TOCO) before it will work with the TensorFlow Lite interpreter. TOCO converts models into an optimized FlatBuffer format that allows them to run efficiently on TensorFlow Lite. We also need to create a new label map before running the model.
