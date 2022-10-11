# 订单创建

> 用于生成 `USDT.TRC20` 的支付数据。商户可选择直接跳转至 `官方收银台` 供用户支付，也可以使用数据 `自定义收银台`。在用户支付成功后，系统将即时进行 `回调通知`。



## 接口地址

```bash
POST https://pro.dpay.com/v1/api/order/create
```



## 接口参数

| 参数名      | 含义               | 验证 | 类型        | 说明                                                         |
| :---------- | :----------------- | :--- | :---------- | :----------------------------------------------------------- |
| publicKey   | RSA 公钥           | 必填 | string(300) | 商户RSA公钥，从商户后台获取。                                |
| orderNo     | 商户端订单号       | 必填 | string(60)  | 系统在通知商户接口时，会带上这个参数。例：SO201710192541。   |
| amount      | 订单金额           | 必填 | Double      | 精确到小数点后4位。                                          |
| currency    | 订单币种           | 必填 | string(8)   | 订单币种单位。支持：`CNY`、`USD`。                           |
| symbol      | 支付币种           | 必填 | string(16)  | 订单支付币种。当前固定为 `USDT`。                            |
| network     | 主网络             | 必填 | string(20)  | 支付币种所使用的主网络，当前固定为 TRC20                     |
| notifyUrl   | 完成后回调通知地址 | 选填 | string(255) | 1. 用户支付完成后，系统会发送一个 `post` 消息到这个地址。 2. 该参数不需要 `urlencode`。例如：http://www.demo.com/pay_notify。 3. 商户也可统一在后台对该参数进行配置。 |
| returnUrl   | 完成后同步跳转地址 | 选填 | string(255) | 1. 用户支付完成后，系统会自动跳转到这个地址。 2. 该参数不要 `urlencode`。例如：https://www.demo.com/pay_return。 |
| customerNo  | 商户端用户编号     | 选填 | string(60)  | 可以为用户名，也可以为数据库中的用户编号。例：sss@gmail.com，xxx等。 |
| productName | 商户端产品名称     | 选填 | string(60)  | 该参数会显示在官方收银台页面顶部，留空则显示默认值。         |
| locale      | 收银台多语言       | 选填 | string(12)  | 该参数指定收银台多语言，支持：中文（zh-CN），英文（en-US），默认为中文。 |
| signature   | 签名串             | 必填 | string(300) | 安全校验签名串。                                             |

说明

`signature` 的生成规则为：
```
toHex(sign(signAlgorithm,privateKey,signData))
```
signAlgorithm : 签名算法固定为 MD5withRSA 
privateKey : RSA私钥，从商户后台获取（<font color=red>注：不要泄露给他人</font>）
signData :  

```
orderNo + amount + network + symbol + currency + returnUrl + notifyUrl + customerNo + /n productName //组装顺序严格按照该顺序连接，服务端会根据该顺序进行验签
```



## 接口返回

| 参数名     | 含义           | 类型        | 说明                                                         |
| :--------- | :------------- | :---------- | :----------------------------------------------------------- |
| orderNo    | 官方订单号     | string(32)  | 商户可使用该字段，调用 `订单查询` 接口，核实用户是否支付完成。 |
| amount     | 订单金额       | Double      | 订单金额，原数据返回                                         |
| currency   | 订单币种       | string(8)   | 订单币种，原数据返回                                         |
| symbol     | 订单支付币种   | string(16)  | 支付币种，原数据返回                                         |
| tradeAmt   | 订单支付金额   | Double      | 支付金额，由 `订单金额` + `订单币种` 按照 `USDT.TRC20` 实时行情转换而来。 |
| address    | 订单收款地址   | string(64)  | 商户配置的接收用户支付的钱包地址。                           |
| cashierUrl | 官方收银台地址 | string(256) | 官方收银台 `url` 地址，商户可直接跳转到该地址供用户支付。    |
| imageStr   | 收款码         | string      | 供钱包扫码支付的二维码 图片base64字符串格式。                |
| usefulTime | 订单过期时间   | Date        | 订单过期时间。                                               |
| signature  | 签名串         | string(300) | 安全校验签名串。                                             |

说明

数据验签规则为：

```
verify(sign(signAlgorithm,signature,publicKey,signData))
```

signAlgorithm : 签名算法固定为 MD5withRSA 
publicKey : 官方平台RSA公钥，从返回数据中获取
signData :  

```
orderNo + amount + tradeAmt + currency + symbol + usefulTime + address + imageStr + /n
cashierUrl //组装顺序严格按照该顺序连接，dPay服务端签名顺序
```

说明

订单支付金额（`tradeAmt`），由 `订单金额` + `订单币种`，按照 `USDT.TRC20` 实时行情转换而来。



### 返回示例

```json
{
  "code": 200,
  "data": {
      "orderNo": "80841278ad144a54a70d443c1354f7c1",
      "amount": "100.00",
      "currency": "CNY",
      "symbol": "USDT",
      "tradeAmt": "15.66",
      "address": "TNN47tpcWKoNYuWGKEGMPdW653ZFSnDsy9",
      "qrcodeUrl": "https://pro.dPay.com/api/transaction/qrcode?token=80841278ad144a54a70d443c1354f7c1",
      "cashierUrl": "https://pro.dPay.com/v1/api/order/pay?orderNo=80841278ad144a54a70d443c1354f7c1",
      "signature": "49acae00de1a6e003e2010a5563249db",
      "usefulTime": 1800,
  },
  "success": true
}
```