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
ContentType: application/json
```

请求参数
```go
type EmLoginRequest struct {
    Namespace int `json:"namespace"`    //预留，可不填
    
    // em参数
    E string `json:"e"`
    M string `json:"m"`
    
    SGameId string `json:"s_game_id"`   // 游戏标识
    
    Sign string `json:"sign"`   // 签名 详见签名规则
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
ContentType: application/json
```

请求参数
```go
type WechatLoginRequest struct {
    Namespace int `json:"namespace"`    //预留，可不填
    
    SGameId string `json:"s_game_id"`   // 游戏标识
    Sign    string `json:"sign"`    // 签名 详见签名规则
    
    Code string `json:"code"`   // 微信登录返回的code
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
ContentType: application/json
```

请求参数
```go
type TTLoginRequest struct {
    Namespace int `json:"namespace"` //预留，可不填

    SGameId string `json:"s_game_id"`   // 游戏标识
    Sign    string `json:"sign"`    // 签名 详见签名规则

    AccessToken string `json:"accessToken"` //登录成功后 TapSDK 返回的 access_token
    MacKey      string `json:"macKey"` //登录成功后 TapSDK 返回的 mac_key
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
ContentType: application/json
```
请求参数
```go
type QQLoginRequest struct {
    Namespace int `json:"namespace"` //预留，可不填

    SGameId string `json:"s_game_id"`   // 游戏标识
    Sign    string `json:"sign"`        // 签名 详见签名规则

    AccessToken string `json:"access_token"`    //登录成功后 QQSDK 返回的 access_token
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
```go
type YXLoginRequest struct {
    Namespace int `json:"namespace"` //预留，可不填

    SGameId string `json:"s_game_id"`   // 游戏标识
    Sign    string `json:"sign"`    // 签名 详见签名规则

    Account string `json:"account"` 
    Passwd  string `json:"passwd"`
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
ContentType: application/json
```

请求参数
```go
type ThirdLoginRequest struct {
    Namespace int `json:"namespace"` //预留，可不填

    SGameId string `json:"s_game_id"`   // 游戏标识
    Sign    string `json:"sign"`    // 签名 详见签名规则

    CUid string `json:"c_uid"`  //渠道用户ID
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
```go
type AppleLoginRequest struct {
    Namespace int `json:"namespace"` //预留，可不填 

    SGameId string `json:"s_game_id"`   // 游戏标识
    Sign    string `json:"sign"`      // 签名 详见签名规则

    Code string `json:"code"`   // 苹果登录返回的code
}

// 响应参数 见通用登录回复
```
[签名规则](#sign)

[通用登录回复](#loginResp)

### <a id="SendMobileMessage">发短信验证码</a> 
路径：/SendMobileMessage

```
Method: POST
ContentType: application/json
```

###### 请求参数
```go
type SendMobileMessageRequest struct {
    Namespace int    `json:"namespace"`
    Mobile    string `json:"mobile"` //手机号
    SendType  string `json:"send_type"` //发送类型 1绑定 2登录
}
```

### <a id="BindMobile">渠道用户绑定手机</a> 
路径：/BindMobile

```
Method: POST
ContentType: application/json
```

###### 请求参数
```go
type BindMobileRequest struct {
    Namespace    int    `json:"namespace"`
    Mobile       string `json:"mobile"` //手机号
    DeviceNo     string `json:"device_no"` //设备号
    ValidateCode string `json:"validate_code"` //验证码
    ChannelId    string `json:"channel_id"` //渠道ID
    CUid         string `json:"c_uid"` //渠道用户ID
}
```

[code中台常量定义](#code)

### <a id="MobileLogin">手机号登录</a> 
路径：/MobileLogin

```
Method: POST
ContentType: application/json
```

###### 请求参数
```go
type MobileLoginRequest struct {
    Namespace    int    `json:"namespace"`
    Mobile       string `json:"mobile"` //手机号
    DeviceNo     string `json:"device_no"` //设备号
    ValidateCode string `json:"validate_code"` //验证码
}
```

[通用登录回复](#loginResp)

### <a id="loginResp">通用登录回复</a>
```go
type LoginResult struct {
Code int `json:"code"`

    E string `json:"e"`
    M string `json:"m"`

    Uid         int64  `json:"uid"` //S-gameId
    DisplayName string `json:"display_name"` // YX Third 没有返回（待定）

    PFType string `json:"pf_type"`
    PFID   string `json:"pf_id"`

    Token string `json:"token"`
    Key   string `json:"key"`

    WlcStatus    int64 `json:"wlc_status"`  // 0 认证成功 1认证中 2 认证失败
    WlcIndulgeInfo WlcIndulgeInfo `json:"wlc_indulge_info"`
}

//WlcIndulgeInfo 防沉迷信息
WlcIndulgeInfo struct {
    IsAdult      bool  `json:"is_adult"`    // 是否成年
    UserBirthday int64 `json:"user_birthday"`   // 用户生日
    
    SingleRechargeMax     int64 `json:"single_recharge_max"`    // 单笔充值最大金额 -1 无限制
    AccumulateRechargeMax int64 `json:"accumulate_recharge_max"`    // 累计充值最大金额     -1 无限制  
}

```

### <a id="realcheck">实名认证</a>   
路径：/WlcCheck

```
Method: POST
ContentType: application/json
```

###### 参数
```go
// WlcCheckReq 实名认证请求参数
type WlcCheckReq struct {
    // em参数
    E string `json:"e"`
    M string `json:"m"`
	
    Name  string `json:"name"`  // 用户姓名
    IdNum string `json:"idNum"` // 用户身份证号码

    SGameId string `json:"s_game_id"`   // 游戏标识
    Sign    string `json:"sign"`    // 签名 详见签名规则
}

WlcCheckResp struct {
    Status  int  `json:"status"` // 0 认证成功 1认证中 2 认证失败
	wlcIndulgeInfo WlcIndulgeInfo `json:"wlc_indulge_info"`
	CanPlay bool `json:"can_play"`  // 是否可以游戏
}

//WlcIndulgeInfo 防沉迷信息
WlcIndulgeInfo struct {
    IsAdult      bool  `json:"is_adult"`    // 是否成年
    UserBirthday int64 `json:"user_birthday"`   // 用户生日
    
    SingleRechargeMax     int64 `json:"single_recharge_max"`    // 单笔充值最大金额 -1 无限制
    AccumulateRechargeMax int64 `json:"accumulate_recharge_max"`    // 累计充值最大金额     -1 无限制  
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

```go

//支付类型
const (
    TA_WECHAT_APP    TypeTransaction = "WX_APP"    // 微信APP
    TA_WECHAT_NATIVE TypeTransaction = "WX_NATIVE" // 微信NATIVE
    TA_ALI           TypeTransaction = "ALI"       // 支付宝
    TA_APPLE         TypeTransaction = "AP"        // 苹果
)

type TypeTransaction string

//货币信息
type Amount struct {
    Total    float64 `json:"total"`   // 总金额
    Currency string  `json:"currency"`  // 货币类型 可不填，不填情况下默认为 CNY：人民币
}

//下单请求
type TransactionRequest struct {
    GameOrderId   string          `json:"game_order_id"`  // 游戏订单ID
    Desc          string          `json:"description"`      // 订单描述
    Amount        Amount          `json:"amount"`        // 货币信息
    GameNotifyUrl string          `json:"game_notify_url"`  // 游戏回调地址
    Attach        string          `json:"attach"`     // 附加信息 服务器透传，回调时原样返回
    SGameId       string          `json:"s_game_id"`    // 游戏标识
    Type          TypeTransaction `json:"type"`       // 支付类型
    IsSandbox     bool            `json:"is_sandbox"`   // 是否沙盒测试 仅苹果支付有效 默认为false

    // em参数
    E string `json:"e"`
    M string `json:"m"`
	
    Sign    string `json:"sign"`    // 签名 详见签名规则
    ReqTime string `json:"req_time"`    // 请求时间(时间戳)
}

//下单响应
type TransactionResponse struct {
    Code  int              `json:"code"`
    Type  TypeTransaction  `json:"type"`
    Param TransactionParam `json:"param"`   // 支付参数
}

type TransactionParam struct {
    WeChatAppPayParam        *wechat.AppPayParams `json:"wechat_app_pay_param"` // 微信APP支付参数
    WeChatNativePay          string               `json:"wechat_native_pay"`    // 微信NATIVE支付参数
    AliAppOrderInfo          string               `json:"ali_app_order_info"`   // 支付宝APP支付参数
    AliWebOrderInfo          string               `json:"ali_web_order_info"`   // 支付宝WEB支付参数
    AppleApplicationUsername string               `json:"apple_application_username"`   // 苹果支付applicationUsername 即为游戏订单ID
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
```go
// 苹果支付回调
type ApplePayVerifyIdTokenV1 struct {
    GameId  string `json:"game_id"`
    SgameId string `json:"sgame_id"`
    Receipt string `json:"receipt"`
}

type ApplePayVerifyIdTokenV1Rsp struct {
    Code int `json:"code"` //参考 code中台常量定义 
    SuccessTransactionList []string `json:"success_transaction_list"`   // 成功的transaction_id
}
```
ApplePayVerifyIdTokenV1Rsp 参数 SuccessTransactionList 为成功的[transaction_id](https://developer.apple.com/documentation/appstorereceipts/responsebody/latest_receipt_info)

[code中台常量定义](#code)

### <a id="code">code中台常量定义</a>
```go
const (
    CODE_SUCCESS            = 200 //成功
    CODE_PARAM_MISS         = 116 //参数缺失
    CODE_SERVICE_BUSY       = 126 //服务器繁忙（内部逻辑错误）
    CODE_TOKEN_EXPIRED      = 128 //令牌已过期
    CODE_INVALID_NAMESPACE  = 157 //无效的Namespace
    CODE_PASSWORD_ERROR     = 158 //密码错误
    CODE_ACCOUNT_NOT_EXIST  = 159 //账号不存在
    CODE_SEND_SMS_TOO_FAST  = 160 //发送短信过快
    CODE_VALIDATE_CODE_ERR  = 161 //验证码错误
    CODE_PAYCALLBACK_ERROR  = 162 //支付回调错误
    CODE_MOBILE_EXIST       = 163 //手机账号已存在
    BAN_SANDBOX             = 164 //禁止沙盒测试
    WLC_ACCOUNT_NOT_EXIST   = 165 //账号不存在
    WLC_TRACE_ERROR         = 166 //wlc上报失败
    WLC_CHECH_ERROR         = 166 //wlc校验失败
    WLC_QUERY_ERROR         = 167 //wlc查询失败
    SIGN_ERROR              = 168 //签名错误
    SGAME_ID_NOT_EXIST      = 169 //sgame_id不存在
    CODE_LOGIN_TYP_ERROR    = 170 //未开放对应登录方式
    CODE_PAY_TYP_ERROR      = 171 //未开放对应支付方式
    CODE_IS_NOT_ADULT_LIMIT = 172 //未成年人限制
    CODE_NOT_FOUND          = 404 //APPLE资源不存在
    CODE_SERVER_ERROR       = 500 //APPLE服务器错误
    RECHARGE_SINGLE_LIMIT   = 173 //充值单笔限额
    RECHARGE_ACCUMULATE     = 174 //充值累计限额
)
```

### <a id="sign">签名规则</a>
```go

游戏标识: 前端密匙

101: DC6DEE092984EFA3CE7519FE08BDBAA6 //影之刃测试密匙。正式服务器会有不同
```

请求消息中 SGameId 字段为上述中游戏标识

请求消息中的 Sign 字段，通过签名规则生成：

    签名规则为：MD5(游戏标识 + "-" + 前端密匙)