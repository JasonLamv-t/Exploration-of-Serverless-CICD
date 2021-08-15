# Serverless-Devs 基于 GhostScript 的 PDF 转 JPG 实践

该项目是基于GhostScript的PDF转JPG工具，借助[Serverless-Devs](https://github.com/Serverless-Devs/Serverless-Devs/blob/master/readme_zh.md)工具进行依赖安装并部署到阿里云函数计算，是一个serverless的简单示例。

## 开始之前

### Serverless-Devs

如果您的开发环境没有Serverless-Devs，如果您的开发环境具备[npm](https://www.npmjs.com/)，可执行以下命令进行安装：

```shell
npm install  @serverless-devs/s -g
```

或者 通过 [yarn](https://yarnpkg.com/) 进行安装

```shell
yarn global add  @serverless-devs/s
```

更多内容请参考[Serverless Devs Install-tutorial](https://github.com/devsapp/fc/blob/main/docs/Getting-started/Install-tutorial.md)。

### Docker

本文档涉及本地调试，因此需要开发环境具有[Docker](https://www.docker.com/)，您可根据您开发平台的不同安装不同版本，可参考[Serverless Devs Install-tutorial](https://github.com/devsapp/fc/blob/main/docs/Getting-started/Install-tutorial.md)可选部分。

### Aliyun RAM账号

前往[RAM 访问控制](https://ram.console.aliyun.com/users)创建一个子用户，赋予其管理函数计算(FC)服务权限「AliyunFCFullAccess」并创建AccessKey。

### Serverless-Devs密钥配置

参考[配置阿里云密钥](https://github.com/devsapp/fc/blob/main/docs/Getting-started/Setting-up-credentials.md)及[S config](http://www.serverless-devs.com/docs/command#config指令)，将上一步创建的子用户AccessKey配置到S中。

## 开发

### 初始化项目

执行`s init https://github.com/JasonLamv-t/serverless-devs-ghostscript_example.git`来初始化项目，并修改s.yaml中的access字段为您自己的密钥别名。

### 安装依赖并构建

```shell
$ s build
```

### 添加测试文件

在项目目录下添加一个名为「test.pdf」的测试文件。

### 构建Runtime并安装依赖

执行`s build`，S检测到项目中存在「Funfile」时将会以Custom Container模式构建并安装依赖，得到`Build artifact successfully`时说明构建成功。

### 本地调用

```shell
$ s local invoke
```

可以查看文件`.s/build/artifacts/ghostscript/pdf2jpg-test/test.jpg`查看效果

### 部署

```shell
$ s deploy    
```

### 云端调用

``` shell
$ s invoke
```

## 参考

[awesome-fc/ghostscript_example: 基于 GhostScript 的 PDF 转 JPG Serverless 示例项目 (github.com)](https://github.com/awesome-fc/ghostscript_example)

