# 接口说明



## 编码

数据编码统一为 `utf-8`。



## appId

商户应用ID，用于确认商户唯一身份，从商户后台 设置->平台公钥 中获取。

## 字段选填


对于接口参数标记为 选填 的字段，可传入具体的值或者空字符串。



## signature

为保障接口安全，系统端会对接收到的所有数据，使用 `signature` 匹配校验，防止数据被非法篡改。

对于 `signature` 的生成算法为`md5WithRsa`。

举个例子：

如某个接口需要提交数据 `orderNo`、 `amount`两个字段，且 :

`privateKey` = `*********`（privateKey 由商户保管，且不可泄露给第三者）

`orderNo` = `PO82C7`，`amount` =  `100`。

根据 `signature` 的生成规则：

<font size=5 >`toHex(sign(md5WithRsa,privateKey, orderNo+amount))`</font>

则最终提交的 `signature` 数据为：

`toHex(sign('md5WithRsa',privateKey,'PO82C7100')) = 4263378beb1c22573a6a64f9e61f7545de11fbc1de11edb98431a73484defa52c0b71d45fbefa4c8502bf916e889441aacd7f9643b2fedb424d9fe4a1a4b7489772b558fef3b75f6f02e387e2bcffb17281044516185e676f4af93e425bfe608dbe5b17b168bd8db0d8ce2a43f09002da23f5c68f180e35c2b49918034aa2196`。



<font color=red>**注：`toHex(data)` 表示将签名后的数据转换为16进制字符串 。具体实现可以参考[dPay demo](https://github.com/usoppz/dPayDemo-java). <br>&nbsp;&nbsp;&nbsp;&nbsp;   `privateKey` 由商户从平台获取后由自身保管，且不可泄露给他人！！**</font>

