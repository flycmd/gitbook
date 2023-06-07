# GitBook-Server-Docker

## 简介

部署官方文档（使用gitbook）的基础容器。


## 编译镜像
- 通过git 或者 下载方式拿到此项目，切换到此项目根目录
- 镜像变异 `docker build . -t gitbook-server:latest` , 其中 `gitbook-server` 可以自定义, 编译完成镜像可放到自有镜像仓库（只需要在部署机器上有此镜像即可，无更新的情况下不需要重复编译）


## 初始化编译 gitbook项目( 官方文档项目跳过此步骤 )
```
docker run --rm -v "$PWD/gitbook:/gitbook" gitbook-server gitbook init
```

> 该命令会自动创建用于 `示例` 的 `GitBook` 文件 。
实际效果就是在工作目录 `./gitbook` 下创建两个符合 `GitBook` 语法的文件 `README.md` 和 SUMMARY.md 。


## 构建 GitBook 项目

在 Docker 镜像中执行命令 `gitbook build`

```
docker run --rm -v "$PWD/gitbook:/gitbook" gitbook-server gitbook build
```

> 该命令会根据 `GitBook` 文件 `README.md` 和 `SUMMARY.md` 构建 html 项目 。
　实际效果就是在工作目录 ./gitbook 下构建目录名为 _book 的静态网页文件 。
　本地可以通过 ./gitbook/_book/index.html 测试访问 。


## 启动 GitBook 服务 ( 用于调试的时候 )
在 Docker 镜像中执行命令 gitbook serve：

```
docker run --rm -v "$PWD/gitbook:/gitbook" -p 4000:4000 gitbook-server gitbook serve
```

> 该命令效果就是构建一个可以访问 `./gitbook/_book/index.html` 的 Web 服务。
 访问地址 `localhost:4000`


## 后台运行方式启动 GitBook 服务 ( 生产使用 )
```
docker run -d --rm -v "$PWD/gitbook:/gitbook" -p 4000:4000 gitbook-server gitbook serve
```


## 停止服务
```
docker ps -- 查看CONTAINER ID

docker stop <CONTAINER ID> -- 停止服务
```


## Github Page 构建超时

尝试删除 .git 目录再进行构建


## 部署 生产文档流程
1. 拉取药部署的官方文档git项目
2. cd 官方文档项目
2. rm .git
3. 安装依赖（如果没有更新依赖可以跳过此步骤）
4. 编译项目
5. 移动 _book 内的所有文件及子目录文件到网站目录(nginx可以解析图片、css、js、html、.json文件即可，纯静态)
