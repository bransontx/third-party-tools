### 第三方工具类

###### 实现功能：

- 第三方登录
    1. 微信
    2. 支付宝
    3. Apple
- 第三方支付
    1. 微信
    2. 支付宝
    3. Apple
- 短信
    1. 阿里云
- OSS
    1. 阿里云

需要依赖：

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
</dependency>

<dependency>
    <groupId>commons-codec</groupId>
    <artifactId>commons-codec</artifactId>
</dependency>

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.75</version>
</dependency>


支付宝依赖、短信依赖
<dependency>
    <groupId>com.alipay.sdk</groupId>
    <artifactId>alipay-sdk-java</artifactId>
    <version>4.10.192.ALL</version>
    <scope>provided</scope>
    <exclusions>
        <exclusion>
            <artifactId>fastjson</artifactId>
            <groupId>com.alibaba</groupId>
        </exclusion>
        <exclusion>
            <artifactId>commons-logging</artifactId>
            <groupId>commons-logging</groupId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>com.aliyun</groupId>
    <artifactId>aliyun-java-sdk-core</artifactId>
    <version>4.4.6</version>
    <scope>provided</scope>
    <exclusions>
        <exclusion>
            <artifactId>commons-logging</artifactId>
            <groupId>commons-logging</groupId>
        </exclusion>
    </exclusions>
</dependency>

微信依赖
<dependency>
    <groupId>org.jdom</groupId>
    <artifactId>jdom2</artifactId>
    <version>2.0.6</version>
</dependency>

苹果登录依赖
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>com.auth0</groupId>
    <artifactId>jwks-rsa</artifactId>
    <version>0.9.0</version>
    <scope>provided</scope>
</dependency>
```





##### 配置类（com.gf.tools.config.ToolsProp）

```java
/*支付宝配置*/
String aliAppId;

String aliPublicKey;

String aliPrivateKey;

String aliGetWay;
/*支付宝配置*/

/*短信配置*/
String accessKeyId;

String accessKeySecret;

String smsRegionId;

String smsSignName;
/*短信配置*/

/*微信支付*/
String wxAppId;

String wxApiKey;

String wxMchId;

String wxAppSecret;

String wxPayUrl;
/*微信支付*/

/*OSS*/
private String ossEndpoint;
// 为空默认取 accessKeyId
private String ossAccessKeyId;
// 为空默认取 accessKeySecret
private String ossAccessKeySecret;
/*OSS*/
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

##### OSS工具

```
引入MAVEN
<dependency>
    <groupId>com.aliyun.oss</groupId>
    <artifactId>aliyun-sdk-oss</artifactId>
    <version>3.8.0</version>
</dependency>

获取实例：OSSClientUtil.getInstance(ToolsProp toolsProp)

/**
* 获取阿里云OSS客户端对象
* @return ossClient
*/
OSSClient getOSSClient()

/**
* 创建存储空间
* @param ossClient OSS连接
* @param bucketName 存储空间
*/
String createBucketName(OSSClient ossClient, String bucketName)

/**
* 删除存储空间buckName
* @param ossClient oss对象
* @param bucketName 存储空间
*/
void deleteBucket(OSSClient ossClient, String bucketName)

/**
* 创建模拟文件夹
* @param ossClient oss连接
* @param bucketName 存储空间
* @param folder 文件夹
* @return 文件夹名
*/
String createFolder(OSSClient ossClient, String bucketName, String folder)

/**
* 根据key删除OSS服务器上的文件
* @param ossClient oss连接
* @param bucketName 存储空间
* @param folder 文件夹名 如"folder/"
* @param pathFileName Bucket下的文件的路径名+文件名 如："images/girl.jpg"
*/
void deleteFile(OSSClient ossClient, String bucketName, String folder, String pathFileName)

/**
* 上传图片至OSS
* @param ossClient oss连接
* @param file 上传文件（文件全路径如：/home/users/images/girl.jpg）
* @param bucketName 存储空间
* @param folder 文件夹名 如"images/"
* @return String 唯一MD5数字签名
*/
String uploadObject2OSS(OSSClient ossClient, File file, String bucketName, String folder)
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
