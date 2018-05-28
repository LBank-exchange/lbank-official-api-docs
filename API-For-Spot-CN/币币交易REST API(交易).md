 # 币币交易REST API(交易)

## 开始使用    

REST，即Representational State Transfer的缩写，是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，正得到越来越多网站的采用。其优点如下：    
> - 在 `RESTful` 架构中，每一个 `URL` 代表一种资源；    
> - 客户端和服务器之间，传递这种资源的某种表现层；    
> - 客户端通过四个 `HTTP` 指令，对服务器端资源进行操作，实现“表现层状态转化”。   

建议开发者使用 `REST API` 进行币币交易或者资产提现等操作。    
    
## 请求交互    

`REST` 访问的根URL：`http://api.lbank.info/`
  
访问时需要科学上网

所有请求基于 `Http` 协议，请求头信息中 `contentType` 需要统一设置为：`application/x-www-form-urlencoded`    
	
请求交互说明    
> 1. 请求参数：根据接口请求参数规定进行参数封装。    
> 2. 提交请求参数：将封装好的请求参数通过 `POST` 或 `GET` 方式提交至服务器。    
> 3. 服务器响应：服务器首先对用户请求数据进行参数安全校验，通过校验后根据业务逻辑将响应数据以 `JSON` 格式返回给用户。    
> 4. 数据处理：对服务器响应数据进行处理。 

## API参考  

### 币币交易 API 

1.获取用户账户资产信息 

> URL: `http://api.lbank.info/v1/user_info.do`

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|sign|String|是|请求参数的签名|

请求示例:	

```javascript
# RequestPOST http://api.lbank.info/v1/user_info.do
{
  "api_key"："16702619-0bc8-446d-a3d0-62fb67a8985e",
  "sign"："0E0872AD955C0E715B43C78F24B3053A",
}
# Response
{
  "result"："true",
  "info":{
    "freeze"：{
    "btc"：1.0000,
    "zec"：0.0000,
    "cny"：80000.00
  },
  "asset"：{
    "net"：95678.25
  },
  "free"：{
    "btc"：2.0000,
    "zec"：0.0000,
    "cny"：34.00
  }
}
```

返回值说明	
|字段|描述|
|-|-|
|result | `true` 代表成功返回 |
|false |代表返回失败 |
|asset |账户总资产 |
|freeze |账户冻结余额 |
|free |账户可用余额|

2.下单 

请求参数	


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|symbol|String|是|交易对<br>`eth_btc`:以太坊； `zec_btc`:零币 |
|type|String|是|委托买卖类型<br>限价单：`buy/sell`<br>市价单：`buy_market/sell_market` |
|price|String|参考描述|下单价格<br>限价买单和卖单：`大于等于0`<br>市价买单：`BTC`交易最少买入 `0.001` 个BTC 的金额 (金额>0.01*卖一价)<br>市价卖单不传此参数且无需加密 `sign` |
|amount|String|参考描述 |交易数量 <br>限价卖单和卖单：`BTC` 数量大于等于 `0.001`<br>市价买单不传且需加密`sign`|
|sign|String|是|请求参数的签名|

请求示例:	

```javascript
# Request
POST http://api.lbank.info/v1/create_order.do
{
  "api_key"："16702619-0bc8-446d-a3d0-62fb67a8985e",
  "symbol"："eth_btc",
  "type"："buy",
  "price"："5323.42",
  "amount"："3",
  "sign"："16702619-0bc8-446d-a3d0-62fb67a8985e",
}
# Response
{
  "result"："true",
  "order_id"："16702619-0bc8-446d"
}
```

返回值说明	


|字段|描述|
|-|-|
|result|`true` 成功<br>`false` 失败 |
|order_id|订单ID| 

2.撤销订单 

请求参数	


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|symbol|String|是|交易对<br>`eth_btc`:以太坊； `zec_btc`:零币 |
|order_id|String|是|订单id<br>多个订单 `order_id1,order_id2`, 订单个数限制 `1-3` |
|sign|String|是|请求参数的签名|

请求示例:	

```javascript
# Request
POST http://api.lbank.info/v1/cancel_order.do
{
  "api_key"："16702619-0bc8-446d-a3d0-62fb67a8985e",
  "symbol"："eth_btc",
  "order_id"："24f7ce27-af1d-4dca-a8c1-ef1cbeec1b23",
  "sign"："16702619-0bc8-446d-a3d0-62fb67a8985e",
}
# Response
{
  "result"："true",
  "order_id"："*****",
  "success"："*****,*****,*****",
  "error"："*****,*****"
}
```

返回值说明	


|字段|描述|
|-|-|
|result | `true` 撤单成功，等待系统执行撤单<br>`false` 撤单失败（用于单笔订单） |
|order_id|订单ID（用于单笔订单）| 
|success|撤单请求成功的订单ID，等待系统执行撤单（用于多笔订单）| 
|error|撤单请求失败的订单ID（用于多笔订单）

3.查询订单 

请求参数	


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|symbol|String|是|交易对<br>`eth_btc`:以太坊； `zec_btc`:零币 |
|order_id|String|是|订单id<br>多个订单 `order_id1,order_id2`, 订单个数限制 `1-3` |
|sign|String|是|请求参数的签名|

请求示例:	

```javascript
# Request
POST http://api.lbank.info/v1/orders_info.do
{
  "api_key"："16702619-0bc8-446d-a3d0-62fb67a8985e",
  "symbol"："eth_btc",
  "order_id"："24f7ce27-af1d-4dca-a8c1-ef1cbeec1b23,57a80854-cf31-489a-93b3-d25a2d4c12f2",
  "sign"："16702619-0bc8-446d-a3d0-62fb67a8985e",
}
# Response
{
  "result"："true",
  "orders"：[
    {
      "symbol"："eth_btc",
      "amount"：10.000000,
      "create_time"：1484289832081,
      "price"：5000.000000,
      "avg_price"：5277.301200,
      "type"："sell",
      "order_id"："ab704110-af0d-48fd-a083-c218f19a4a55",
      "deal_amount"：10.000000,
      "status"：2
    },
    {
      "symbol"："eth_btc",
      "amount"：10.000000,
      "create_time"：1484287197233,
      "price"：6000.000000,
      "avg_price"：0.000000,
      "type"："sell",
      "order_id"："57a80854-cf31-489a-93b3-d25a2d4c12f2",
      "deal_amount"：0.000000,
      "status"：-1
    }
  ]
}
```
返回值说明	


|字段|描述|
|-|-|
|result | `true` 成功<br>`false` 失败|
|order_id|订单ID（用于单笔订单）| 
|orders|查询的订单集合| 
|symbol|交易对，`eth_btc`：以太坊， `zec_btc`：ZCash 
|amount|限价单：下单数量 <br>市价单：市价单的btc数量
|create_time|委托时间|
|price|限价单：委托价格 <br> 市价单：市价单的cny金额|
|avg_price|平均成交价|
|type|`buy`：限价买入<br>`sell`：限价卖出<br>`buy_market`：市价买入 <br>`sell_market`：市价卖出
|deal_amount|成交数量|
|status|委托状态<br>`-1`：已撤销 <br>`0`：未成交 <br>`1`： 部分成交<br> `2`：完全成交 <br>`4`：撤单处理中

3.查询订单历史 

请求参数	


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|sign|String|是|请求参数的签名|
|api_key|String|是|用户申请的 `api_key`|
|symbol|String|是|交易对<br>`eth_btc`:以太坊； `zec_btc`:零币 |
|current_page|String|是|当前页码|
|page_length|String|是|每页数据条数（不得小于1,不得大于200）|

请求示例:	

```javascript
# Request
POST http://api.lbank.info/v1/orders_info_history.do
{
  "api_key"："16702619-0bc8-446d-a3d0-62fb67a8985e",
  "symbol"："eth_btc",
  "status"："0",
  "current_page"："1",
  "page_length"："100",
  "sign"："16702619-0bc8-446d-a3d0-62fb67a8985e",
}
# Response
{
  "result"："true",
  "total"："1",
  "page_length"："20",
  "orders"：[
    {
      "symbol"："eth_btc",
      "amount"：5.0000,
      "create_time"：1484716198613,
      "price"：6666.0000,
      "avg_price"：0.0000,
      "type"："sell",
      "order_id"："c3f9c478-5b06-4e1e-9df9-9f84f57a446c",
      "deal_amount"：0.0000,
      "status"：0
    },
    {
      "symbol"："eth_btc",
      "amount"：10.0000,
      "create_time"：1484716151390,
      "price"：6000.0000,
      "avg_price"：6136.3895,
      "type"："sell",
      "order_id"："9ead39f5-701a-400b-b635-d7349eb0f6b4",
      "deal_amount"：10.0000,
      "status"：2
    }
  ],
  "current_page"："1",
}
```
返回值说明	


|字段|描述|
|-|-|
|result | `true` 成功<br>`false` 失败|
|symbol|交易对，`eth_btc`：以太坊， `zec_btc`：ZCash 
|amount|限价单：下单数量 <br>市价单：市价单的btc数量
|create_time|委托时间|
|order_id|订单ID（用于单笔订单）| 
|orders|查询的订单集合| 
|price|限价单：委托价格 <br> 市价单：市价单的cny金额|
|avg_price|平均成交价|
|type|`buy`：限价买入<br>`sell`：限价卖出<br>`buy_market`：市价买入 <br>`sell_market`：市价卖出
|deal_amount|成交数量|
|status|委托状态<br>`-1`：已撤销 <br>`0`：未成交 <br>`1`： 部分成交<br> `2`：完全成交 <br>`4`：撤单处理中
|current_page|当前页码|
|page_length|每页数据条数|
|total|该查询状态的总记录数|
