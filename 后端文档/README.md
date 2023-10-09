# sbasedoc 中台接入文档--后端版本

## 接口文档

### 登录
[用户登录](#login)

[用户行为上传](#usertrace)

[防沉迷相关文档](#wlc)
### 支付
[支付下单](#transaction)

[支付发货](#sendgoods)

### 签名
[后端签名规则](#sign)

---
### <a id="login">用户登录</a>
EMLogin 为公司快捷登录 PFLogin 为平台登录，例如：微信，QQ.....
![登录流程图](../image/login.png)

暂时直接明文传送 LoginToken => SGameId，后续会改为加密传送
### <a id="usertrace">用户行为上传</a> 
路径：/WlcLoginTrace[README.md]

```
Method: POST
ContentType: application/json
```

```go
// 请求参数 LoginTraceParam 用户行为数据上报请求参数
type WLcLoginTraceReq struct {
    Collections []*WlcLoginTrace `json:"collections"`
    
    SGameId string `json:"s_game_id"` // 游戏标识
    ReqTime string `json:"req_time"`  // 请求时间(时间戳)
    Sign    string `json:"sign"`      // 签名 详见签名规则
}

type WlcLoginTrace struct {
    Uid int64      `json:"uid"` // 游戏用户唯一标识
    BT  wlc.BTType `json:"bt"`  // 游戏用户行为类型 0：下线 1：上线
}

//响应参数
WlcLoginTraceResp struct {
    Code int `json:"code"`
}
```
[签名规则](#sign)

[code中台常量定义](#code)

#### <a id="wlc">防沉迷相关文档</a>

[接口对接技术规范.pdf](../WLC/接口对接技术规范.pdf)

[网络游戏防沉迷实名认证系统.pdf](../WLC/网络游戏防沉迷实名认证系统.pdf)

## 支付
PF常用支付流程图
![支付流程图](../image/pay.png)

### <a id="transaction">支付下单</a>
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

    Uid     int64           `json:"uid"` // 玩家唯一标识
    Sign    string `json:"sign"`    // 签名 详见签名规则
    ReqTime string `json:"req_time"`    // 请求时间(时间戳)
}

//下单响应
type TransactionResponse struct {
    Code  int              `json:"code"`
    Type  TypeTransaction  `json:"type"`
    Param TransactionParam `json:"param"`
}

//掉起支付参数
type TransactionParam struct {
    WeChatAppPayParam        *wechat.AppPayParams `json:"wechat_app_pay_param"` // 微信支付参数
    AliOrderInfo             string               `json:"ali_order_info"`   // 支付宝支付参数
    AppleApplicationUsername string               `json:"apple_application_username"`   // 苹果支付参数
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

[code中台常量定义](#code)

### <a id="sendgoods">支付发货</a>

服务器实现回调

```go
//通知参数
type SendGoodsReq struct {
    OrderId string `json:"order_id"` // 游戏订单ID => TransactionRequest.GameOrderId
    Attach  string `json:"attach"`
	
    Sign     string `json:"sign"`   // 签名 详见签名规则
    ReqTime  string `json:"req_time"`// 签名时间(时间戳)
}

//响应参数
type SendGoodsResp struct {
    Code int `json:"code"`
}
```
[签名规则](#sign)

[code中台常量定义](#code)

## <a id="sign">后端签名规则</a>
```go
游戏标识: 后端密匙

101: A798138F62D303BFC816352120747AFC // 影之刃测试，正式服会有变动
```

请求消息中 SGameId 字段为上述中游戏标识

请求消息中的 Sign 字段：

    签名规则为：MD5(游戏标识 + "-" + 后端密匙 + "-" + ReqTime)

    ReqTime 为请求时间戳

### <a id="code">code中台常量定义</a>
```go
const (
    CODE_SUCCESS           = 200 //成功
    CODE_PARAM_MISS        = 116 //参数缺失
    CODE_SERVICE_BUSY      = 126 //服务器繁忙（内部逻辑错误）
    CODE_TOKEN_EXPIRED     = 128 //令牌已过期
    CODE_INVALID_NAMESPACE = 157 //无效的Namespace
    CODE_PASSWORD_ERROR    = 158 //密码错误
    CODE_ACCOUNT_NOT_EXIST = 159 //账号不存在
    CODE_SEND_SMS_TOO_FAST = 160 //发送短信过快
    CODE_VALIDATE_CODE_ERR = 161 //验证码错误
    CODE_PAYCALLBACK_ERROR = 162 //支付回调错误
    CODE_MOBILE_EXIST      = 163 //手机账号已存在
    BAN_SANDBOX            = 164 //禁止沙盒测试
    WLC_ACCOUNT_NOT_EXIST  = 165 //账号不存在
    WLC_TRACE_ERROR        = 166 //wlc上报失败
    WLC_CHECH_ERROR        = 166 //wlc校验失败
    WLC_QUERY_ERROR        = 167 //wlc查询失败
    SIGN_ERROR             = 168 //签名错误
    SGAME_ID_NOT_EXIST     = 169 //sgame_id不存在
    CODE_LOGIN_TYP_ERROR   = 170 //未开放对应登录方式
    CODE_PAY_TYP_ERROR     = 171 //未开放对应支付方式
    CODE_NOT_FOUND         = 404 //APPLE资源不存在
    CODE_SERVER_ERROR      = 500 //APPLE服务器错误
)
```