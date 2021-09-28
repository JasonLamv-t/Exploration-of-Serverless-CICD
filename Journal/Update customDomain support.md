# 支持自定义域名

## 思路

目标函数配置了自定义域名，那么辅助函数需要配置 auto custom domain，然后 `s invoke` 时传入具体的 auto 生成的域名，就能支持自定义域名的调用。

## 测试

1. createSession 查看返回内容
2. 