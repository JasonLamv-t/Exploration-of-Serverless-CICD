# Issue discussion

## [本地调试和端云联调， 本地函数计算的执行环境， 最好能做一些相关限制 · Issue #80 · devsapp/fc (github.com)](https://github.com/devsapp/fc/issues/80)

> reference：https://help.aliyun.com/document_detail/51907.html
> 比如有 cpu 和 内存 quota 限制， 线程和进程数目限制等

思路：

proxied CA：

- no customer container：init method 生成 docker 参数时根据yaml进行配置
- customer container：需要回去review代码

local start：需要review，实现逻辑应该和proxied差不多

## [端云联调： 辅助函数如何支持oss 触发器 · Issue #89 · devsapp/fc (github.com)](https://github.com/devsapp/fc/issues/89)

> 假设是如下场景， 有一个函数， 有一个 oss 触发器， 触发规则前缀是/a, 后缀是空， s 工具创建端云联调辅助函数的时候， 辅助函数可能因为 oss 的触发规则， 导致不能在辅助函数上创建 oss 触发器， 这个时候应该如何处理？
> 辅助函数创建成功， 吞掉不能创建触发器的错误， 提示用户可以临时将 oss 触发器指向辅助函数？

不能理解场景，需要进行相关测试

相同触发规则只能配一个函数

## [端云联调，我希望 proxied invoke 的时候， 能输出调用的具体是哪个辅助函数 · Issue #103 · devsapp/fc (github.com)](https://github.com/devsapp/fc/issues/103)

![image-20210825171022054](https://tva1.sinaimg.cn/large/008i3skNgy1gtt5eajrctj60iw0chq5202.jpg)

这个issue似乎已经解决了

### [s proxied invoke 支持自定义域名 · Issue #206 · devsapp/fc (github.com)](https://github.com/devsapp/fc/issues/206)

需要讨论具体场景