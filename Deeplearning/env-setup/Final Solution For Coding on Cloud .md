# Final Solution For Coding on Cloud 
# [远程编程的终极解决方案]

	SSH(kernel) + PyCharm(python-ide) + Transmit(SFPT-mac) + Mobaxterm(SFPT-win)
	
### 1. 配置远程服务器的SSH服务
首先在远程服务器上安装SSH程序（一般Ubuntu系统会自带）  

	sudo apt-get install ssh
	
打开远程服务器上上的 X11-forwarding，这里 是关于 X11 的介绍，它用来实现远程图形界面的传输，对画图等涉及图形界面的程序是必须的。当然，如果你只希望纯命令行的使用服务器，可以跳过本步

	sudo vi /etc/ssh/sshd_config #打开配置文件
	# i 进入vi的编辑模式
	X11Forwarding yes      #开启 X11
	# Esc退出编辑模式，并 :wq 保存并退出
	
### 2. 配置本地PC
安装xquartz

如果你使用MAC，

1. 首先安装 [homebrew](https://brew.sh/index_zh-cn)，和 [Cask](https://caskroom.github.io/)，然后使用命令 `brew cask install java pycharm xquartz`来安装
2. 或者，直接下载 xquartz 安装包进行安装


类似的，打开本地PC的X11-forwarding功能

	echo 'ForwardX11 yes' >> ~/.ssh/config
	echo 'Compression yes' >> ~/.ssh/config
	
现在，通过SSH连接到远程服务器

	ssh -Y username@ip

然后查看 *display environment variable*:
	
	echo $DISPLAY #记住这里返回的结果
	
然后在远程服务器上，打开 `.bashrc`, 加入该变量
	DISPLAY=localhost:17.0
	
### 3. 配置 PyCharm
首先添加 **远程解释器**：
![](https://cdn-images-1.medium.com/max/1600/1*2VRhJ2LNTPgj6oYkPc3Gdw.png)
![](https://cdn-images-1.medium.com/max/1600/1*EPPSpS-d5b5tmqy8gigvvA.png)
这里一定要选择好你 远程服务器上 的 Python 目录，如果你装了不止一个 Python 解释器的话。

然后是设置 **文件同步**：
PyCharm的工作流程是：本地修改-->上传到远程服务器-->在服务器里执行-->返回结果到本地。

这里有一个非常不好用的地方，就是即使你想运行一个远程文件，你也要先把它下载到本地，然后再执行上面一系列操作。

为了每次新建空文件夹的时候，PyCharm都能自动帮你上传到远程服务器，首先做一个设置：
![](https://cdn-images-1.medium.com/max/1600/1*K7QSRTvSdOuElRsHilqGCQ.png)

下面就开始正式新建 SFTP 服务了：

![](https://cdn-images-1.medium.com/max/1600/1*Q839zZt9Ep3mPJGTrlF4xw.png)

添加完之后，一定要在 Mapping 设置好 **本地机器和远程服务器的目录对应关系**

![](https://cdn-images-1.medium.com/max/1600/1*GWI-7WfkLpqQBvbRqz7Jeg.png)

然后，在 “Tools > Deployment > Automatic Upload” 打开自动上传

![](https://cdn-images-1.medium.com/max/1600/1*4_NIKQV7ufumNhoi70Uh7g.png)

最后，我们设置我们的Python Console 中的环境变量

![](https://cdn-images-1.medium.com/max/1600/1*9tEBk6KvR9uQ1L4W7kkIYQ.png)

为了能够正常的显示图片，你需要注意三件事情：

1. 在远程服务器上出现了正常的 DISPLAY 变量
2. 在远程服务器和本地机器上都有正常的 X-11 forwarding
3. 使用了合适的 matplotlib backend 画图
4. 当前至少有一个符合上述条件的，分离的，SSH连接

下面是一个跑通了的程序
```Python
from PyQt5 import QtCore, QtGui, QtWidgets
#import cv2
import tensorflow
import matplotlib

matplotlib.use('Qt5Agg')
import matplotlib.pyplot as plt
#plt.switch_backend('agg')
import numpy as np

print("Tensorflow Imported")
plt.plot(np.arange(100))
plt.show()
plt.savefig('test.png')
plt.close()
```





























