# Doom Emacs初步使用


几年前稍微玩过`Emacs`,但没有深入，虽然现在也不算精通，不过起码还能融入日常使用，也算入门吧。

<!--more-->
国内很多教程都是教怎么配置，怎么写`Emacs Lisp`，殊不知，这样反而会打击新人的积极性，毕竟很麻烦，很多人没有这个耐心一个个配。不如先拿一个现成的使用，后面的个性化配置等用熟后再学也不迟。

## 首先安装

国内已有很多教程，这里给出他们的链接，建议安装版本为27

- [21 天学会 Emacs（2022 Edition）](https://book.emacs-china.org/#org7522f55)
-  [Doom Emacs 的教程](https://github.com/hlissner/doom-emacs/blob/master/docs/getting_started.org)
- [Windows用户通过WSL2 安装Emacs](https://hkvim.com/post/windows-setup/)

## 代理

`Doom Emacs`安装方式一般是通过克隆github上的仓库来进行的，直接克隆将会非常慢而且容易失败，这里提供一些解决办法


以Windows中的WSL2为例，首先Windows系统上要挂上代理(SSR，V2Ray等等)，建议把Windows上的git也开启代理，在powershell上输入如下命令：

```bash
# HTTP/HTTPS 协议，port 需与代理软件设置的一致

git config –-global http.proxy http://127.0.0.1:port

# SOCKS5 协议

git config --global http.proxy socks5://127.0.0.1:port
```

对代理软件要允许它访问局域网。然后在WSL的终端的~目录下中

```bash
vim ~/.proxyrc

# 输入如下内容后，键入Esc 和 : x保存
# 7890换成自己的代理软件端口

#!/bin/bash
host_ip=$(cat /etc/resolv.conf |grep "nameserver" |cut -f 2 -d " ")
export ALL_PROXY="http://$host_ip:7890"

# 退出后在终端输入
source .proxyrc
```

当然也可以选择安装proxychains

```bash
sudo apt-get install proxychains

sudo nano /etc/proxychains.conf

# 改为如下，ip和端口改为windows上的ip和代理的端口
socks5  127.0.0.1 8888

# 要运行时，在前面假释proxychains即可，如：
proxychains curl -i google.com

```

然后就可以快速克隆 Doom Emacs的仓库了

```bash
git clone --depth 1 https://github.com/hlissner/doom-emacs ~/.emacs.d

~/.emacs.d/bin/doom install

```
## 快捷键

执行完毕后即可键入`emacs`打开了，下面介绍一些基本操作，如果你会简单的Vim操作那么更简单，因为Doom Emacs天然支持Vim操作，按Ctr+Z可以进行切换

```
可供参考链接：
https://noelwelsh.com/posts/doom-emacs/
```

按键|意义|
---|:--:|
Spc+b|缓冲区操作|
Spc+w|窗口操作|
Spc+f|文件操作|
Spc+:|等同于M+x,可输入指令|
Spc+TAB|workspace操作|
Spc+h|帮助文档|
Spc+s|搜索文件、缓冲区等|
M+x eshell|打开终端|
Spc+q|退出|

这些只要你键入前两个，后面的选项会提示,相当方便。
vim相关操作不需要太多，懂等i,esc,v等相关操作和对应的模式即可。
以上，一个高配版vim就完成了

## 开启编程语言插件

进入`~/.doom.d/init.el` 把`lsp` 前面的注释去掉，并把下方对应的语言开启
以上即可进行快乐的玩耍使用了。






