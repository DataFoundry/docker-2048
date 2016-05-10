```  
$  oc new-app https://github.com/alexwhen/docker-2048.git
```  
其中：  
* `oc` 是ldp平台的命令行控制工具，他提供了对ldp平台的所有控制功能  
* `new-app`是ldp平台的操作命令，它可以通过后续指定的若干参数完成一个应用的构建和发布  
* `https://github.com/alexwhen/docker-2048.git` 是一个git代码仓库，在github上搜索docker-2048即可得到，它是我们要在ldp平台发布的第一个应用  

输入命令并点击回车后，命令行会等待一段时间，等待时间长短与代码仓库的代码量，以及命令执行位置与github的网络条件有关，这段等待时间中oc会先clone代码仓库到本地，对代码仓库中的dockerfile进行解析，主要是获取基础镜像信息，命令执行成功后会显示如下信息  

```  
--> Found Docker image 9d71014 (8 weeks old) from docker.io for "library/alpine"

    * An image stream will be created as "alpine:latest" that will track the source image
    * A Docker build using source code from https://github.com/alexwhen/docker-2048.git will be created
      * The resulting image will be pushed to image stream "docker-2048:latest"
      * Every time "alpine:latest" changes a new build will be triggered
    * This image will be deployed in deployment config "docker-2048"
    * Port 80 will be load balanced by service "docker-2048"
      * Other containers can access this service through the hostname "docker-2048"
    * WARNING: Image "docker-2048" runs as the 'root' user which may not be permitted by your cluster administrator

--> Creating resources with label app=docker-2048 ...
    imagestream "alpine" created
    imagestream "docker-2048" created
    buildconfig "docker-2048" created
    deploymentconfig "docker-2048" created
    service "docker-2048" created
--> Success
    Build scheduled for "docker-2048", use 'oc logs' to track its progress.
    Run 'oc status' to view your app. 
```  
通过输出信息我们可以看到ldp平台构建和发布应用的几个基本要素  
* `buildconfig`，可以简写为bc,用来存储镜像构建所需的配置信息，包括最基本的代码仓库地址，构建分支、tag、commit-id信息，dockerfile位置，镜像构建输出位置及名称，在相对高级的应用场景下还包含ci出发器，github webhock、私有git仓库登录信息等  
* `deployconfig`，简写为dc，用来存储镜像部署所需的配置信息，  
* `service`，简写为svc,是平台提供应用高可用和服务发现功能的入口  
* `imagestream`，简写为is,是平台显示私有仓库镜像信息的入口，通过他也是平台CD功能的触发入口  
