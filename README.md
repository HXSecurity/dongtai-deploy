# dongtai-deploy
[![DongTai-project](https://img.shields.io/badge/DongTai%20versions-beta-green)](https://huoxianclub.github.io/LingZhi/)

## 一、单机版部署
> docker-compose一键部署与docker镜像一键部署方案从阿里云私有镜像仓库拉取镜像，速度快，无网络问题，推荐使用

### 1. docker-compose一键部署
[部署方案](docker-compose/readme.md)

### 2. docker镜像一键部署方案
待更新

### 3. 源码一键部署

Linux/Mac环境，安装docker服务时，可运行`build_with_source.sh`脚本，指定当前及机器的内网IP地址，如：192.168.0.1
```
# 拉取最新的代码
$ git clone https://github.com/HXSecurity/DongTai.git DongTai

$ cd DongTai

# 运行shell脚本，一键部署
$ bash build_with_source.sh
```
即可成功打包docker镜像并运行，运行之后，通过：http://内网地址 访问即可

### 4. 手动部署

服务之间存在一定的依赖，部署时，需按照顺序进行部署，顺序如下：
- DongTai-webapi
- DongTai-openapi
- DongTai-engine
- DongTai-web
- agent

#### DongTai-webapi服务

源码部署

1.初始化数据库

安装MySql 5.7，创建数据库`DongTai-webapi`，运行数据库文件`conf/db.sql`，进入webapi目录，运行`python manage.py createsuperuser`命令创建管理员

2.修改配置文件

复制配置文件`conf/config.ini.example为conf/config.ini`并需改其中的配置，其中:
- engine对应的url为`DongTai-engine`的服务地址
- apiserver对应的url为`DongTai-openapi`的服务地址

3.运行服务

运行`python manage.py runserver`启动服务

#### DongTai-openapi服务

1.修改配置文件

复制配置文件`conf/config.ini.example`为`conf/config.ini`并需改其中的配置；其中：

- engine对应的url为`DongTai-engine`的服务地址
- apiserver对应的url为`DongTai-openapi`的服务地址
- 数据库配置为`DongTai-webapi`服务所使用的数据库

2.运行服务

运行`python manage.py runserver`启动服务

#### DongTai-engine服务
1.修改配置文件

复制配置文件`conf/config.ini.example`为`conf/config.ini`并需改其中的配置；其中：

- engine对应的url为`DongTai-engine`的服务地址
- apiserver对应的url为`DongTai-openapi`的服务地址
- 数据库配置为`DongTai-webapi`服务所使用的数据库

2.运行服务

运行`python manage.py runserver`启动服务

#### DongTai-web服务
1.安装`npm`依赖
```bash
$ npm install
```

2.编译为可发布版本
```bash
$ npm run build
```

3.将`dist`目录放入nginx服务的静态资源目录

4.修改nginx配置，设置前端接口对应的后端服务，nginx的配置可参考`nginx.conf`

## 二、集群版部署
### Kubernetes版本一键部署
[部署方案](kubernetes/README.md)
