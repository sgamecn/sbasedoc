# sbasedoc 中台接入文档--后端版本

## 接口文档

### 登录
[用户登录](#login)

[用户行为上传](#usertrace)

[防沉迷相关文档](#wlc)
### 支付
[支付下单](#paytransaction)

[支付发货](#sendgoods)

### 签名
[后端签名规则](#sign)

### 防沉迷
[防沉迷详细规则](#chenmi)

---
### <a id="login">用户登录</a>
EMLogin 为公司快捷登录 PFLogin 为平台登录，例如：微信，QQ.....
![登录流程图](../image/login.png)

暂时直接明文传送 LoginToken => SGameId，后续会改为加密传送
### <a id="usertrace">用户行为上传</a> 
路径：/WlcLoginTrace[README.md]

```
Method: POST
```

```protobuf
syntax = "proto3";

// 请求参数 LoginTraceParam 用户行为数据上报请求参数
message WLcLoginTraceReq {
  repeated WlcLoginTrace Collections = 1; // 用户行为数据

  string SGameId = 2; // 游戏标识
  string ReqTime = 3; // 请求时间戳
  string Sign = 4;  // 签名 详见签名规则
}

enum BTType {
  BTTypeLogout = 0; // 下线
  BTTypeLogin = 1; // 上线
}

message WlcLoginTrace {
  string Uid = 1; // 游戏用户ID (特殊处理专门用于上传的ID，由前端或者良哥回传)
  BTType BT = 2;  // 行为类型
}

//Wlc上报登录轨迹回复 SBase->Client
message WLcLoginTraceResp {
  CODE Code = 1;  // 错误码  详见code中台常量定义
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

Apple支付流程图
![支付流程图](../image/ApplePay.png)

### <a id="paytransaction">支付下单</a>

2步骤中的参数由服务器生成，前端进行透传

```protobuf
syntax = "proto3";

//下单请求具体参数
message TransactionRequest {

  // 游戏ID，由中台分配给游戏服使用（例如影三为 101）
  string SGameId = 1;

  // 产品id（充值档位id），由中台分配给游戏服使用（如com.sgame.yzr3.648）
  string ProductID = 2;

  // 下单时间
  string ReqTime = 3;

  // 游戏回调地址
  string GameNotifyUrl = 4;

  // 透传参数
  string Attach = 5;

  // SDK下单信息(客户端拉起页面展示的订单信息)
  // TA_WECHAT_APP = 0;   // 微信APP
  // TA_WECHAT_NATIVE = 1; // 微信NATIVE
  // TA_ALI = 2;     // 支付宝
  // TA_APPLE = 3;        // 苹果
  int32 ClientSdkType = 11;
  string ClientSdkDesc = 12;

  //可选参数
  string CpOrderId = 21; // 游戏订单ID
  string CpUserId = 22; // 用户ID
  string CpServerId = 23; // 服务器ID
  string CpRoleId = 24; // 角色ID
  string CpRoleName = 25; // 角色名
  string CpItemId = 26; // 游戏物品id
  string CpItemName = 27; // 游戏物品名
}
```
具体数据由后端数据加工后，发送给前端，前端转发SBase进行下单

具体数据加工方式为：

base64加密(proto.Marshal(TransactionRequest))之后为 下单信息

最终透传字符串为：md5加密(下单信息+后端密匙)+下单信息 

### <a id="sendgoods">支付发货</a>

发货服务器实现对应接口

```protobuf
syntax = "proto3";

//发货
message SendGoodsReq {

  string OrderId = 1; // 订单ID
  string TransactionId = 2; // 交易ID
  //  string Attach = 2;  // 透传参数

  string Sign = 3;  // 签名
  string ReqTime = 4; // 请求时间

  TransactionRequest transaction = 11; // 下单数据
}

message SendGoodsResp {
  int32 Code = 1;
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
```protobuf
syntax = "proto3";

enum CODE {
  DEFAULT 			           = 0;
  SUCCESS                 = 200; //成功
  SERVICE_BUSY            = 110; //服务器繁忙（内部逻辑错误）
  PARAM_MISS              = 111; //参数缺失
  SIGN_ERROR              = 112; //签名错误
  ACCOUNT_NOT_EXIST       = 113; //账号不存在
  EM_DECODER_ERROR        = 114; //em解码错误
  //ACCOUNT_INFO_NOT_EXIST  = 115; //账号Info不存在

  IS_NOT_ADULT_LIMIT      = 116; //未成年人限制

  LOGIN_TYP_NOT_OPEN      = 130; //未开放对应登录方式
  LOGIN_VERIFY_FAIL       = 132; //登录验证失败
  LOGIN_TOKEN_EXPIRED     = 133; //令牌已过期
  LOGIN_QUERY_WLC_ERROR   = 134; //登录查询wlc失败
  LOGIN_EM_BAN            = 135; //登录em登录方式禁止 收到此错误码后，客户端应该删除本地的em登录方式

  //Mobile_DEVICE_NO_FORBID = 150; //设备号禁止
  //IP_FORBID               = 151; //IP禁止

  SEND_SMS_TOO_FAST       = 160; //发送短信过快
  VALIDATE_CODE_ERR       = 161; //验证码错误
  PAY_CALLBACK_ERROR      = 162; //支付回调错误
  MOBILE_EXIST            = 163; //手机账号已存在
  SEND_BUSINESS_LIMIT     = 164; //业务限流

  WLC_CHECK_ERROR         = 180; //wlc校验失败
  WLC_TRACE_ERROR         = 181; //wlc上报失败
  WLC_QUERY_ERROR         = 182; //wlc查询失败

  S_GAME_ID_NOT_EXIST     = 170; //s_game_id不存在
  ID_NUM_ERROR            = 176; //身份证号码错误

  PAY_TYP_NOT_OPEN              = 300; //未开放对应支付方式
  PAY_TRANSACTION_PARAM_ERROR   = 301; //下单失败
  PAY_RECHARGE_SINGLE_LIMIT     = 302; //充值单笔限额
  PAY_RECHARGE_ACCUMULATE_LIMIT = 303; //充值累计限额
  PAY_BAN_SANDBOX               = 304; //禁止沙盒测试

  DELETE_GAME_USER_NOT_EXIST = 400; //删除账号不存在
}
```

###  <a id="chenmi">防沉迷详细规则</a>

a.游戏时长限制，未成年用户仅可在周五、周六、周日和法定节假日的20时至21时进行游戏，并在21点整点强制下线；

b.游戏消费限制，未满12周岁的用户无法进行游戏充值；12周岁（含）以上未满16周岁的用户，单次充值上限50元人民币，每月充值上限200元人民币；16周岁（含）以上未成年用户，单次充值上限100元人民币，每月充值上限400元人民币。

中台登录时会进行判定判定时候可以登录 9点强制踢 前端自行实现 