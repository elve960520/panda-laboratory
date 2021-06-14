记录docker安装tensorflow 2.2.0 c++版安装踩坑(GPU)
###鸣谢
感谢[此博客](https://blog.csdn.net/djstavaV/article/details/106290915)的填坑记录以及编译生成的C++库
下载地址：
[csdn](https://download.csdn.net/download/djstavaV/12447363)
[google云盘](https://drive.google.com/open?id=10mD1HdQ1xzGwHPN1QHxXscfh3MWBjpy7)
不管哪种下载方法想办法下载下来（由于csdn没有c币，我使用的翻墙下载google云盘）

#### 下载dockers镜像
> docker pull nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04

#### 运行docker镜像
> docker run -itd --name ubuntu-18.04 --gpus all nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04 /bin/bash 

#### 测试docker镜像
> docker run --gpus all nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04 nvidia-smi
如果输出一个表格说明运行正常

#### 进入docker镜像
> docker exec -it ubuntu-18.04 /bin/bash
> cd /tmp

### 安装软件
``` bash
apt-get update
apt-get install wget autoconf automake libtool
apt-get install libffi-dev unzip vim
```

#### 下载并安装protobuf
- 下载protobuf，一定要下载这个版本，其他版本我测试过有bug，按下面下载即可
``` bash
wget https://github.com/protocolbuffers/protobuf/archive/310ba5ee72661c081129eb878c1bbcec936b20f0.tar.gz
```
- 解压文件
``` bash
tar -xvf protobuf*
```
- 安装（以下步骤分步输入）
``` bash
cd protobuf
./autogen.sh
./configure
make
make install 
ldconfig
```

#### 下载并安装cmake
> wget https://github.com/Kitware/CMake/releases/download/v3.18.0-rc4/cmake-3.18.0-rc4-Linux-x86_64.sh
> chmod +x cmake-3.18.0-rc4-Linux-x86_64.sh
> ./cmake-3.18.0-rc4-Linux-x86_64.sh

#### 下载编译好的库
将文章开头提到的库下载下来
下载地址：
[csdn](https://download.csdn.net/download/djstavaV/12447363)
[google云盘](https://drive.google.com/open?id=10mD1HdQ1xzGwHPN1QHxXscfh3MWBjpy7)
然后可以使用rz命令传到服务器上（具体rz命令使用自行查阅）
不会rz也可以使用scp，不过很麻烦

#### 解压
下载后的文件应为
> tensorflow-2.2.0-gpu-cuda10.1-cudnn7.6.zip
- 解压
> unzip tensorflow-2.2.0-gpu-cuda10.1-cudnn7.6.zip
- 解压后会得到两个文件
> tensorflow-2.2.0-gpu-so.tar.gz
> tensorflow-2.2.0-include.tar.gz
- 这里压缩名字和文件对应不上，先修改名字
``` bash
mv tensorflow-2.2.0-gpu-so.tar.gz tensorflow-2.2.0-gpu-so.tar
mv tensorflow-2.2.0-include.tar.gz tensorflow-2.2.0-include.tar
```
- 然后解压两个文件
``` bash
tar -xvf tensorflow-2.2.0-gpu-so.tar
tar -xvf tensorflow-2.2.0-include.tar
```
- 得到两个文件夹
> tensorflow
> include

#### 编译
- 在root目录下新建tensorflow_cc目录用作工程目录
> mkdir /root/tensorflow_cc && cd mkdir /root/tensorflow_cc
- 将解压的两个文件夹拷贝到当前目录下(我的原来在/tmp下)
> cp -r /tmp/include ./
> cp -r /tmp/tensorflow ./
- 新建main.cpp，将下面代码拷贝进去
> vim main.cpp
``` cpp
#include <iostream>
#include "tensorflow/cc/client/client_session.h"
#include "tensorflow/cc/ops/standard_ops.h"
#include "tensorflow/core/framework/tensor.h"

using namespace tensorflow;
using namespace tensorflow::ops;

int main()
{
    Scope root = Scope::NewRootScope();

    // Matrix A = [3 2; -1 0]
    auto A = Const(root, { {3.f, 2.f}, {-1.f, 0.f} });
    // Vector b = [3 5]
    auto b = Const(root, { {3.f, 5.f} });
    // v = Ab^T
    auto v = MatMul(root.WithOpName("v"), A, b, MatMul::TransposeB(true));

    std::vector<Tensor> outputs;
    ClientSession session(root);

    // Run and fetch v
    TF_CHECK_OK(session.Run({v}, &outputs));
    std::cout << "tensorflow session run ok" << std::endl;
    // Expect outputs[0] == [19; -3]
    std::cout << outputs[0].matrix<float>();

    return 0;
}
```
新建CMakeLists.txt，将下面代码拷贝进去
> vim CMakeLists.txt
``` bash
project(libtf)
cmake_minimum_required(VERSION 3.0)

add_definitions(-std=c++11)

set(TENSORFLOW_ROOT_DIR /root/tensorflow_cc)

include_directories(
        ${TENSORFLOW_ROOT_DIR}/include
)

aux_source_directory(./ DIR_SRCS)

link_directories(/root/tensorflow_cc/tensorflow)

add_executable(libtf ${DIR_SRCS})

target_link_libraries(libtf /root/tensorflow_cc/tensorflow/libtensorflow_cc.so /root/tensorflow_cc/tensorflow/libtensorflow_framework.so.2)

```
- 新建BUILD目录用作编译目录
> mkdir BUILD
- 此时目录应该结构如下
> CMakeLists.txt  include  tensorflow main.cpp BUILD
- 然后进入BUILD进行编译
``` bash
cd BUILD
cmake ..
make
./libtf
```

#### 验证
如果操作无误的话会输出如下说明成功
``` bash
2020-07-15 07:50:06.189807: I tensorflow/core/platform/cpu_feature_guard.cc:143] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX512F
2020-07-15 07:50:06.197277: I tensorflow/core/platform/profile_utils/cpu_utils.cc:102] CPU Frequency: 2095170000 Hz
2020-07-15 07:50:06.198722: I tensorflow/compiler/xla/service/service.cc:168] XLA service 0x5601fa962630 initialized for platform Host (this does not guarantee that XLA will be used). Devices:
2020-07-15 07:50:06.198748: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (0): Host, Default Version
2020-07-15 07:50:06.201424: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcuda.so.1
2020-07-15 07:50:08.352472: I tensorflow/compiler/xla/service/service.cc:168] XLA service 0x5601fa8c1550 initialized for platform CUDA (this does not guarantee that XLA will be used). Devices:
2020-07-15 07:50:08.352502: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (0): Tesla T4, Compute Capability 7.5
2020-07-15 07:50:08.352510: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (1): Tesla T4, Compute Capability 7.5
2020-07-15 07:50:08.352517: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (2): Tesla T4, Compute Capability 7.5
2020-07-15 07:50:08.352524: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (3): Tesla T4, Compute Capability 7.5
2020-07-15 07:50:08.365629: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1561] Found device 0 with properties: 
pciBusID: 0000:3b:00.0 name: Tesla T4 computeCapability: 7.5
coreClock: 1.59GHz coreCount: 40 deviceMemorySize: 14.73GiB deviceMemoryBandwidth: 298.08GiB/s
2020-07-15 07:50:08.366668: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1561] Found device 1 with properties: 
pciBusID: 0000:86:00.0 name: Tesla T4 computeCapability: 7.5
coreClock: 1.59GHz coreCount: 40 deviceMemorySize: 14.73GiB deviceMemoryBandwidth: 298.08GiB/s
2020-07-15 07:50:08.367669: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1561] Found device 2 with properties: 
pciBusID: 0000:87:00.0 name: Tesla T4 computeCapability: 7.5
coreClock: 1.59GHz coreCount: 40 deviceMemorySize: 14.73GiB deviceMemoryBandwidth: 298.08GiB/s
2020-07-15 07:50:08.368490: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1561] Found device 3 with properties: 
pciBusID: 0000:af:00.0 name: Tesla T4 computeCapability: 7.5
coreClock: 1.59GHz coreCount: 40 deviceMemorySize: 14.73GiB deviceMemoryBandwidth: 298.08GiB/s
2020-07-15 07:50:08.368833: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcudart.so.10.1
2020-07-15 07:50:08.846465: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcublas.so.10
2020-07-15 07:50:09.035757: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcufft.so.10
2020-07-15 07:50:09.068711: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcurand.so.10
2020-07-15 07:50:09.355638: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcusolver.so.10
2020-07-15 07:50:09.420187: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcusparse.so.10
2020-07-15 07:50:09.425393: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcudnn.so.7
2020-07-15 07:50:09.432944: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1703] Adding visible gpu devices: 0, 1, 2, 3
2020-07-15 07:50:09.432978: I tensorflow/stream_executor/platform/default/dso_loader.cc:44] Successfully opened dynamic library libcudart.so.10.1
2020-07-15 07:50:09.437382: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1102] Device interconnect StreamExecutor with strength 1 edge matrix:
2020-07-15 07:50:09.437398: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1108]      0 1 2 3 
2020-07-15 07:50:09.437406: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1121] 0:   N Y Y Y 
2020-07-15 07:50:09.437412: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1121] 1:   Y N Y Y 
2020-07-15 07:50:09.437418: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1121] 2:   Y Y N Y 
2020-07-15 07:50:09.437425: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1121] 3:   Y Y Y N 
2020-07-15 07:50:09.442284: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1247] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:0 with 369 MB memory) -> physical GPU (device: 0, name: Tesla T4, pci bus id: 0000:3b:00.0, compute capability: 7.5)
2020-07-15 07:50:09.444371: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1247] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:1 with 14071 MB memory) -> physical GPU (device: 1, name: Tesla T4, pci bus id: 0000:86:00.0, compute capability: 7.5)
2020-07-15 07:50:09.446104: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1247] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:2 with 14071 MB memory) -> physical GPU (device: 2, name: Tesla T4, pci bus id: 0000:87:00.0, compute capability: 7.5)
2020-07-15 07:50:09.447763: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1247] Created TensorFlow device (/job:localhost/replica:0/task:0/device:GPU:3 with 67 MB memory) -> physical GPU (device: 3, name: Tesla T4, pci bus id: 0000:af:00.0, compute capability: 7.5)
tensorflow session run ok
19
```
- 由于我的机器上有个四个GPU所以输出多一些，正常一个GPU的话输出一个就正常，但是如果没有输出GPU支持的话就有问题需要解决
