# 端云联调支持nas

## 需求探究

### 现有端云联调

s proxied setup后，本地run两个容器，一个是代理，一个是代码
云端部署了一个event-function，即辅助函数，命名规则为session-\*，环境参数有tunnel\*等，可以看出中间有一个阿里云搭建的隧道，辅助函数与本地代理通过token进行握手穿透。

### 挂载nas测试

修改s.yaml，添加nas config为auto
执行` s nas upload index.js /mnt/auto/index.js`报错`Message:  Not fount nasConfig.`
执行`s deploy`后即正常，即auto需要进行配置，首次deploy时会产生如下的辅助函数，是否需要删除呢？

![image-20210817101811817](https://tva1.sinaimg.cn/large/008i3skNgy1gtjkiz9g18j607204n0sm02.jpg)

执行`s invoke`后，通过执行`s exec -- nas command ls /mnt/auto`可以看到invoke创建的文件

`s local invoke`创建的文件在`.s/nas/auto-default`下

![image-20210817141223306](https://tva1.sinaimg.cn/large/008i3skNgy1gtjram47oyj609n02g74702.jpg)

### 端云联调测试

#### 正常

#### 添加logConfig和nasConfig

nas调用相当于local invoke，即没有挂载上

问题：没有执行s deploy情况下，直接进行proxied setup要如何创建对应的nas？

#### setup后直接容器内部挂载

报错和issue一致

## 解决思路

setup过程中解析nasConfig，然后创建对应资源，然后在ca构建阶段对nas进行挂载

## 其他问题

### ✅ Buffer

经常报以下错误

```bash
(node:56168) [DEP0005] DeprecationWarning: Buffer() is deprecated due to security and usability issues. Please use the Buffer.alloc(), Buffer.allocUnsafe(), or Buffer.from() methods instead.
(Use `node --trace-deprecation ...` to show where the warning was created)
```

### 费用

![image-20210817133426199](https://tva1.sinaimg.cn/large/008i3skNgy1gtjq75j3zuj60ji04fwej02.jpg)

一个demo而已，也有正常返回，为什么使用了这么多的资源0.0

![image-20210820171003277](https://tva1.sinaimg.cn/large/008i3skNgy1gtndafrbyoj60lj05emxf02.jpg)

![image-20210822064805468](https://tva1.sinaimg.cn/large/008i3skNgy1gtp6jwot81j607401pq2q02.jpg)

怀疑是预留资源没有正确释放掉

### RAM问题

![image-20210820192020833](https://tva1.sinaimg.cn/large/008i3skNgy1gtnh1yub4bj60ty0k6jug02.jpg)

### VPC无法删除

![image-20210822064853121](https://tva1.sinaimg.cn/large/008i3skNgy1gtp6kovfkoj612f08p3z202.jpg)

## 实现

1. 添加test，检查传入参数，并获取nasconfig

2. ![image-20210818203652969](https://tva1.sinaimg.cn/large/008i3skNgy1gtl810zbk3j60d700sglh02.jpg)

3. handlerInputs添加nasConfig的处理，并抛出

4. 找一下CA的创建是在哪里：local-invoke

5. 原nas部分逻辑

   ​	初始化

   ​		读取nasConfig

   ​			解析nasConfig，转换为本地映射来进行挂载

6. 原docker逻辑

   ​	初始化

   ​		根据配置挂载

   ​		生成docker名称，根据rutime拉取镜像等

   ​	启动

   ​		判断代理容器

   ​		配置环境变量

7. 思路

   1. 解析nasConfig，auto的话按原来的逻辑，非auto的解析mount继续
   2. 修改new docker hostConfig，添加privileged
   3. cmd，添加挂载操作

8. 跑动：registry.cn-beijing.aliyuncs.com/aliyunfc/runtime-nodejs8:v1.9.19

   没跑起来



1. 添加privileged ✅

2. 绕过s.yaml，直接挂载试试
   1. 在exec curl 127.0.0.1:port 前exec mount操作，会卡住，但是容器run起来了，也就是res测试过程出现了问题
   2. 修改了FC_DOCKER_VERSION为1.9.19，尝试进去容器里面挂载
      `mount -t nfs -o vers=3,nolock,proto=tcp,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 23c874843f-lth37.cn-shenzhen.nas.aliyuncs.com:/ /mnt`
   3. mount操作无响应
   4. 怀疑是辅助函数没有挂载vpc
   5. 挂vpc失败，重新尝试生成vpc和nas
   6. 权限问题

3. 可以通

4. 尝试在监测容器存在后执行touch成功

5. 尝试init过程修改cmd失败

6. init过程判断是否auto，auto按原来思路挂载本地，非Auto删除mount参数 ✅

7. 删除CA vpcConfig ❌ 本地CA不涉及vpc

8. 删除辅助函数全部nasConfig ✅

9. 添加挂载命令 ✅

10. 情况：

11. 

    |           | nasAuto | nasNone           | nasNoAuto                        |
    | --------- | ------- | ----------------- | -------------------------------- |
    | vpcAuto   | 本地 ✅  | Permission denied | 拒绝 ✅                           |
    | vpcNone   | 本地 ✅  | Permission denied | 拒绝 ✅                           |
    | vpcNoAuto | 本地 ✅  | Permission denied | 配置正确通过，配置错误还需要处理 |

12. 效果图

    ![image-20210824140852063](https://tva1.sinaimg.cn/large/008i3skNgy1gtrujon5w7j60cf09k40502.jpg)



​		

