# 接入工具

目前 `dPay` 已提供 `java` 编程语言的接入示例及开发工具包。商户可参考相关项目，快速接入平台服务。

## Swagger  接入工具

https://dpaycoin.com/swagger-ui.html  swagger地址

<img src="images/swagger.png" alt="image-20221207111147873" style="zoom:80%;" />

## java 接入Demo

[https://github.com/usoppz/dPayDemo-java](https://github.com/usoppz/dPayDemo-java)

## java-SDK
[https://github.com/usoppz/dPay-SDK](https://github.com/usoppz/dPay-SDK)

可以通过下载SDK自行编译后，引入`jar` 包。也可以通过maven 或者 gradle 方式直接引入：

maven:

```xml
<dependency>
  <groupId>com.dpaycoin</groupId>
  <artifactId>dPay-SDK</artifactId>
  <version>1.0</version>
</dependency>
```

gradle:

```apl
implementation 'com.dpaycoin:dPay-SDK:1.0'
```

