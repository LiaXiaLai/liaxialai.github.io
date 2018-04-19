---
title: Python 虚拟环境
date: 2018-03-09 11:05:20
update: 2018-04-02 17:50:21
tags:
  - Python
published: true
---

![](https://ws2.sinaimg.cn/large/006tNc79gy1fp6b83bctej30gp05njra.jpg)

要问当前最热的语言是什么，那么必定会是 Python。随着近几年的人工智能的发展，Python 在数据处理方面的能力得到青睐，随着云计算和大数据的概念的提起，Python 也经常用来作为数据采集的利器，在初创的互联网公司，Python 也常常用来作为 Web 服务器的后端。

总之，Python 用途太广了，但是又是非常简单方便的。总之「人生苦短，我用 Python。」下图为 TIBO 编程语言排行榜。

![](https://ws3.sinaimg.cn/large/006tNc79gy1fp6bd52luej314s0e2wfd.jpg)

可以看到，Python 的排行仅次于三大老牌语言Java、C、C++ 之后，并且还正处于上升的趋势，未来的发展不可限量。

在说到 Python 项目开发之前，我们需要对 Python 的虚拟环境有个充分的认识，并且，我将简单的介绍比较常用的虚拟环境工具。

## 一、为什么需要虚拟环境？

假设有这样一个场景，我在同时进行项目 A 和 B 的开发，这两个项目都需要同一个包「requests」。如果这两个项目使用了相同的版本，那或许没有什么问题。当假如 A 需要 1.0 版本，而 B 需要 2.0 版本的时候，问题出现了，Python 并不能按照模块的版本号来进行区分。

在这个时候，我们就需要使用一个虚拟环境，来对每个项目创建独立的开发环境。这样，在上诉问题中，解决的办法就很简单了：创建两个虚拟环境，分别为 A 和 B 的，然后在虚拟环境下，引入自己所需要的依赖包。各自安好。

虚拟环境的优点已经显而易见了，但是，可不仅仅只有这些。

1. 保持项目环境的独立性
2. 打包项目变得简单、快捷
3. 更好的进行包管理

在 Python 虚拟环境中，我推荐使用两个 virtualenv 和 pipenv。还有一个 conda，但是它经常用来作为科学计算的虚拟环境管理，在这里我就不阐述了。

## 三、virtualenv 虚拟环境

**1.安装 virtualenv**

安装 virtualenv 比较简单，一条命令：

```bash
[sudo] pip3 install virtualenv
```

安装完成之后，命令端的输出应该像是这样的。

![](https://ws4.sinaimg.cn/large/006tNc79gy1fp6c4obmqcj30vg07gaar.jpg)

**2.创建虚拟环境**

安装成功之后，便可以创建虚拟环境了。使用如下的命令：

```bash
virtualenv ENV
```

虚拟环境创建成功的输出信息应该是这样的。

![](https://ws2.sinaimg.cn/large/006tNc79gy1fp6c8qu6e6j30vk06i3z5.jpg)

**ENV** 代表的就是想要创建虚拟环境目录的名称。比如，在当前目录下，创建了一个 ENV 目录。

![](https://ws2.sinaimg.cn/large/006tNc79gy1fp6ccaiofpj30zo08qmxm.jpg)

该目录下，又有几个目录和文件。

+ **ENV/bin**: 虚拟环境中 Python 的执行路径。
+ **ENV/lib**: 虚拟环境中的 Python 依赖包
+ **ENV/include**: 虚拟环境中 Python 的库函数

**3.激活虚拟环境**

首先，我们输入下面的命令激活虚拟环境：

```bash
# 切换到 ENV 目录
cd ENV

# 激活环境 Posix systems
source bin/active
```

好了，这样就激活成功了，看看命令行发现了什么变化？

![](https://ws1.sinaimg.cn/large/006tNc79gy1fp6h3n2tjqj30mc010t8l.jpg)

在用户名之前出现了 **(ENV)** 标志，没错，这就代表进入了虚拟环境之中了，这时候，我们只需要向正常使用 pip 一样，安装所需要的依赖包了。

例如，安装一个 flask 的包，在虚拟环境中输入：

```bash
# python 2 使用 Pip
pip3 install flask
```

验证是否安装成功。

![](https://ws2.sinaimg.cn/large/006tNc79gy1fp6h8odrdgj30ss05qdg9.jpg)

没有出现报错信息，这是可行的。

**4.退出虚拟环境**

那我们要验证是否对真实的 Python 环境造成了包混乱呢？首先，需要输入下面的命令来退出虚拟环境：

```bash
deactivate
```

这样就退出虚拟环境了，是不是很方便呢。现在，我们来验证下真实的 Python 环境是否含有 flask 包，在命令行输入

![](https://ws2.sinaimg.cn/large/006tNc79gy1fp6hejdhcdj30su07wdhc.jpg)

出现了报错消息，**ModuleNotFoundError: No module named 'flask'** 代表的就是没有找到名为 flask 的包。成功。

**5.重新进入虚拟环境验证**

现在，重新进入虚拟环境验证是否能够再次成功导入 flask 包呢？我在这儿就不演示了。

**6.删除虚拟环境**

删除虚拟环境是非常简单的，只需要退出虚拟环境之后，再将虚拟环境生成的目录删除掉即可。

**7.virtualenv 常见的参数**

+ <strong>system-site-packages</strong>: 令虚拟环境能够访问真实的系统全局的 site-packages 包
+ <strong>--no-site-packages</strong>: 令虚拟环境不能够访问真实的系统全局的 site-packages 包
+ <strong>--python</strong>: 指定虚拟环境的 Python 版本，例如 --python=python3.6

## 四、pipenv 虚拟环境

pipenv 是另一款 Python 虚拟环境的工具，由 kennethreitz 开发，没错，他也开发了 requests。pipenv 会自动创建虚拟环境并且会将安装的包信息写入 **Pipfile** 文件 和 **pipfile.lock** 文件（建议不要手动修改）。

**1.安装 pipenv**

输入下面的命令安装 pipenv：

```bash
# macOS 环境下
brew install pipenv

# 其他环境下建议使用命令
pip install pipenv
```

下载完成后，输入 **pipenv --version** 查看是否安装成功。

![](https://ws4.sinaimg.cn/large/006tNc79gy1fp6i3uiespj30ow022jrd.jpg)

**2.创建虚拟环境**

这里看到，我安装的是 9.0.3 版本的。现在如何创建虚拟环境呢？别着急，输入下面的命令：

```bash
# 创建虚拟环境目录
mkdir ENV

cd ENV

# 安装包
pipenv install flask
```

pipenv 会自动的创建虚拟环境，并且下载所需要的依赖，这样一个 pipenv 就创建完成了。

![](https://ws1.sinaimg.cn/large/006tNc79gy1fp6i85x9wbj30gk04c3ym.jpg)

看到这样的提示，就表示一切准备就绪了。

![](https://ws4.sinaimg.cn/large/006tNc79gy1fp6ie31hlmj30gs09kt8t.jpg)

我们可以看到 Pipfile 的格式。

+ **packages**: 代表了在虚拟机环境中安装的包

**3.验证虚拟环境**

首先，创建一个 Python 文件。

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello Flask"
    
if __name__ == '__main__':
    app.run(debug=True)
```

然后输入下面的命令来运行。

```bash
pipenv run python3 flask_demo.py
```

会看到如下的输出，不用明白什么意思。

![](https://ws4.sinaimg.cn/large/006tNc79gy1fp6ih1y26nj30vk06iglx.jpg)

然后在浏览器输入地址 **127.0.0.1:5000**，会看到网页的内容。

![](https://ws2.sinaimg.cn/large/006tNc79gy1fp6ii8lwfhj305m022a9u.jpg)

代表我们运行成功了。

**4. pipenv 常见参数**

+ <strong>--three,--two</strong>: 选择 Python 3 还是 Python 2
+ <strong>--site-packages</strong>: 可以访问系统全局包
+ <strong>--rm</strong>: 删除虚拟环境
    
## 五、virtualenv 与 pipenv 的区别

virtualenv 和 pipenv 都作为 Python 下优秀的包管理软件，那他们有什么不同呢？

首先 pipenv 作为后起之秀，比 virtualenv 要好一点的。为什么 pipenv 比 virtualenv 要更加优秀呢，我就直接翻译官方文档上的介绍。

+ 不再使 pip 和 virtualenv 分离。使用 pipenv 之后，它们作为一个整体。
+ pipenv 使用 Pipfile 自动管理 requirements.txt 文件
+ pipenv 更加的安全
+ pipenv 可以更加直观的查看依赖包信息（pipenv graph）

## 六、可能的错误

### 6.1 pipenv ValueError: unknown locale: UTF-8

在 macOS 上使用 `pipenv` 安装第三方包的时候，可能会出现报错。

```text
Creating a Pipfile for this project...
Creating a virtualenv for this project...
Traceback (most recent call last):
  File "/usr/local/bin/pew", line 7, in <module>
    from pew.pew import pew
  File "/usr/local/lib/python2.7/site-packages/pew/__init__.py", line 11, in <module>
    from . import pew
  File "/usr/local/lib/python2.7/site-packages/pew/pew.py", line 36, in <module>
    from pew._utils import (check_call, invoke, expandpath, own, env_bin_dir,
  File "/usr/local/lib/python2.7/site-packages/pew/_utils.py", line 22, in <module>
    encoding = locale.getlocale()[1] or 'ascii'
  File "/usr/local/Cellar/python/2.7.13/Frameworks/Python.framework/Versions/2.7/lib/python2.7/locale.py", line 564, in getlocale
    return _parse_localename(localename)
  File "/usr/local/Cellar/python/2.7.13/Frameworks/Python.framework/Versions/2.7/lib/python2.7/locale.py", line 477, in _parse_localename
    raise ValueError, 'unknown locale: %s' % localename
ValueError: unknown locale: UTF-8
```

这是因为 macOS 默认的编码不是 `UTF-8` 导致的，所以，需要在终端上进行修改。

```bash
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
```

执行上面的命令之后，即可正常工作啦。

# 参考链接

+ [Virtualenv](https://virtualenv.pypa.io/en/stable/)
+ [PipEnv](https://pipenv.readthedocs.io/en/latest/), By kennethreitz


