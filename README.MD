### 第三方工具类

###### 说明：一下功能已全部实现,需要请联系作者: qq1969696789
```
实现了微信、支付宝、Apple的支付和登录、阿里云短信验证码
```
##### 配置类（com.gf.tools.config.ToolsProp）

```java
/*支付宝配置*/
private String AliAppId;

private String AliPublicKey;

private String AliPrivateKey;

private String AliGetWay;
/*支付宝配置*/

/*短信配置*/
private String SmsAccessKeyId;

private String SmsSecret;

private String SmsRegionId;

private String SmsSignName;
/*短信配置*/

/*微信支付*/
private String WxAppId;

private String WxApiKey;

private String WxMchId;

private String WxAppSecret;

private String WxPayUrl;
/*微信支付*/
```

##### payment 支付模块(com.gf.tools.payment.base.Payment)

######        

###### 支付宝支付(AliPayment)

```java
/**
  * 支付宝支付
  * @param paymentRequest {@link PaymentRequest}
  * @param prop {@link ToolsProp}
  * @return {@link PaymentResponse}
  */
@Override
PaymentResponse prepare(PaymentRequest paymentRequest, ToolsProp prop)
        
/**
  * 支付宝支付退款
  * @param refundRequest 退款参数
  */
static AlipayTradeRefundResponse refund(RefundRequest refundRequest, ToolsProp prop)        
               
/**
  * 线上资金授权冻结
  * @param fundAuthRequest 冻结参数 {@link FundAuthRequest}
  * @param prop {@link ToolsProp}
  * @return 跳转支付宝的orderString
  */
AlipayFundAuthOrderAppFreezeResponse fundAuthOrderAppFreeze(FundAuthRequest fundAuthRequest, ToolsProp prop)

/**
  * 预授权解冻
  * @param fundAuthUnFreezeRequest {@link FundAuthUnFreezeRequest}
  * @param prop 支付宝参数  {@link ToolsProp}
  * @return  {@link FundAuthUnFreezeResponse}
  */
FundAuthUnFreezeResponse fundAuthOrderUnFreeze(FundAuthUnFreezeRequest fundAuthUnFreezeRequest, ToolsProp prop)
    
/**
  * 支付宝预授权转支付
  * @param fundAuthToTradeRequest {@link FundAuthToTradeRequest}
  * @return  {@link FundAuthToTradeResponse}
  */
FundAuthToTradeResponse tradePay(FundAuthToTradeRequest fundAuthToTradeRequest, ToolsProp prop)

```

######       

###### 苹果内购(ApplePayment)

```java
/**
  * 苹果内购，支付验证成功orderStr则返回success，success返回true
  * 失败则会报错，验证成功后业务自理
  * @implSpec ToolsProp不用传
  */
PaymentResponse prepare(PaymentRequest paymentRequest, ToolsProp prop)
```

######       

######  weixin 微信支付

```java
/**
  * 微信支付
  * @param paymentRequest 支付参数 {@link PaymentRequest}
  * @param prop 微信配置
  * @return {@link PaymentResponse}
  */
@Override
PaymentResponse prepare(PaymentRequest paymentRequest, ToolsProp prop)
```



##### social 登录模块(com.gf.tools.social.base.Login)

  

###### 支付宝

```java
/**
  * 支付宝登录
  * @param request 登录参数 {@link LoginRequest}
  * @param prop 第三方应用配置
  * @return 登录返回值 {@link LoginResponse}
  */
LoginResponse login(LoginRequest request, ToolsProp prop)
```

######         

###### 苹果IOS

```java
/**
  * 苹果登录
  * @param request 登录参数 {@link LoginRequest}
  * @param prop 苹果登录不需要传
  * @return 登录返回值 {@link LoginResponse}
  */
LoginResponse login(LoginRequest request, ToolsProp prop)
```



######  微信

```java
/**
  * 微信登录
  * @param request 登录参数 {@link LoginRequest}
  * @param prop 第三方应用配置
  * @return 登录返回值 {@link LoginResponse}
  */
LoginResponse login(LoginRequest request, ToolsProp prop)
```

#####     sms 短信模块



###### AliyunSms 阿里云短信

```java
@Resource
private AliyunSms aliyunSms;

@Test
public void testGet(){
    // 模板内容示例：验证码${code}，您正在登录，若非本人操作，请勿泄露。
    // 生成验证码
    String code = String.valueOf(new Random().nextInt(10000));
    Map<String, String> params = new HashMap<>();
    params.put("code", code);
    //无报错信息则发送成功
    aliyunSms.sendSmsAliyun("手机号", "模板ID", params);
    //处理业务：保存验证码
}
```



##### 其他方法



###### 支付宝

```java
/**
  * 支付宝回调获取参数
  */
Map<String, String> PaymentUtils.alipayCallbackParams(HttpServletRequest request)

/**
  * 获取预授权回调参数
  * @param request request
  * @return @see {@link FundAuthResponse}
  */
FundAuthResponse PaymentUtils.getFundResponse(HttpServletRequest request)
    
/**
  * 支付宝回调参数验证签名
  * @param request request请求
  * @param publicKey 支付宝公钥
  * @return 验证结果
  */    
boolean PaymentUtils.verifySignature(HttpServletRequest request, String publicKey)
```

######   	支付返回参数解析（微信、支付宝）

说明:该方法支付宝和微信通用，需要传入微信和支付宝的秘钥,只用微信或支付宝则只需传一个

```java
/**
  * 支付返回参数
  * @param wxApiKey 微信APIKEY
  * @param aliPublicKey 支付宝公钥
  * @param tradeType 支付类型 {@link TradeType}
  * @return 返回参数 {@link CallbackParams}
  */
static CallbackParams getCallbackParams(TradeType tradeType, String wxApiKey, String aliPublicKey , HttpServletRequest request)
```

###### 微信工具

获取小程序手机号

```
/**
  * 解密获取手机号
  * @param encryptedData 包括敏感数据在内的完整用户信息的加密数据
  * @param iv 加密算法的初始向量
  * @param sessionKey 服务端获取
  * @param AppId 微信AppId
  */
String WxUtils.wxGetPhone(String encryptedData, String iv, String sessionKey, String AppId)
```
