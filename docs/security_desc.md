# 安全说明



## 密钥

商户的 `public key` 和 `private key` 非常重要，请勿将private key显示在任何网页代码、网址参数中。



## signature

为保障接口安全，系统端会对接收到的所有数据，进行 `signature` 匹配校验，建议商户端包含同样的校验逻辑，防止意外的安全风险。



## 账户安全

建议商户开启基于 `Google authenticator` 的两步登录安全验证，防范因账密泄漏导致的资产安全问题。



## 接口安全

商户可在系统后台配置 `接口IP白名单`，防范因密钥泄漏导致的资产安全问题。



## HTTPS

部分接口需要提供 `notify_url` & `redirect_url`，建议商户对服务器部署 `https` 服务，保障数据的通信安全。
