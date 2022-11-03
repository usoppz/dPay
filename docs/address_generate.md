# 收款地址生成

> 商户可调用该接口，生成自己的收款地址。该操作不会替换当前已启用的收款地址。

!>  该接口返回的数据是一次性的，平台不会存储任何内容。<br>
  钱包地址的私钥信息非常重要，请务必妥善保管，否则可能会导致意外的资金损失。


## 接口地址

```bash
GET https://dpaycoin.com/api/address/generate
```

## 接口参数

| 参数名    | 含义       | 验证 | 类型        | 说明                                           |
| :-------- | :--------- | :--- | :---------- | :--------------------------------------------- |
| appId     | 商户应用ID | 必填 | string(40)  | 商户应用ID，从商户后台 设置->平台公钥 中获取。 |
| network     | 主网 | 必填 | string(20) | 需要生成地址的主网络，当前固定为  `TRC20 ` |
| signature | 签名串     | 必填 | string(300) | 安全校验签名串。                               |

说明

数据签名规则为：

```
toHex(sign(signAlgorithm,privateKey,signData))
```

- signAlgorithm : 签名算法固定为 MD5withRSA 

- privateKey : 商户RSA私钥，从商户后台 设置->**商户密钥** 中获取（<font color=red>注：不要轻易泄露给他人</font>）

- signData :  签名数据如下

```
appId + network
```

## 接口返回

| 参数名    | 含义         | 类型        | 说明                               |
| :-------- | :----------- | :---------- | :--------------------------------- |
| address   | 钱包地址 | string(38)  | 钱包地址。 |
| privateKey   | 钱包地址私钥 | string(38)  | 钱包地址私钥。 |
| signature | 签名串       | string(300) | 安全校验签名串。                   |

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
appId + address + privateKey
```

## 返回示例

```json
{
    "success": true,
    "code": 200,
    "msg": "SUCCESS",
    "data": {
        "address": "TW4FVhVqxeCUUZXM2aWKK62YJGNpqrfgGE",
        "privateKey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        "signature": "874e8636425bb2f03b0c05ab0b280b9ea35ca25bb4942dcad650a51c0e953a80a3e49b317a51a9875b7af606acda5e607decdbaed3700673de3fed09b569854c0723fc279b4d227bd7241cc61f277b4a193846b9edbaeffd1f19244d30f6ef4e49be98b46df20cf396927073336987e34dd01a68baa837bf9a15239d160507e7"
    }
}
```