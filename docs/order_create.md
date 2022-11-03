# 订单创建

> 用于生成 `USDT.TRC20` 的支付数据。商户可选择直接跳转至 `官方收银台` 供用户支付，也可以使用数据 `自定义收银台`。在用户支付成功后，系统将即时进行 `回调通知`。



## 接口地址

```bash
POST https://dpaycoin.com/api/order/create
```



## 接口参数

| 参数名      | 含义               | 验证 | 类型        | 说明                                                         |
| :---------- | :----------------- | :--- | :---------- | :----------------------------------------------------------- |
| appId       | 商户应用ID         | 必填 | string(40)  | 商户应用ID，从商户后台 设置->平台公钥 中获取。               |
| orderNo     | 商户端订单号       | 必填 | string(60)  | 系统在通知商户接口时，会带上这个参数。例：SO201710192541。   |
| amount      | 订单金额           | 必填 | double      | 精确到小数点后2位。                                          |
| currency    | 订单币种           | 必填 | string(8)   | 订单币种单位。支持：`CNY`、`USD`。                           |
| symbol      | 支付币种           | 必填 | string(16)  | 订单支付币种。当前固定为 `USDT`。                            |
| network     | 主网络             | 必填 | string(20)  | 支付币种所使用的主网络，当前固定为  `TRC20 `                 |
| notifyUrl   | 完成后回调通知地址 | 选填 | string(255) | 1. 用户支付完成后，系统会发送一个 `post` 消息到这个地址。 2. 该参数不需要 `urlencode`。例如：http://www.demo.com/payNotify。 <br>3. 商户也可统一在后台对该参数进行配置。 |
| returnUrl   | 完成后同步跳转地址 | 选填 | string(255) | 1. 用户支付完成后，系统会自动跳转到这个地址。<br> 2. 该参数不要 `urlencode`。例如：https://www.demo.com/payReturn。 |
| customerNo  | 商户端用户编号     | 选填 | string(60)  | 可以为用户名，也可以为数据库中的用户编号。例：sss@gmail.com，xxx等。 |
| productName | 商户端产品名称     | 选填 | string(60)  | 该参数会显示在官方收银台页面顶部，留空则显示默认值。         |
| locale      | 收银台多语言       | 选填 | string(12)  | 该参数指定收银台多语言，支持：中文（zh-CN），英文（en-US），默认为中文。 |
| signature   | 签名串             | 必填 | string(300) | 安全校验签名串。                                             |

说明

`signature` 的生成规则为：
```
toHex(sign(signAlgorithm,privateKey,signData))
```
- signAlgorithm : 签名算法固定为 MD5withRSA 
- privateKey : 商户RSA私钥，从商户后台 设置->**商户密钥** 中获取（<font color=red>注：不要轻易泄露给他人</font>）
- signData :  签名数据，组装顺序严格按照该顺序组装

```
orderNo + amount + network + symbol + currency + returnUrl + notifyUrl + customerNo + productName
```



## 接口返回

| 参数名     | 含义           | 类型        | 说明                                                         |
| :--------- | :------------- | :---------- | :----------------------------------------------------------- |
| orderNo    | 官方订单号     | string(32)  | 商户可使用该字段，在商户后台通过 `订单查询` 功能，核实用户是否支付完成。 |
| amount     | 订单金额       | double      | 订单金额，原数据返回                                         |
| currency   | 订单币种       | string(8)   | 订单币种，原数据返回                                         |
| symbol     | 订单支付币种   | string(16)  | 支付币种，原数据返回                                         |
| tradeAmt   | 订单支付金额   | double      | 支付金额，精确到小数点后4为，由 `订单金额` + `订单币种` 按照 `USDT.TRC20` 实时行情转换而来。 |
| address    | 订单收款地址   | string(38)  | 商户配置的接收用户支付的钱包地址。                           |
| cashierUrl | 官方收银台地址 | string(256) | 官方收银台 `url` 地址，商户可直接跳转到该地址供用户支付。    |
| qrcodeUrl  | 收款码地址     | string      | 商户配置的接收用户支付的钱包地址二维码图片地址。             |
| usefulTime | 订单过期时间   | long        | 订单过期时间。格式：对应的全 `时间戳 `                       |
| signature  | 签名串         | string(300) | 安全校验签名串。                                             |

说明

数据验签规则为：

```
verify(sign(signAlgorithm,signature,publicKey,signData))
```

- signAlgorithm : 签名算法固定为 MD5withRSA 
- signature：dPay 签名串，返回数据中获取
- publicKey : 官方平台公钥，从商户后台 设置 ->**平台公钥** 中获取
- signData :  验签数据，组装顺序严格按照该顺序组装

```
appId + orderNo + amount + tradeAmt + currency + symbol + usefulTime + address + qrcodeUrl + cashierUrl 
```

说明

订单支付金额（`tradeAmt`），由 `订单金额` + `订单币种`，按照 `USDT.TRC20` 实时行情转换而来。



### 返回示例

```json
{
    "success": true,
    "code": 200,
    "msg": "SUCCESS",
    "data": {
        "orderNo": "POC712A9647CD2431F8FF3068C8AE1F907",
        "amount": 10,
        "tradeAmt": 9.9993,
        "currency": "usd",
        "symbol": "usdt",
        "usefulTime": 1665736446000,
        "address": "TW4FVhVqxeCUUZXM2aWKK62YJGNpqrfgGE",
        "qrcodeUrl": "https://dpaycoin.com/v1/api/order/qrcode?orderNo=POC712A9647CD2431F8FF3068C8AE1F907",
        "cashierUrl": "https://dpaycoin.com/v1/api/order/pay?orderNo=POC712A9647CD2431F8FF3068C8AE1F907",
        "signature": "874e8636425bb2f03b0c05ab0b280b9ea35ca25bb4942dcad650a51c0e953a80a3e49b317a51a9875b7af606acda5e607decdbaed3700673de3fed09b569854c0723fc279b4d227bd7241cc61f277b4a193846b9edbaeffd1f19244d30f6ef4e49be98b46df20cf396927073336987e34dd01a68baa837bf9a15239d160507e7"
    }
}
```