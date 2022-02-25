## Debug for GuangDa Camera

### 1、SV3D模块编译(Specified for ARM-end)

#### 1、获取SV3D develop分支最新代码

```shell
git clone ssh://git@192.168.12.18:7999/skdgrap/graphic_feature_sv3d.git # 获取项目远程仓库
cd sv3d
git branch # 查看当前分支是否为develop分支

# 如果不是develop分支，执行此部分
git branch -a # 查看所有分支
git checkout origin/develop # 切换至develop分支
git pull # 拉取远程最新版本的develop分支代码(pull相当于fetch远程+merge远程和本地)

# 拉取SV3D依赖的两个子模块idc_soc_perception_lfs和tda4_rots
git submodule sync --recursive # 同步子模块
git submodule update --init --recursive # 更新子模块

cd idc_soc_perception_lfs
git checkout feature/PDCU-1154-map-apa-parking-slot-information-to-sv3d-for-hmi-display # 切换至此最新分支
git submodule sync --recursive # 同步该模块下的子模块
git submodule update --init --recursive # 更新该模块下的子模块
```

#### 2、修改malisc编译器路径

```makefile
# located in ./sv3d/hw/build/tools/shaderc/malisc/compiler.mk
#-MALISC ?= /home/wws/2T/04-src/02-SV3D/Mali_Offline_Compiler_v6.2.0/malisc
+MALISC ?= /home/YOUR_USER_NAME/SV3D_Env/Mali_Offline_Compiler_v6.2.0/malisc
```

#### 3、安装GLES2依赖库

```shell
sudo apt-get install libgles2-mesa-dev # 不安装这个依赖库，编译ARM端的SV3D时会报错

# 报错如下
# Fatal Error: GLES2/gl2.h:No such file or directory
```

#### 4、编译SV3D(ARM-end)，生成SV3D动态库

```shell
cd sv3d
make DEBUG=1 -j8 all
```





### 2、Flight (FLexible InteGration cHain and Toolset)

> IDC模块中集成SV3D模块生成的动态库

#### 1、安装FastDDS和IDDS模块

```shell
# Install FastDDS package
git clone ssh://git@192.168.12.18:7999/linux/imotion_eprosima_fast_dds_linux.git
cd imotion_eprosima_fast_dds_linux
sh install.sh

# Install IDDS package
git clone ssh://git@192.168.12.18:7999/linux/imotion_dds_cli_tool.git
cd imotion_dds_cli_tool
sh scripts/install.sh
# 注：若安装了ros2，则先在终端中执行 unset CMAKE_PREFIX_PATH ，再安装 iDDS
# 安装完idds后配置idds的环境变量DDS_DOMAIN_ID和DDS_DYNTYPE_XML_PATH
```

> 鉴于程序在运行时，DDS通信端口会动态变化，域端口ID变量在接受数据端执行程序时手动设定，比如`export DDS_DOMAIN_ID = 50` ；
>
> 接受数据端接收数据后需要通过XML文件对相应的数据进行解析，而XML文件位置在`idc_soc_perception_lfs`工程中确定的位置，故XML文件路径变量固定在环境变量中，比如在`./bashrc`文件中添加环境变量`DDS_DYNTYPE_XML_PATH`，添加方式如下：
>
> ```shell
> # ./bashrc中添加如下变量
> DDS_DYNTYPE_XML_PATH="/YOUR_ROOT_TO_TARGET_CODE/idc_soc_perception_lfs/config/data/xmls/sorted"
> export DDS_DYNTYPE_XML_PATH
> ```



> **IDDS工具常用的查看指令(-h 命令可以查看帮助)**
>
>  idds_node_list    查看当前DDS总线上正在运行的节点列表
> idds_node_info   查看当前DDS总线上正在运行的节点信息
> idds_topic_list    查看当前DDS总线上正在发布的主题列表
> idds_topic_echo  显示当前DDS总线上正在发布的实时数据
> idds_bag_record  录制当前DDS总线上正在发布的实时数据，并存入sqlite数据库(.db3)
> idds_bag_play    回放录制的.db3文件到DDS总线
> idds_bag_info    查看录制的.db3文件的信息概述 



#### 2、模块编译指令

| compile commands          | ./idc.sh sil_gcc -m clean -t sil -p cpj_dflzm -d idcmid -v fb0 |
| ------------------------- | ------------------------------------------------------------ |
| (Clean, Configure, Build) | ./idc.sh sil_gcc -m configure -t sil -p cpj_dflzm -d idcmid -v fb0 |
|                           | ./idc.sh sil_gcc -m build -t sil -p cpj_dflzm -d idcmid -v fb0 -j 16 |
|                           |                                                              |
| compile commands arm      | ./idc.sh arm -m clean -t apu -p cpj_dflzm -d idcmid -v fb0   |
| (Clean, Configure, Build) | ./idc.sh arm -m configure -t apu -p cpj_dflzm -d idcmid -v fb0 |
|                           | ./idc.sh arm -m build -t apu -p cpj_dflzm -d idcmid -v fb0 -ct vial_app |

> **其中vial_app就是调用SV3D模块动态库的主函数**



### 3、Debug GuangDa Camera with SV3D

#### 1、Config Env and Code

```shell
# 在tda4开发板上，/home/root/sv3d目录下添加如下文件和目录
bind # sv3d(ARM-end)编译后生成的文件目录	    
cc # 
pc # 
latest.log  
log1.txt    	    
run.sh
vx_app_multi_cam_1_img.txt
vx_app_multi_cam_2_pipe_img.txt
vx_app_multi_cam_3_data_ref_q_img.txt
vx_app_multi_cam_4_pipe0_img.txt

# 将tda4依赖库和所需的可执行文件(sv3d中的tda4)放置于/opt/tda4目录下

```

#### 2、执行脚本

```shell
sh run.sh # 运行SV3D模块
```

