#### SV3D_SIL Version Compile FlowMask with Remote_control Tool

#### Reference

> Conference: DesignRef::Vehicle body mask method: Mask.Bin Generation

#### 主要步骤

##### 1、准备文件

* sv3d代码仓库：git clone ssh://git@192.168.12.18:7999/skdgrap/graphic_feature_sv3d.git
* Mask Tool：主要是`cc_sv3d_masking_tool/Masking.exe`文件
* Vehicle_model文件：`sv3d/cc/vehicle_model`已经包含此文件

##### 2、Raw image裁剪

##### 3、更新相机内参xml文件

##### 4、由Masking.exe软件制作mask

##### 5、拷贝mask至`sv3d`目录下

##### 6、编译sv3d sil 版本，修改launch.json文件，运行sv3d

##### 7、启动远程控制端软件cc_remote_control.exe文件，连接sv3d运行端

##### 8、在远程控制端界面的下端输入框中输入mask图像

`mask front.bmp right.bmp rear.bmp left.bmp right.bmp left.bmp`

##### 9、点击enter执行，生成masks.bin文件，位于`cc/resources/`目录下

##### 10、切换相机视角，设定位置，获取新的参数，然后更新相机参数`CodingParameters.xml`文件

