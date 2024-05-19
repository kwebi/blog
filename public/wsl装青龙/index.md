# windows安装wsl和青龙面板


## **安装WSL**

在cmd中输入以下命令：

```
wsl --install
```

有疑问可看下面文章

[如何使用 WSL 在 Windows 上安装](Linuxlearn.microsoft.com/zh-cn/windows/wsl/install)

需要注意的点是系统版本不能太低。

如果中途显示访问失败可以考虑更换DNS和Host

## 安装docker

下载docker desktop，安装中根据提示选择wsl方式，并根据下面文章设置：

[WSL 上的 Docker 容器入门](learn.microsoft.com/zh-cn/windows/wsl/tutorials/wsl-containers)

完成上面步骤后，可打开cmd，输入wsl，再输入docker，有内容则成功。

## 安装青龙

确保进入wsl里面，输入：

```
wget -q https://yanyu.ltd/https://raw.githubusercontent.com/yanyuwangluo/VIP/main/Scripts/sh/ql.sh -O ql.sh && bash ql.sh
```

根据提示安装即可

进入后台后，安装依赖可看下面文章：

[青龙面板-安装依赖](https://help.yunjiutian.com/project-1/doc-75/)




