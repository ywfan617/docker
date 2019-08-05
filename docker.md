# Docker learning  

## Docker是什么  

+ <h3>Docker理念</h3>  
    软件带环境安装 ：安装的时候，把原始环境一模一样的复制过来，。开发人员利用Docker可以消除写作编码时“在我的环境上可以运行”  

    1. docker是基于Go语言实现的云开源项目。  

    2. Docker的主要目标是“Build ,ship ,and run app anywhere”，也就是说通过对应用组件的封装，分发，部署，运行等生命周期的管理，使用户的app（可以是一个wen应用或数据库应用等等）及其运行环境能够做到<u>“一次封装，到处运行”</u>。  

    3. linux容器技术的出现解决了这样的一个问题，而docker就是在它的基础上发展过来的，将应用运行在Docker容器上面，而Docker容器在任何操作系统上都是一致的，这就实现了跨平台，跨服务器。<u>只需要一次配置环境，换到别的机子上就可以一键部署好，大大简化了操作</u>。  

+ <h3>Docker定义</h3>
    Docker解决了运行环境和配置文件软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术。  
    <u>Docker本身是一个容器运行载体或称之为管理引擎。我们把应用程序和配置依赖打包好形成一个可交付的运行环境，这个打包好的运行环境就是image文件。只有通过这个镜像文件才能生成Docker容器。image文件可以看作是容器的模板。Docker根据image文件生成容器的实例。同一个image文件可以生成多个运行的容器实例</u>。

+ <h3>Docker的三要素</h3>  
  
  + 镜像：就是一个只读的模板。<u>镜像（类）可以用来创建Docker容器，一个镜像可以创建很多容器（实例）</u>。  

  + 容器：Docker利用容器独立运行一个或一组应用。<u>容器是用镜像创建的运行实例</u>。容器可以被启动，开始，停止，删除。每个容器都是相互隔离的，保证安全的平台。  

  + 仓库：是集中存放镜像文件的场所。仓库和仓库注册服务器是有区别的，仓库注册服务器上存放着多个仓库，每个仓库又包含了多个镜像，每个镜像有不同的标签。仓库分为公开仓库和私有仓库，最大的公开仓库是Docker hub（中心，轮轴）,国内的公开仓库包括阿里云，网易云。  

## Docker能干嘛  

+ <h3>容器虚拟化技术</h3>  
    <u>linux容器不是模拟一个完成的操作系统</u>，而是对进程进行隔离，有了容器，就可以经软件运行所需的所有资源打包到一个隔离的容器中，容器与虚拟机不同，不需要捆绑一套操作系统，只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行。  
+ Docker的架构图   
    ![Docker架构图](file:///D:/2019/markdown文档/docker文档/docker_structuregraph.png)  

## Docker去哪里下  

+ 官网  
    docker官网：<http://www.docker.com>  
    docker中文网站：<https://www.docker-cn.com>  
+ 仓库  
    docker hub官网：<https://hub.docker.com>(国外网站)  

## docker安装  

+ 检查Docker的安装环境
    1. 内核版本 uname -r  

    2. 操作系统版本  cat /ect/redhat-release  
    3. www.docs.docker.com官方安装教程  

+ 阿里云镜像加速  
    1. 是什么 <https://dev.aliyun.com/search.html>
    2. 注册一个属于自己的阿里云账户，获取加速器地址链接，登录名：TVFANa  
        https://7mni7ua9.mirror.aliyuncs.com  

    3. 配置本机Docker运行镜像加速器  

    4. 重新启动Docker后台服务：service docker restart

    5. linux系统下配置完加速器需要检查是否生效  

        ```bash
        sudo mkdir -p /etc/docker
        sudo tee /etc/docker/daemon.json <<-'EOF'
        {
        "registry-mirrors": ["https://7mni7ua9.mirror.aliyuncs.com"]
        }
        EOF
        sudo systemctl daemon-reload
        sudo systemctl restart docker
        ```  

+ 测试用例：docker run hello-world  
    测试过程![Docker架构图](file:///D:/2019/markdown文档/docker文档/run_theory.png)  

+ 底层原理  
    1. Docker是怎么工作的  
    Docker是一个Client-Server结构的系统，Docker守护进程运行在主机上，然后通过Socket链接从客户端访问，守护进程从客户端接收命令并管理运行在主机上的容器，<u>容器，是一个运行时环境，就是我们前面说到的集装箱</u>。  
    2. Docker与其他VM相比  

        |硬件|Docker容器|虚拟机|  
        |--- |   ---   |---|
        |操作系统|与宿主机共享OS|宿主机OS上运行虚拟机OS|  
        |存储大小|镜像小，便于存储与传输|镜像庞大|  
        |运行性能|几乎无额外性能损失|操作系统额外的CPU，内存消耗|  

    3. 与虚拟机的区别  

        + Docker不需要Hypervision实现硬件资源虚拟化，运行在docker容器中的程序直接使用的都是实际物理机的硬件资源。因此zaicpu，内存利用率上有明显优势。  

        + docker利用的宿主机的内核，不需要guest内核

## docker常用命令

1. 帮助命令  
    docker --version  
    docker --info  
    docker --help  
2. 镜像命令  
    docker images [a][q][digest]:列出本地主机的镜像  
    docker search image ：到仓库寻找镜像
    docker pull image：拉取镜像
    docker rmi image1：tag1 [image2:tag2]....：删除1个或多个镜像  
    docker rmi $(docker images -qa)  
3. 容器命令  
    + docker run [options] image [command][args]  不加任何参数显示为正在运行的容器的信息
    + docker ps [-l][-a][-n3][q]:列出正在运行的docker容器[上一次运行的][s所有的][最近的三个] [只显示id]
    + exit:退出容器  
    + ctrl+P+Q：容器不停止退出  
    常用命令  

    ```bash  
        docker run -it image #交互式运行
        docker run -d image #后台运行
    ```  

    + docker start containerid/name  
    + docker restart containerid/name  
    + docker stop containerid/name  
    + docker kill containerid/name :强制杀死  
    + docker rm containerid/name  
    + docker rm $(docker images -qa)  
    + docekr log -t[加入时间戳] -f[跟随最新的日志打印] -tail[显示最后几条] dockerid/name  #查看docker日志  
    + docker top containerid/name查看容器内运行的进程  
    + docker inspect containerid/name  ：查看容器的详细信息，json串
    + docker attach containerid/name：直接进入容器启动命令的终端，不会启动新的进程
    + docker exec -it containerid/name  ：是在容器中打开新的终端，并且可以启动新的进程
    + docker cp containerid/name:/tmp/yum.log /root：容器中的内容拷贝到主机  

## docker镜像  

1. 是什么  
    + 镜像时一种轻量级，可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软禁啊所需的所有内容，包括代码，运行时，库，环境变量和配置文件。  

    + UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。
    特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录  

    + 镜像分层  

2. docker commit  

3. docker容器数据卷：希望把容器的数据保存下来，数据持久化 主机->容器，容器->主机  ；数据卷容器->容器数据卷的继承 --volumn-from  

## dockfile  

1. 手动编写一个dockerfile文件，符合file的规范  
2. docker build:获得一个自定义的镜像  
3. docker run
