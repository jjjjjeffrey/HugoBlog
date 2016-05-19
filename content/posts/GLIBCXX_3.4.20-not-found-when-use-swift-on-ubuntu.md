+++
date = "2016-05-18T16:41:25+08:00"
draft = false
title = "解决在Ubuntu上配置swift出现GLIBCXX_3.4.20 not found问题"

+++

在Linux上配置Swift环境在这里就不多说了，参见[Installing Swift](https://swift.org/getting-started/#installing-swift)，配置好以后发现如下问题：

```
$ swift --version
swift: /usr/lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.20' not found (required by swift)
swift: /usr/lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.21' not found (required by swift)
```

我们运行以下命令发现，找不到GLIBCXX_3.4.20和GLIBCXX_3.4.21，产生这个问题的原因是GCC版本低，需要更新：

```
$ strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX
GLIBCXX_3.4
GLIBCXX_3.4.1
GLIBCXX_3.4.2
GLIBCXX_3.4.3
GLIBCXX_3.4.4
GLIBCXX_3.4.5
GLIBCXX_3.4.6
GLIBCXX_3.4.7
GLIBCXX_3.4.8
GLIBCXX_3.4.9
GLIBCXX_3.4.10
GLIBCXX_3.4.11
GLIBCXX_3.4.12
GLIBCXX_3.4.13
GLIBCXX_3.4.14
GLIBCXX_3.4.15
GLIBCXX_3.4.16
GLIBCXX_3.4.17
GLIBCXX_3.4.18
GLIBCXX_3.4.19
GLIBCXX_DEBUG_MESSAGE_LENGTH
```

接下来我们来解决这个问题：

```
$ wget http://ftp.de.debian.org/debian/pool/main/g/gcc-5/libstdc++6-5-dbg_5.3.1-19_amd64.deb
$ ar -x libstdc++6-5-dbg_5.3.1-19_amd64.deb
$ tar xvf data.tar.xz
$ sudo cp usr/lib/x86_64-linux-gnu/debug/libstdc++.so.6.0.21 /usr/lib/x86_64-linux-gnu/
$ cd /usr/lib/x86_64-linux-gnu/
$ sudo rm libstdc++.so.6
$ sudo ln libstdc++.so.6.0.21 libstdc++.so.6
```
我们再次来运行这个命令来看一下：

```
$ strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX
GLIBCXX_3.4
GLIBCXX_3.4.1
GLIBCXX_3.4.2
GLIBCXX_3.4.3
GLIBCXX_3.4.4
GLIBCXX_3.4.5
GLIBCXX_3.4.6
GLIBCXX_3.4.7
GLIBCXX_3.4.8
GLIBCXX_3.4.9
GLIBCXX_3.4.10
GLIBCXX_3.4.11
GLIBCXX_3.4.12
GLIBCXX_3.4.13
GLIBCXX_3.4.14
GLIBCXX_3.4.15
GLIBCXX_3.4.16
GLIBCXX_3.4.17
GLIBCXX_3.4.18
GLIBCXX_3.4.19
GLIBCXX_3.4.20
GLIBCXX_3.4.21
```
这时GLIBCXX_3.4.20和GLIBCXX_3.4.21就出现了，ok，完美的解决了这个问题。
