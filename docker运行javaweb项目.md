# 命令说明
  ## docker run <相关参数> <镜像 ID> <初始命令>
-i：表示以“交互模式”运行容器

-t：表示容器启动后会进入其命令行

-v：表示需要将本地哪个目录挂载到容器中 （这个目录和本地是交互的，目录下的文件在容器和本地上是可以相互看见的）
  格式：-v <宿主机目录>:<容器目录>

  sudo docker run -i -t -v /data/software/:/data/software/ ae983d5e88ce /bin/bash

 ## 向容器内拷贝
  docker cp  文件名称  容器id : 容器下的路径

# 思路
  创建2个容器 一个是数据库容器，一个是java+tomcat容器(可以自己制作容器，commit成镜像，也可以直接pull tomcat，自带java)
  ## 创建mysql容器
    docker run --name <mysql_container> -p 10001:3306 -e MYSQL_ROOT_PASSWORD=123 -d <mysql_image>
    此时mysql已经可以访问了，如果要实现数据备份到宿主机，要使用volume,增加参数，将docker容器内的mysql的datadir挂载到宿主机的目录下
     -v /home/lib/mysql/:/var/lib/mysql/
     
  ## 创建tomcat容器
    docker run --name tomcat8 -p 31943:8080 -d tomcat
      此时使用 ip:31943可以访问tomcat了
     tomcat的日志文件、上传的图片一般也需要挂载宿主机目录
     *** 向docker容器内的tomcat下部署web项目 ***
     docker cp /opt/web/GeologyService/ <containerID>:/usr/local/tomcat/webapps/
     不用重启docker即可生效
