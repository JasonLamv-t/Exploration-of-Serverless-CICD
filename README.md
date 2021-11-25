# Serverless CI/CD 探索

官网：http://www.serverless-devs.com/#/home

归属项目：[Summer-2021 (iscas.ac.cn)](https://summer.iscas.ac.cn/#/org/prodetail/210770460)

导师：[张千风](https://github.com/git-qfzhang)

项目完成工作：

1. 基于Serverless Devs开发的前段CI/CD方案（指引文档及demo），并发布到官网：[官网文章](https://www.serverless-devs.com/blog/node-based-frondend-ci-cd)；[文章代码](https://github.com/JasonLamv-t/serverless-devs-node-based-cicd)；[示例项目](https://github.com/JasonLamv-t/serverless-cicd-example-vue)
2. 基于Serverless Devs 开发的PDF转图片开箱即用应用，已经发布至S工具组件仓库中：[官网文章](https://www.serverless-devs.com/blog/pdf2jpg-custom-runtime-dependent-installation-practices)；[基础代码](https://github.com/JasonLamv-t/serverless-devs-ghostscript_example)；[开箱即用应用]
3. 端云联调组件：
   1. 支持本地挂载NAS（需要配置VPC）：[Issue](https://github.com/devsapp/fc/issues/178)
   2. 支持OSS触发器：[Issue](https://github.com/devsapp/fc/issues/89)
   3. 修复本地挂载NAS未AssumeYes的问题：[Release Info](https://github.com/devsapp/fc-proxied-invoke/releases/tag/0.0.13)
   4. 支持自定义域名：[Issue](https://github.com/devsapp/fc/issues/206)
   5. 支持按s.yaml配置对本地CA资源进行限制：[Issue](https://github.com/devsapp/fc/issues/80)
4. FC-Common组件：[Repo](https://github.com/devsapp/fc-common)
   1. 添加根据内存大小生成容器资源限制配置方法
   2. 对已有的方法进行测试
   3. 完善组件文档
   4. 发布0.0.1版本
5. 修改s.yaml规范，去除冗余、陈旧信息：[PR](https://github.com/devsapp/fc/pull/240)
6. 本地调试组件：支持按s.yaml配置对本地CA资源进行限制：[Issue](https://github.com/devsapp/fc/issues/80)、[PR](https://github.com/devsapp/fc-local-invoke/pull/29)

项目部分源码已上传至Gitlab，具体可以参考文档。Journal部分主要就是以上工作过程中产生的过程文档。
