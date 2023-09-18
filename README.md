# sbasedoc

### 接口文档

接口文档中的地址统一通过Tag配置来获取

### 通用登录回复(前端)

```go
type LoginResult struct {
Code int `json:"code"`

    // em参数
    E string `json:"e"`
    M string `json:"m"`

    Uid         int64  `json:"uid"` //S-gameId
    DisplayName string `json:"display_name"` // YX Third 没有返回（待定）

    PFType string `json:"pf_type"`
    PFID   string `json:"pf_id"`

    Token string `json:"token"`
    Key   string `json:"key"`

    IsWlc bool `json:"is_wlc"` //是否实名认证 如果没有实名认证 需要跳转到实名认证页面
}

code中台常量定义
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
    CODE_NOT_FOUND         = 404 //APPLE资源不存在
    CODE_SERVER_ERROR      = 500 //APPLE服务器错误
)
```

#### 实名认证 /WlcCheck

```
Method: POST
ContentType: application/json
```

###### 参数
```go
// CheckParam 实名认证请求参数
type CheckParam struct {
	AI    string `json:"ai"`    // 长度 32，游戏内部成员标识
	Name  string `json:"name"`  // 用户姓名
	IdNum string `json:"idNum"` // 用户身份证号码
}

type CheckResult struct {
    Code int `json:"code"`
}
```

#### 用户行为上传 /WlcLoginTrace（服务器）

```
Method: POST
ContentType: application/json
```

###### 参数
```go
// LoginTraceParam 用户行为数据上报请求参数
type LoginTraceParam struct {
    Collections []*LoginTrace `json:"collections"`
}

type LoginTrace struct {
    No uint8  `json:"no"` // 在批量模式中标识一条行为数据，取值范围 1-128
    SI string `json:"si"` // 长度 32， 一个会话标识只能对应唯一的实名用户，一个实名用户可以拥有多个会话标识；同一用户单次游戏会话中，上下线动作必须使用同一会话标识上报 备注：会话标识仅标识一次用户会话，生命周期仅为一次上线和与之匹配的一次下线，不会对生命周期之外的任何业务有任何影响
    BT BTType `json:"bt"` // 游戏用户行为类型 0：下线 1：上线
    OT int64  `json:"ot"` // 行为发生时间戳，单位秒
    CT CTType `json:"ct"` // 用户行为数据上报类型 0：已认证通过用户 2：游客用户
    DI string `json:"di"` // 长度 32，游客模式设备标识，由游戏运营单位生成，游客用户下必填
    PI string `json:"pi"` // 长度 38，已通过实名认证用户的唯一标识，已认证通过用户必填
}

```

#### EM登录 /EMLogin （前端）

```
Method: POST
ContentType: application/json
```

###### 参数
```go
type EmLoginRequest struct {
	// em参数
	E string `json:"e"`
	M string `json:"m"`
    }
```

#### 微信登录 /WechatLogin （前端）
[微信官方接入文档](https://developers.weixin.qq.com/doc/oplatform/Mobile_App/WeChat_Login/Development_Guide.html)

```
Method: POST
ContentType: application/json
```

###### 参数
```go
type WechatLoginRequest struct {
	// 微信登录参数
	Namespace int `json:"namespace"` //预留，可不填
	Code string `json:"code"` //授权临时票据（code）
}
```

#### TapTap登录 /TapTapLogin （前端）
[TapTap官方接入文档](https://developer.taptap.cn/docs/sdk/taptap-login/guide/start/)

```
Method: POST
ContentType: application/json
```

###### 参数
```go
type TTLoginRequest struct {
    Namespace int `json:"namespace"` //预留，可不填

    AccessToken string `json:"accessToken"` //登录成功后 TapSDK 返回的 access_token
    MacKey      string `json:"macKey"` //登录成功后 TapSDK 返回的 mac_key
}
```
#### QQ登录 （没有新用户代码）/QQLogin （前端）
[QQ官方接入文档](https://wiki.connect.qq.com/qq%e7%99%bb%e5%bd%95)
```
Method: POST
ContentType: application/json
```

###### 参数
```go
type QQLoginRequest struct {
    Namespace int `json:"namespace"` //预留，可不填

	AccessToken string `json:"access_token"` 
}
```

#### 英雄登录 （没有新用户代码）/YXLogin （前端）

```
Method: POST
ContentType: application/json
```

###### 参数
```go
type YXLoginRequest struct {
    Namespace int `json:"namespace"` //预留，可不填

    Account string `json:"account"` 
    Passwd  string `json:"passwd"`
}
```

#### 第三方登录 （没有新用户代码） /ThirdLogin （前端）

```
Method: POST
ContentType: application/json
```

###### 参数
```go
type ThirdLoginRequest struct {
    Namespace int `json:"namespace"` //预留，可不填

    Cuid string `json:"cuid"`
}
```

#### APPLE登录 /AppleLogin （前端）

```
Method: POST
ContentType: application/json
```

###### 参数
```go
type AppleLoginRequest struct {
    Namespace int `json:"namespace"` //预留，可不填

	Code string `json:"code"`
}
```

## 支付
支付通用
```go
type Amount struct {
    Total    float64 `json:"total"`
    Currency string  `json:"currency"`
}
```

### 微信支付
#### 微信下单 /WechatTransaction (前后均可)

```go
type WeChatTransactionRequest struct {
	GameOrderId   string `json:"game_order_id"` //游戏订单号
	Desc          string `json:"description"`   //商品描述
	Amount        Amount `json:"amount"`    //金额
	GameNotifyUrl string `json:"game_notify_url"`  //游戏回调地址
	Attach        string `json:"attach"`    //透传数据
	SgameId       string `json:"sgame_id"`  //SgameId  玩家游戏ID
	}
// 回复结构
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
[Wechat官方APP调起支付API](https://pay.weixin.qq.com/wiki/doc/apiv3/apis/chapter3_2_4.shtml)

AppPayParams 为微信下单后返回的参数，直接传给客户端调起微信支付

#### 支付宝下单 /AliTransaction (前后均可)

```go
type AliTransactionRequest struct {
    GameOrderId   string `json:"game_order_id"` //游戏订单号
    Subject       string `json:"subject"`      //商品标题
    Amount        Amount `json:"amount"`    //金额
    Attach        string `json:"attach"`    //透传数据
    GameNotifyUrl string `json:"game_notify_url"` //游戏回调地址
    SgameId       string `json:"sgame_id"`  //SgameId  玩家游戏ID
    }
}
// 回复结构
AliTransactionResponse struct {
    OrderInfo string `json:"order_info"`
}

OrderInfo JSON 格式
type TradePay struct {
    ErrorResponse
    TradeNo             string           `json:"trade_no,omitempty"`
    OutTradeNo          string           `json:"out_trade_no,omitempty"`
    BuyerLogonId        string           `json:"buyer_logon_id,omitempty"`
    TotalAmount         string           `json:"total_amount,omitempty"`
    ReceiptAmount       string           `json:"receipt_amount,omitempty"`
    BuyerPayAmount      string           `json:"buyer_pay_amount,omitempty"`
    PointAmount         string           `json:"point_amount,omitempty"`
    InvoiceAmount       string           `json:"invoice_amount,omitempty"`
    FundBillList        []*TradeFundBill `json:"fund_bill_list"`
    StoreName           string           `json:"store_name,omitempty"`
    BuyerUserId         string           `json:"buyer_user_id,omitempty"`
    DiscountGoodsDetail string           `json:"discount_goods_detail,omitempty"`
    AsyncPaymentMode    string           `json:"async_payment_mode,omitempty"`
    VoucherDetailList   []*VoucherDetail `json:"voucher_detail_list"`
    AdvanceAmount       string           `json:"advance_amount,omitempty"`
    AuthTradePayMode    string           `json:"auth_trade_pay_mode,omitempty"`
    MdiscountAmount     string           `json:"mdiscount_amount,omitempty"`
    DiscountAmount      string           `json:"discount_amount,omitempty"`
    CreditPayMode       string           `json:"credit_pay_mode"`
    CreditBizOrderId    string           `json:"credit_biz_order_id"`
}
```
[支付宝官方APP调起支付API](https://opendocs.alipay.com/open/02e7gq)

OrderInfo 为支付宝下单后返回的参数，直接传给客户端调起支付宝支付

#### 苹果支付 /AppleTransaction (前后均可)

```go
// 苹果下单
type AppleTransactionRequest struct {
	GameOrderId   string `json:"game_order_id"`     //游戏订单号
	Amount        Amount `json:"amount"`        //金额
	ProductID     string `json:"product_id"`    //商品ID APPLE 定义的商品ID
	Attach        string `json:"attach"`    //透传数据
	GameNotifyUrl string `json:"game_notify_url"`   //游戏回调地址
	SgameId       string `json:"sgame_id"`  //SgameId  玩家游戏ID
	GameId        string `json:"game_id"` //游戏ID 预留 可不填
	IsSandBox     bool   `json:"is_sand_box"`       //是否沙盒测试
    }
}
```

#### 苹果支付回调 /AppleVerifyIdTokenV1 (前端)
```go
// 苹果支付回调
type ApplePayVerifyIdTokenV1 struct {
    GameId  string `json:"game_id"`
    SgameId string `json:"sgame_id"`
    Receipt string `json:"receipt"`
}
```

#### 发货 (后端)
```go
type SendGoodsReq struct {
	OrderId string `json:"order_id"`
	Attach  string `json:"attach"`
	Token   string `json:"token"`
}

SendGoodsResp struct {
	Code int `json:"code"`
}
```