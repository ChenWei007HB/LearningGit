### SV3D项目

#### SV3D_remote控制软件

#### SV3D项目代码流程

##### sv3d

* bind/gcc/target_standalone：gcc/g++编译后的文件，包括依赖库(.d文件)和目标文件(.o文件)
  * cc
  * generated
  * hw
  * pc
* cc：
  * target
    * standalone
      * src/sv3d_main.cpp
      * inc/CustomSystemConf.h
      * inc/sv3d_main.h
      * Makefile
* generated
* hw
* jenkins
* pc

#### 数据接收与数据处理

#### 不同传感器信号的接收与处理方式

#### 图像拼接与虚拟相机

> 此处的图像拼接(Image Stitching)已经作为工具化的处理方式；

##### VirtualCamera与ImageStitching之间的联系

* cc
  * virtcam
    * CameraPositions
    * CustomCameraSteeringUpdateCallback
    * HeadUnitHemisphereCameraUpdater
    * HemisphereCameraUpdater

* pc
  * svs
    * factory
    * virtcam
      * CameraFirstPersonManipulator
      * CameraGestureManipulator
      * CameraSteeringUpdateCallback
      * CameraManipulatorManager

* **VirtualCam包含的views**
  * auto1
  * auto2
  * bird --> Surroundview
  * birdZoom
  * bumperZoomFront
  * bumperZoomRear
  * 



#### OSG三维渲染建模开源库的使用

##### OSG库的主要构成

* osg
* osgViewer
* 