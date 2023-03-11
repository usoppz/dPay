# 收款地址启用|禁用

> 商户可调用该接口，启用|禁用已有的收款地址。启用后，同一主网已启用的其他地址会自动设为禁用。（该接口主要针对无需归集的类型订单）

## 接口地址

```bash
POST https://dpaycoin.com/api/address/disabled
```

## 接口参数

| 参数名    | 含义       | 验证 | 类型        | 说明                                           |
| :-------- | :--------- | :--- | :---------- | :--------------------------------------------- |
| appId     | 商户应用ID | 必填 | string(40)  | 商户应用ID，从商户后台 设置->平台公钥 中获取。 |
| address | 商户收款地址 | 必填 | string(38) | 商户订单收款地址 |
| status | 使用状态 | 必填 | byte | 地址使用状态： 1.启用  2.禁用 |
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
appId + address + status
```


## 返回示例

```json
{
    "success": true,
    "code": 200,
    "msg": "SUCCESS",
    "data": null
}
```