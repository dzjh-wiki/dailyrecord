# python模块安装

----
## 1 选择镜像
### 1.1 国内镜像
  * 阿里云：https://mirrors.aliyun.com/pypi/simple
  * 豆瓣：https://pypi.doubanio.com/simple
  * 清华大学：https://pypi.tuna.tsinghua.edu.cn/simple
  * 清华大学2：https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple

### 1.2 使用命令
```
pip install 模块名 -i 镜像地址 --trusted-host 镜像主机名
```
  * 其中镜像主机名为镜像地址的域名，如阿里云的镜像主机名为：`mirrors.aliyun.com`。

## 2 安装模块
### 2.1 安装gRPC
```
// 安装 python gRPC
pip install grpcio

// 安装 ProtoBuf 相关的 python 依赖库
pip install protobuf

// 安装 python grpc 的 protobuf 编译工具
pip install grpcio-tools
```