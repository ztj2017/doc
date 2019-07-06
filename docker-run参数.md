# docker run <相关参数> <镜像 ID> <初始命令>

-i：表示以“交互模式”运行容器

-t：表示容器启动后会进入其命令行

-v：表示需要将本地哪个目录挂载到容器中 （这个目录和本地是交互的，目录下的文件在容器和本地上是可以相互看见的）
  格式：-v <宿主机目录>:<容器目录>

#我的相关程序都在当前机器的/data/software/目录下，并且想把它挂载到容器的相同目录下：

> sudo docker run -i -t -v /data/software/:/data/software/ ae983d5e88ce /bin/bash
