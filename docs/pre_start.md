# 准备工作



## 接入步骤

欢迎阅读 `dPay API` 技术接入文档。在开始接入之前，你需要：

### 前期准备

1. 访问并登录 [商户后台](https://dpaycoin.com/login.html)。

2. 在商户后台分别查看技术接入必需的dPay平台公钥、商户私钥以及appId：

    `publicKey` & `privateKey` & `appId`。

3. 在商户后台配置你的 `收款地址` （也可以通过API接口方式配置）。

4. 阅读我们 [接口约定](https://usoppz.github.io/dPay/#/interface_desc) 及 [安全说明](https://usoppz.github.io/dPay/#/security_desc) 相关的内容。

### 代码接入（Java）

1. 新增配置文件 <u>config.properties</u>，并加入如下内容：

   ```properties
   host =https://dpaycoin.com
   ##商户私钥 merchant private key
   merchantPrivateKey =MIICdQIBADANBgk********** ## 前期准备 步骤2 获取到的商户私钥
   ##dPay平台公钥 platform public key
   dPayPublicKey =MIGfMA0GCSqGSIb************  ## 前期准备 步骤2 获取到的dPay平台公钥
   ##merchant appId
   appId =4c1fe739-54f9-44f3-bba8-c334a4541a7b  ## 前期准备 步骤2 获取到的appId
   ```

2. 通过下载SDK源码自行打包后引入jar包，或者直接通过maven、gradle 配置直接引入

   SDK源码下载地址：[https://github.com/usoppz/dPay-SDK](https://github.com/usoppz/dPay-SDK)

   maven配置：

   ```xml
   <dependency>
     <groupId>com.dpaycoin</groupId>
     <artifactId>dPay-SDK</artifactId>
     <version>1.0</version>
   </dependency>
   ```
   gradle配置：

   ```apl
   implementation 'com.dpaycoin:dPay-SDK:1.0'
   ```

   

3. 编写代码，下面代码以创建订单为例：

   ```java
   public static void main(String[] args) {
       //一句代码就可以搞定支付订单的创建
       SignOrderDTO order = OrderClient.createOrder(getOrderReq());
       //打印返回数据
       System.out.println(JSON.toJSONString(order));
   }
   /**
    * 订单创建请求参数
   */
   private static PayOrderReq getOrderReq() {
       PayOrderReq req = new PayOrderReq();
       req.setOrderNo("PO123455678WD8GD0SD6668879BEF");
       req.setAmount(50.00);
       req.setCurrency("cny");
       req.setSymbol("usdt");
       req.setOrderType(2);
       req.setNetwork("trc20");
       req.setCustomerNo("test000301");
       req.setProductName("product0001");
       req.setReturnUrl("http://yourdomain/v1/order/returnUrl");
       req.setNotifyUrl("http://yourdomain/v1/order/notifyUrl");
       return req;
   }
   ```

   <font color='red'>注：appId 在配置文件配置后，可以不用在请求参数里面在设置appId。</font>

   

**说明**

其他接口你也可以直接参考 [Demo](https://github.com/usoppz/dPayDemo-java)，快速接入我们的API服务。

## 遇到问题

如你在接入过程中遇到任何问题，请优先阅读我们的 常见问题 文档。你还可以：

通过电报和我们取得联系。联系电报: [@dpayz](https://t.me/dpayz)。

