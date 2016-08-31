---
layout:     post
title:      "Python导入自定义的包"
subtitle:   "Loneliness Makes Man Stronger!"
date:       2016-08-31
author:     "WangJialin"
tags:
    - python

---

- [1.一个很重要的原则](#start)
- [2.如何使目录成为一个package](#import_module_1)
- [3.同一Package内部的module如何相互导入使用](#import_module_2)

<a name = 'start'></a>

这个网上的详细的教程已经非常之多了。在这里我力求言简意赅。

首先，有一个很重要的原则要把握好，

```
那就是import的时候，python解释器首先会在一个sys.path中寻找这些包的路径，

然后就是当前目录下寻找，如果寻找不到的话就会报不存在的错误！

```

一般情况下，以下面的目录结构为例，假设我们要在main.py中使用utils包中的module的话。

```
- ParentPack(folder)
	-  __init__py
 	-  Sub1Pack(folder)
 		- __init__.py
 		- module11.py
 		- module12.py
 	-  Sub2Pack(folder)
    	- __init__.py
 		- module21.py
 		- module22.py
	- main.py
```
(ps:init前后分别为两条短划线)

### 如何使目录成为一个package

在python里，通过`__init__.py`文件来识别此目录是一个package的。因此，在utils目录下面，一定要新建一个`__init__.py`文件。

<a name = 'import_module_1'></a>

### 同一Package内部的module如何相互导入使用

另外，就是在包内部的module中，如何引用包内的其他module，这个真的是折磨了我好久！！切记，千万不要以为是在一个目录里，所以直接`import module`就好了！！！要加上包的名字！！！
假如要在module11.py中使用module12.py的函数的话，
一定要这样import：

```
import utils.module12
```

这样在main.py中，你可以这样使用：

```
import utils.module12
```

或者

```
from utils import module1
```

<a name = 'import_module_2'></a>

### 不同子Package的module中如何导入使用

比如想在module21中导入module11。那么同样，OK， 你可以像同一个包一样导入！！但是，记住，这时假如你运行module21时，会报错的，因为module11不属于当前目录下的包的模块。可是，假如，你在main.py中导入module21,（此时module21中已经导入module11），你运行main.py则不会出错，因为此时module21是main.py当前目录下Sub2Pack中的模块。

因此，把握好我刚开始提到的那个原则就不会出现什么问题了。