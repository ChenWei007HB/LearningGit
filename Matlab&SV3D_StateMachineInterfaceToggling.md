### SV3D 状态机界面切换 --> Matlab

### 1、Matlab安装

#### 1、准备Matlab镜像文件.iso(Linux)与破解文件crack文件夹

> 以**Matlab99R2020b_Linux_64.iso**文件为例

#### 2、安装

```shell
# 挂在MATLAB镜像文件
mkdir matlab # MATLAB文件夹与matlab.iso文件位于同一个目录
sudo mount -t auto -o loop Matlab99R2020b_Linux_64.iso matlab
# 执行安装文件install
sudo matlab/install -javadir /usr/lib/jvm/java-8-openjdk-amd64/jre/
# 安装界面启动后，输入安装秘钥
09806-07443-53955-64350-21751-41297
# 输入license文件，license文件选用破解文件crack文件夹中的license文件
license.lic
# 选择安装路径，默认安装在/usr/local/MATLAB/R2020b下但会报错，故选择安装在/opt/local/路径下
/opt/local/MATLAB/R2020b/
# 选择安装需要的库/包，此处选择MATLAB和Simulink两个库即可
MATLAB & Simulink
# 安装完成后取消挂载并删除挂载的文件夹
sudo unmount matlab/
sudo rm -r matlab
# 拷贝crack文件夹中的libmwlmgrimpl.so文件至/opt/local/MATLAB/R2020b/bin/glnxa64/matlab_startup_plugins/lmgrimpl/下
sudo cp ./libmwlmgrimpl.so /opt/local/MATLAB/R2020b/bin/glnxa64/matlab_startup_plugins/lmgrimpl/
# 进入MATLAB安装的bin目录下，启动MATLAB
cd /opt/local/MATLAB/R2020b/bin
sudo ./matlab # 启动MATLAB

```

> **注意**：
>
> 安装在默认`/usr/local/MATLAB/R2020b`时，在执行`sudo ./matlab`时会报错，报错如下：
>
> ```shell
> ./matlab: 475: .: Can't open /usr/local/MATLAB/R2020b/bin/util/arch.sh
> ```
>
> 可能是执行时的权限问题；
>
> 安装在`/opt/local`下就可以正常启动运行MATLAB；