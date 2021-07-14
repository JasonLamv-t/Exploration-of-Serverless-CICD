# Serverless-devs的快速入门

## 前置步骤
1. 通过yarn全局安装@serveless-devs/s
2. 配置aliyun账号信息

## helloworld demo

### [参考文档](https://github.com/devsapp/fc/blob/main/docs/Getting-started/Hello-world-application.md)

### 存在的问题
1. 执行local start时，没有设置defalut账户的话需要一直手动选择才能响应（yaml设置默认access即可解决）
2. 首次调用需要下载依赖，而非提前下载，比如放在build阶段

## node based frondend cicd service

基于node-http触发实现的前端项目CI/CD，[参考实现思路](https://bp.aliyun.com/detail/73?accounttraceid=90cce8fe2f794b50821f5ceb26efd02eetqb)，本函数通过代码托管webhock触发，拉取指定tag的分支并上传到对象存储中。
实际应用中存在以下问题：

- 前端代码一般需要执行打包操作，因此需要有克隆后自定义执行的命令或脚本
- 需要配置静态页面托管（本例子跳过了绑定域名并配置https证书等相关步骤）

### 过程
1. 使用`s init`进行交互式初始化，这里选择的node12-http
2. 打印参数和输入，并尝试返回输出，这里使用apipost和edge来进行测试
3. 设置github webhock，尝试触发
4. 修改yaml，地域修改为深圳，请求方式设置为post、get
5. 观察github webhock 参数
6. body.toString后@会解析为undefined，通过替换解决，
7. 上传密钥和shell脚本，然后好像oss不能在线修改文件名
8. 尝试直接把密钥和脚本放代码里，报read only fs, 权限问题（通过操作/tmp解决）
9. 部署后报错，代码执行没有可用日志，不清楚是不是代码问题（配置问题，部署时本地配置覆盖了云端日志）

### 存在的问题
1. 热重载的问题，如果不设置resp返回的话，重载失效
2. 端口问题，每次重新运行端口都会发生变化，似乎yaml不可指定端口（已提交issue）
3. 阿里云的日志还需要额外配置（控制台开启的话需要，建议自动开启日志）

### 目前进度
云端部署并实现