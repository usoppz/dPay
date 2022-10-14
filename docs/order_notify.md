# 订单通知

说明

用户支付完成后，系统会自动向订单关联的回调地址（`notifyUrl`）发送通知消息，告知该笔订单已支付完成。

## 通知地址

```bash
POST notifyUrl（商户在订单创建接口中指定）
```

## 通知参数

| 参数名          | 含义             | 类型        | 说明                                                         |
| :-------------- | :--------------- | :---------- | :----------------------------------------------------------- |
| platformOrderNo | 官方订单号       | string(32)  | 商户可使用该字段，调用 `订单查询` 接口，核实用户是否支付成功。 |
| merchantOrderNo | 商讨自定义订单号 | string(64)  | 商户自定义订单号，原样返回。                                 |
| network          | 主网         | string(20) | 支付币种所使用的主网络，当前固定为 TRC20 |
| amount          | 订单金额         | double | 商户发起接口请求时指定的 `订单金额`。                        |
| currency        | 订单货币单位     | string(8)   | 商户发起接口请求时指定的 `订单币种`，固定为 `USDT`。         |
| symbol          | 订单支付币种     | string(16)  | 商户发起接口请求时指定的 `支付币种`                          |
| tradeAmt        | 订单支付金额     | double | 订单显示的支付金额，，精确到小数点后4为，由 `订单金额` + `订单币种` + `支付币种` 按照实时行情转换而来。 |
| hash            | 交易 hash        | string(128) | 区块链交易 hash。商户可打开区块链浏览器查询交易详情。        |
| signature       | 签名串           | string(300) | 安全校验签名串。                                             |

说明

数据验签规则为：

```
verify(sign(signAlgorithm,signature,publicKey,signData))
```

- signAlgorithm : 签名算法固定为 MD5withRSA 
- signature：dPay 签名串，返回数据中获取
- publicKey : 官方平台公钥，从dPay商户后台  设置 ->**平台公钥** 栏获取
- signData :  验签数据，组装顺序严格按照该顺序组装

```
platformOrderNo + merchantOrderNo + amount + tradeAmt+ network + symbol + currency + hash
```



## 通知示例

```json
{
    "platformOrderNo": "PO40b2ac118c8e4f0aab5219ceac0e3da8",
    "merchantOrderNo": "ZGbqEadw1puEgDeU",
    "network":"TRC20",
    "amount": "200.00",
    "currency": "CNY",
    "symbol": "USDT",
    "tradeAmt": "31.42",
    "hash": "71f36f7c3eb073a24d0d3e49af6990928a2ae04764c06c07d414acd3f743ae9c",
    "signature": "526517f4603f25ab9ab686c1730f17b5"
}
```

## 通知返回

商户在收到通知信息后，可返回以下内容，告知已收到回调通知：

```json
{
    "success"
}
```

## 通知重试

系统向商户创建订单时指定的 `notifyUrl` 发送 `回调通知` 后，如该 `notifyUrl` 返回的 `http` 结果不为 `success`，则系统会自动触发 `重试机制`。相关规则如下：

1. 间隔 `30秒` 会进行第1次重试。
2. 如第一次重试仍为失败，则间隔 `5分钟` 会进行第2次重试。

说明

商户也可在后台 `订单详情` 页面，动手触发 `回调通知`。