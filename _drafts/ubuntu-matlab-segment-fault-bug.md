
## Ubuntu 16.04下安装matlab R2015b出现段错误(segment fault)

最近慢慢地把各种软件都转到ubuntu下，今天装了下matlab破解版。破解版的教程网上到处都有，都差不多。

按照步骤安装好后进入bin目录下，运行`sudo ./matlab`，结果就出现了如题的错误。

这种错误简直懵逼了。然后，百度了下，找到了解决办法。

官网链接：
http://www.mathworks.com/support/bugreports/1297894

下面是我从网页上copy下来的，大意是ubuntu系统的库太新，和matlab不兼容，因此只需要将一个库文件的名字重命名为旧版本的就OK了。

Summary

MATLAB crashes during startup on Ubuntu 15.04 and newer, as well as distributions derived from those versions

Description

When using Ubuntu Linux distributions 15.04 and newer, as well as distributions derived from those versions, MATLAB can crash during startup.

This crash occurs because these releases include a newer version of libstdc++.so.6 than the version shipped with MATLAB (version 6.0.17). When MATLAB loads version 6.0.17 first, the OS reaches an incompatibility that causes MATLAB to crash.

Workaround

You can force MATLAB to load the newer version of the library provided by the operating system, by following these instructions:

```
    1. Identify the location where MATLAB is installed
    2. Navigate to the sys/os/glnxa64 directory within this installation folder
    3. Rename libstdc++.so.6 library to libstdc++.so.6.old
```
We have done limited testing with version 20 of libstdc++.so.6. If you experience problems with MATLAB when using this version, please contact MathWorks technical support.