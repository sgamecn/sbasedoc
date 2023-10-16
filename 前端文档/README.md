# sbasedoc 中台接入文档--前端版本

## 接口文档

接口文档中的地址统一通过Tag配置来获取

## 登录

登录流程图
EMLogin 为公司快捷登录 PFLogin 为平台登录，例如：微信，QQ.....
![登录流程图](../image/login.png)

[EM登录](#EM)

[微信登录](#wechat)

[TapTap登录](#TapTap)

[QQ登录](#qq)

[英雄登录](#hero)

[第三方登录](#third)

[APPLE登录](#apple)

[登录通用回复](#loginResp)

[实名认证](#realcheck)

[发短信验证码](#SendMobileMessage)

[绑定手机号](#BindMobile)

[手机号登录](#MobileLogin)

## 支付

[支付下单](#transaction)

[apple支付](#applepay)

## 签名规则

[签名规则](#sign)

## Code中台常量定义

[code中台常量定义](#code)

### 防沉迷
[防沉迷详细规则](#chenmi)

---
### <a id="EM">EM登录</a>

路径：/EMLogin
```
Method: POST
```

请求参数
```protobuf
syntax = "proto3";

message EmLoginRequest {
    int64 Namespace = 1;
    
    // em参数
    string E = 2;
    string M = 3;
    
    string SGameId = 4; // 游戏ID
    string Sign = 5;  // 签名
}

// 响应参数 见通用登录回复
```
[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="wechat">微信登录</a> 
路径：/WechatLogin

[微信官方接入文档](https://developers.weixin.qq.com/doc/oplatform/Mobile_App/WeChat_Login/Development_Guide.html)

```
Method: POST
```

请求参数
```protobuf
syntax = "proto3";

message WechatLoginRequest {
  int64 Namespace = 1;

  string SGameId = 2; // 游戏ID
  string Sign = 3;  // 签名

  string Code = 4; // 微信返回的code
}

// 响应参数 见通用登录回复
```
[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="TapTap">TapTap登录</a> 
路径：/TapTapLogin 

[TapTap官方接入文档](https://developer.taptap.cn/docs/sdk/taptap-login/guide/start/)

[签名规则](#sign)

```
Method: POST
```

请求参数
```protobuf
syntax = "proto3";

message TTLoginRequest {
  int64 Namespace = 1;

  string SGameId = 2; // 游戏ID
  string Sign = 3;  // 签名

  string AccessToken = 4; // TapTap返回的access_token
  string MacKey = 5;  // TapTap返回的mac_key
}

// 响应参数 见通用登录回复
```
[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="qq">QQ登录</a>
路径: /QQLogin

[QQ官方接入文档](https://wiki.connect.qq.com/qq%e7%99%bb%e5%bd%95)
```
Method: POST
```
请求参数
```protobuf
syntax = "proto3";

//QQ登录
message QQLoginRequest {
  int64 Namespace = 1;

  string SGameId = 2; // 游戏ID
  string Sign = 3;  // 签名

  string AccessToken = 4; // QQ返回的access_token
}

// 响应参数 见通用登录回复
```
[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="hero">英雄登录 </a>
路径：/YXLogin

直接验证账号密码
```
Method: POST
ContentType: application/json
```

请求参数
```protobuf
syntax = "proto3";

message YXLoginRequest {
  int64 Namespace = 1;

  string SGameId = 2; // 游戏ID
  string Sign = 3;  // 签名

  string Account = 4; // 英雄账号
  string Passwd = 5;  // 英雄密码
}

// 响应参数 见通用登录回复
```
[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="third">第三方登录</a>
对应之前的渠道登录

路径：/ThirdLogin

```
Method: POST
```

请求参数
```protobuf
syntax = "proto3";

//第三方登录
message ThirdLoginRequest {
  int64 Namespace = 1;

  string SGameId = 2; // 游戏ID
  string Sign = 3;  // 签名

  string ChannelId = 4; // 渠道ID
  string CUid = 5;  // 渠道UID
}

```
[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="apple">APPLE登录</a> 
路径：/AppleLogin

[Apple 接入文档](https://blog.unity.com/technology/support-for-apple-sign-in)
```
Method: POST
ContentType: application/json
```

###### 请求参数
```protobuf
syntax = "proto3";

message AppleLoginRequest {
  int64 Namespace = 1;
  
  string SGameId = 2; // 游戏ID
  string Sign = 3;  // 签名
  
  string Code = 4;  // 苹果返回的code
}

// 响应参数 见通用登录回复
```
[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="SendMobileMessage">发短信验证码</a> 
路径：/SendMobileMessage

```
Method: POST
```

###### 请求参数
```protobuf
syntax = "proto3";

message SendMobileMessageRequest {
  string SGameId = 1;
  string Sign = 2;
  int64 Namespace = 3;
  string Mobile = 4; //手机号
  string SendType = 5; //发送类型 1绑定 2登录
}

message SendMobileMessageResponse {
  Code Code = 1;  //参考 code中台常量定义
}
```
[签名规则](#sign)

[code中台常量定义](#code)

### <a id="BindMobile">渠道用户绑定手机</a> 
路径：/BindMobile

```
Method: POST
```

###### 请求参数
```protobuf
syntax = "proto3";

message BindMobileRequest {
  string SGameId  = 1;
  string Sign = 2;
  int64 Namespace  = 3;
  string Mobile = 4;  //手机号
  string DeviceNo = 5;  //设备号
  string ValidateCode   = 6;  //验证码
  string ChannelId  = 7;    //渠道ID
  string CUid = 8;  //渠道用户ID
}

message BindMobileResponse {
  Code Code = 1;  //参考 code中台常量定义
}
```

[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="MobileLogin">手机号登录</a> 
路径：/MobileLogin

```
Method: POST
ContentType: application/json
```

###### 请求参数
```protobuf
syntax = "proto3";

message MobileLoginRequest {
  string SGameId = 1;
  string Sign = 2;
  int64  Namespace = 3;
  string Mobile = 4;
  string DeviceNo = 5;
  string ValidateCode   = 6;
}
```
[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="loginResp">通用登录回复</a>
```protobuf
syntax = "proto3";

//WlcIndulgeInfo 防沉迷信息
message WlcIndulgeInfo {
  bool IsAdult = 1; // 是否成年
  int64 UserBirthday = 2; // 用户生日

  int64 SingleRechargeMax = 3;  // 单笔充值最大金额 -1 无限制
  int64 AccumulateRechargeMax = 4;  // 累计充值最大金额     -1 无限制
}

message LoginResult {
  Code Code = 1;  //参考 code中台常量定义

  // em参数
  string E = 2;
  string M = 3;

  int64 Uid = 4;  // 用户ID
  string DisplayName = 5; // 用户昵称

  string PFType = 6;  // 平台类型
  string PFID = 7;  // 平台ID

  string Token = 8;
  string Key = 9;

  int64 WlcStatus = 10; // 实名认证状态 0 认证成功 1认证中 2 认证失败

  WlcIndulgeInfo WlcIndulgeInfo = 11; // 防沉迷信息
}

```

### <a id="realcheck">实名认证</a>   
路径：/WlcCheck

```
Method: POST
```

###### 参数
```protobuf
syntax = "proto3";

//Wlc上报(实名认证) Client->SBase
message WlcCheckReq {
  // em参数
  string E = 1;
  string M = 2;

  string Name = 3;
  string IdNum = 4;

  string SGameId = 5; // 游戏ID
  string Sign = 6;  // 签名
}

//Wlc上报(实名认证)回复 SBase->Client
message WlcCheckResp {
  Code Code = 1;  //参考 code中台常量定义
  int64 Status = 2; // 实名认证状态 0 认证成功 1认证中 2 认证失败
  WlcIndulgeInfo WlcIndulgeInfo = 3;  // 防沉迷信息
  bool CanPlay = 4; // 是否可以游戏
}

//WlcIndulgeInfo 防沉迷信息
message WlcIndulgeInfo {
  bool IsAdult = 1; // 是否成年
  int64 UserBirthday = 2; // 用户生日

  int64 SingleRechargeMax = 3;  // 单笔充值最大金额 -1 无限制
  int64 AccumulateRechargeMax = 4;  // 累计充值最大金额 -1 无限制
}
```
[code中台常量定义](#code)

### <a id="chenmi">防沉迷详细规则</a>

a.游戏时长限制，未成年用户仅可在周五、周六、周日和法定节假日的20时至21时进行游戏，并在21点整点强制下线；

b.游戏消费限制，未满12周岁的用户无法进行游戏充值；12周岁（含）以上未满16周岁的用户，单次充值上限50元人民币，每月充值上限200元人民币；16周岁（含）以上未成年用户，单次充值上限100元人民币，每月充值上限400元人民币。

中台登录时会进行判定判定时候可以登录 9点强制踢 前端自行实现

## 支付

### 通常支付
![支付流程图](../image/pay.png)
### <a id="transaction">支付下单（获取支付参数）</a>
路径：/Transaction

```
Method: POST
ContentType: application/json
```

```protobuf
syntax = "proto3";

//支付类型
enum TAType {
  TA_WECHAT_APP = 0;   // 微信APP
  TA_WECHAT_NATIVE  = 1; // 微信NATIVE
  TA_ALI  = 2;     // 支付宝
  TA_APPLE = 3;        // 苹果
}

//货币信息
message Amount {
  double Total = 1; // 总金额
  string Currency = 2;  // 货币类型 例如：CNY
}

//下单请求 Server->Client->SBase 服务器加工消息，客户端不用关心
message TransactionRequestEncode {
  string encodeStr = 1;
}

//下单请求回复 Client->SBase
message TransactionResponse {
  Code Code = 1;  //参考 code中台常量定义
  TAType Type = 2;  // 支付类型
  TransactionParam Param = 3;   // 支付参数
}

//下单回复参数 SBase->Client
message TransactionParam {
  bytes WeChatAppPayParam  = 1; // wechatAppPayParam Json结构 透传时为json.Marshal后的[]byte 结构为AppPayParams
  string WeChatNativePay  = 2;  // wechatNativePay 二维码链接
  string AliAppOrderInfo = 3;   // 支付宝app支付
  string AliWebOrderInfo = 4;  // 支付宝web支付
  string AppleApplicationUsername = 5;  // 苹果支付时，需要设置的applicationUsername
}

//wechat.AppPayParams
type AppPayParams struct {
    Appid     string `json:"appid"`  
    Partnerid string `json:"partnerid"`
    Prepayid  string `json:"prepayid"`
    Package   string `json:"package"`
    Noncestr  string `json:"noncestr"`
    Timestamp string `json:"timestamp"`
    Sign      string `json:"sign"`
}
```

[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="applepay">apple支付</a>
![支付流程图](../image/ApplePay.png)

[apple支付接入文档](https://developer.apple.com/documentation/passkit/apple_pay/offering_apple_pay_in_your_app)

接入appleSDK 进行下单时，请设置applicationUsername 为游戏服务器回传订单号

[SetApplicationUsername](https://docs.unity3d.com/Packages/com.unity.purchasing@3.0/api/UnityEngine.Purchasing.IAppleExtensions.html#UnityEngine_Purchasing_IAppleExtensions_SetApplicationUsername_System_String_)

对接中台方面，前端只处理第6步骤：支付回调上报，详情如图

#### 苹果支付回调
路径：/AppleVerifyIdTokenV1
```protobuf
syntax = "proto3";

//Apple支付回调 Client->SBase
message ApplePayVerifyIdTokenV1 {
  string SGameId = 1; // 游戏ID
  string Receipt = 2; // 苹果返回的receipt
}

//Apple支付回调 SBase->Client
message AppleVerifyIdTokenResponse {
  Code Code = 1;  //参考 code中台常量定义
  repeated string SuccessTransactionList = 2;   // 成功的订单列表
}

```
ApplePayVerifyIdTokenV1Rsp 参数 SuccessTransactionList 为成功的[transaction_id](https://developer.apple.com/documentation/appstorereceipts/responsebody/latest_receipt_info)

[code中台常量定义](#code)

### <a id="code">code中台常量定义</a>
```protobuf
syntax = "proto3";

enum Code {
  CODE_DEFAULT 			      = 0;
  CODE_SUCCESS            = 200; //成功
  CODE_PARAM_MISS         = 116; //参数缺失
  CODE_SERVICE_BUSY       = 126; //服务器繁忙（内部逻辑错误）
  CODE_TOKEN_EXPIRED      = 128; //令牌已过期
  CODE_INVALID_NAMESPACE  = 157; //无效的Namespace
  CODE_PASSWORD_ERROR     = 158; //密码错误
  CODE_ACCOUNT_NOT_EXIST  = 159; //账号不存在
  CODE_SEND_SMS_TOO_FAST  = 160; //发送短信过快
  CODE_VALIDATE_CODE_ERR  = 161; //验证码错误
  CODE_PAY_CALLBACK_ERROR  = 162; //支付回调错误
  CODE_MOBILE_EXIST       = 163; //手机账号已存在
  BAN_SANDBOX             = 164; //禁止沙盒测试
  WLC_ACCOUNT_NOT_EXIST   = 165; //账号不存在
  WLC_TRACE_ERROR         = 166; //wlc上报失败
  WLC_CHECK_ERROR         = 167; //wlc校验失败
  WLC_QUERY_ERROR         = 168; //wlc查询失败
  SIGN_ERROR              = 169; //签名错误
  S_GAME_ID_NOT_EXIST      = 170; //sgame_id不存在
  CODE_LOGIN_TYP_ERROR    = 171; //未开放对应登录方式
  CODE_PAY_TYP_ERROR      = 172; //未开放对应支付方式
  CODE_IS_NOT_ADULT_LIMIT = 173; //未成年人限制
  RECHARGE_SINGLE_LIMIT   = 174; //充值单笔限额
  RECHARGE_ACCUMULATE     = 175; //充值累计限额
  CODE_NOT_FOUND          = 404; //APPLE资源不存在
  CODE_SERVER_ERROR       = 500; //APPLE服务器错误
}
```

### <a id="sign">签名规则</a>
```go

游戏标识: 前端密匙

101: DC6DEE092984EFA3CE7519FE08BDBAA6 //影之刃测试密匙。正式服务器会有不同
```

请求消息中 SGameId 字段为上述中游戏标识

请求消息中的 Sign 字段，通过签名规则生成：

    签名规则为：MD5(游戏标识 + "-" + 前端密匙)