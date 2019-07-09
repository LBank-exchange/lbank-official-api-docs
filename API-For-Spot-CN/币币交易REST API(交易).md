 # 币币交易REST API(交易)

## 开始使用    

REST，即Representational State Transfer的缩写，是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，正得到越来越多网站的采用。其优点如下：    
> - 在 `RESTful` 架构中，每一个 `URL` 代表一种资源；    
> - 客户端和服务器之间，传递这种资源的某种表现层；    
> - 客户端通过四个 `HTTPS` 指令，对服务器端资源进行操作，实现“表现层状态转化”。   

建议开发者使用 `REST API` 进行币币交易或者资产提现等操作。    
    
## 请求交互    

`REST` 访问的根URL：`https://www.lbkex.net/` 或 `https://api.lbkex.com/`
 
所有请求基于 `Https` 协议，请求头信息中 `contentType` 需要统一设置为：`application/x-www-form-urlencoded`    
	
请求交互说明    
> 1. 请求参数：根据接口请求参数规定进行参数封装。    
> 2. 提交请求参数：将封装好的请求参数通过 `POST` 或 `GET` 方式提交至服务器。    
> 3. 服务器响应：服务器首先对用户请求数据进行参数安全校验，通过校验后根据业务逻辑将响应数据以 `JSON` 格式返回给用户。    
> 4. 数据处理：对服务器响应数据进行处理。 

## API参考  

### 币币交易 API 

1.获取用户账户资产信息 


请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|sign|String|是|请求参数的签名|

请求示例:	

```javascript
# RequestPOST https://www.lbkex.net/v1/user_info.do
{
  "api_key"："16702619-0bc*********0-62fb67a8985e",
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
|result | `true` 代表成功返回<br> `false` 代表返回失败 |
|asset |账户总资产 |
|freeze |账户冻结余额 |
|free |账户可用余额|


2.下单 

请求参数	


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|symbol|String|是|交易对<br>`eth_btc`:以太坊； `zec_btc`:零币 |
|type|String|是|委托买卖类型 `buy/sell`|
|price|String|参考描述|下单价格<br>买单和卖单：`大于等于0`|
|amount|String|参考描述 |交易数量 <br>卖单和卖单：`BTC` 数量大于等于 `0.001`|
|sign|String|是|请求参数的签名|

请求示例:	

```javascript
# Request
POST https://www.lbkex.net/v1/create_order.do
{
  "api_key"："16702619-0bc*********0-62fb67a8985e",
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


3.撤销订单 

请求参数	


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|symbol|String|是|交易对<br>`eth_btc`:以太坊； `zec_btc`:零币 |
|order_id|String|是|订单id<br>多个订单 `order_id1,order_id2`, 订单个数限制 `1-50` |
|sign|String|是|请求参数的签名|

请求示例:	

```javascript
# Request
POST https://www.lbkex.net/v1/cancel_order.do
{
  "api_key"："16702619-0bc*********0-62fb67a8985e",
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


4.查询订单 

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
POST https://www.lbkex.net/v1/orders_info.do
{
  "api_key"："16702619-0bc*********0-62fb67a8985e",
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
|symbol|交易对，`eth_btc`：以太坊， `zec_btc`：ZCash|
|amount|下单数量|
|create_time|委托时间|
|price|委托价格|
|avg_price|平均成交价|
|type|`buy`：买入<br>`sell`：卖出
|deal_amount|成交数量|
|status|委托状态<br>`-1`：已撤销 <br>`0`：未成交 <br>`1`： 部分成交<br> `2`：完全成交 <br>`4`：撤单处理中


5.查询订单历史（仅支持查最近两天内的历史订单）

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
POST https://www.lbkex.net/v1/orders_info_history.do
{
  "api_key"："16702619-0bc*********0-62fb67a8985e",
  "symbol"："eth_btc",
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
|symbol|交易对，`eth_btc`：以太坊， `zec_btc`：ZCash|
|amount|下单数量|
|create_time|委托时间|
|order_id|订单ID（用于单笔订单）| 
|orders|查询的订单集合| 
|price|委托价格|
|avg_price|平均成交价|
|type|`buy`：买入<br>`sell`：卖出<br>`buy_market`: 市价单买入（price参数表示要买的量，以基准币计算，不需要amount参数）<br>`sell_market`：市价单卖出（amount参数是卖出的量，以卖出的token计算，不需要price参数）|
|deal_amount|成交数量|
|status|委托状态<br>`-1`：已撤销 <br>`0`：未成交 <br>`1`： 部分成交<br> `2`：完全成交 <br>`4`：撤单处理中
|current_page|当前页码|
|page_length|每页数据条数|
|total|该查询状态的总记录数|


6.查询订单成交明细

请求参数	


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|sign|String|是|请求参数的签名|
|api_key|String|是|用户申请的 `api_key`|
|symbol|String|是|交易对<br>`eth_btc`:以太坊； `zec_btc`:零币 |
|order_id|String|是|订单ID|

请求示例:	

```javascript
# Request
POST https://www.lbkex.net/v1/order_transaction_detail.do
{
  "api_key"："16702619-0bc*********0-62fb67a8985e",
  "symbol"："eth_btc",
  "order_id"："24f7ce27-af1d-4dca-a8c1-ef1cbeec1b23",
  "sign"："16702619-0bc8-446d***********-a3d0-62fb67a8985e",
}
# Response
{
  "result"："true",
  "transaction"：[
    {
      "txUuid": "ae926ba8f6f44cae8d347a4b7ac90135",
      "orderUuid": "24f7ce27-af1d-4dca-a8c1-ef1cbeec1b23",
      "tradeType": "sell",
      "dealTime": 1562553793113,
      "dealPrice": 0.0200000000,
      "dealQuantity": 0.0001000000,
      "dealVolumePrice": 0.0000020000,
      "tradeFee": 0.0000010000,
      "tradeFeeRate": 0.000001
    }
  ]
}
```
返回值说明	


|字段|描述|
|-|-|
|result | `true` 成功<br>`false` 失败|
|txUuid|交易编号|
|orderUuid|订单ID|
|tradeType|`buy`：买入<br>`sell`：卖出|
|dealTime|成交时间| 
|dealPrice|成交价格| 
|dealQuantity|成交数量|
|dealVolumePrice|成交额|
|tradeFee|交易手续费|
|tradeFeeRate|交易手续费率|


7.历史成交明细

请求参数	


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|sign|String|是|请求参数的签名|
|api_key|String|是|用户申请的 `api_key`|
|symbol|String|是|交易对<br>`eth_btc`:以太坊； `zec_btc`:零币 |
|type|String|否|订单类型 sell, buy|
|start_date|String|否|开始时间 yyyy-mm-dd，最大值为今天，默认为昨天|
|end_date|String|否|结束时间，yyyy-mm-dd，最大值为今天，默认为今天<br>起始与结束时间的查询窗口最大为2天|
|from|String|否|查询的起始交易编号|
|direct|String|否|查询方向，默认next，成交时间正序，prev为成交时间倒序|
|size|String|否|查询条数，默认100，[1-100]|

请求示例:	

```javascript
# Request
POST https://www.lbkex.net/v1/transaction_history.do
{
  "api_key"："16702619-0bc*********0-62fb67a8985e",
  "symbol"："eth_btc",
  "sign"："16702619-0bc8-446d***********-a3d0-62fb67a8985e",
}
# Response
{
  "result"："true",
  "transaction"：[
    {
      "txUuid": "ae926ba8f6f44cae8d347a4b7ac90135",
      "orderUuid": "5976ed05-6141-4fea-bcd5-179fa7a1fa56",
      "tradeType": "sell",
      "dealTime": 1562553793113,
      "dealPrice": 0.0200000000,
      "dealQuantity": 0.0001000000,
      "dealVolumePrice": 0.0000020000,
      "tradeFee": 0.0000010000,
      "tradeFeeRate": 0.000001
    },
    {
      "txUuid": "ae926ba8f6f44cae8d347a4b7ac90135",
      "orderUuid": "d65d4302-a4ff-4578-aa6b-6717758fb8c2",
      "tradeType": "buy",
      "dealTime": 1562553793113,
      "dealPrice": 0.0200000000,
      "dealQuantity": 0.0001000000,
      "dealVolumePrice": 0.0000020000,
      "tradeFee": 0.0000010000,
      "tradeFeeRate": 0.000002
    },
    {
      "txUuid": "bebadd0f953747d88d1e3181bba36f12",
      "orderUuid": "e0465949-12c2-4b87-87fa-12847a324a09",
      "tradeType": "sell",
      "dealTime": 1562575780302,
      "dealPrice": 0.0200000000,
      "dealQuantity": 0.0001000000,
      "dealVolumePrice": 0.0000020000,
      "tradeFee": 0.0000010000,
      "tradeFeeRate": 0.000001
    }
  ]
}
```
返回值说明	


|字段|描述|
|-|-|
|result | `true` 成功<br>`false` 失败|
|txUuid|交易编号|
|orderUuid|订单ID|
|tradeType|`buy`：买入<br>`sell`：卖出|
|dealTime|成交时间| 
|dealPrice|成交价格| 
|dealQuantity|成交数量|
|dealVolumePrice|成交额|
|tradeFee|交易手续费|
|tradeFeeRate|交易手续费率|


8.获取所有币对的基本信息

请求参数：`无`


请求示例:	

```javascript
# Request
GET https://www.lbkex.net/v1/accuracy.do

# Response
[
  {
    "priceAccuracy": "2",
    "quantityAccuracy": "4",
    "symbol": "pch_usdt"
  }
]
```
返回值说明	


|字段|描述|
|-|-|
|priceAccuracy |价格精度|
|quantityAccuracy |数量精度|
|symbol|交易对|


9.获取用户开放订单（仅支持查最近两天内的开放订单）

请求参数：


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|symbol|String|是|交易对<br>`eth_btc`:以太坊； `zec_btc`:零币 |
|current_page|String|是|当前页码|
|page_length|String|是|每页数据条数（不得小于1,不得大于200）|
|sign|String|是|请求参数的签名|


请求示例:	

```javascript
# Request
POST https://www.lbkex.net/v1/orders_info_no_deal.do
{
  "api_key": "16702619-0bc*********0-62fb67a8985e",
  "symbol": "eth_btc",
  "current_page":"1",
  "page_length":"100",
  "sign":"iuhviuviuhviuerh92834fheifhw98f39r8g3rhf"
}

# Response
{
  'page_length': 100, 
  'current_page': 1, 
  'total': 1, 
  'result': true,
  'orders': [
    {
      'status': 0,
      'custom_id': None, 
      'order_id': 'a7de8dae-16a3-416b-8fc9-e3bb88e0d819', 
      'price': 0.015, 
      'amount': 100.0, 
      'create_time': 1527498699770, 
      'avg_price': 0.0, 
      'type': 'sell',
      'symbol': 'eth_btc', 
      'deal_amount': 0.0
    }
  ]
}
```
返回值说明	


|字段|描述|
|-|-|
|page_length |每页数据条数|
|current_page |当前页码|
|total|总页数|
|result | `true` 代表成功返回<br> `false` 代表返回失败 |
|status | `0` 代表未成交 <br> `1` 部分成交|
|custom_id | 用户发单请求时自定义字段|
|order_id | 订单号 |
|price | 价格 |
|amount | 数量 |
|create_time | 挂单时间 |
|avg_price | 平均成交价 |
|type | 订单类型<br>`sell` 卖单 <br> `buy` 买单 |
|symbol | 交易对 |
|deal_amount|成交数量|


10.美元对人民币的比例（每天0点更新一次）

请求参数:无
请求方式：GET

请求示例:

```javascript
# Request
GET https://www.lbkex.net/v1/usdToCny.do

# Response
{"USD2CNY":"6.4801"}
```
返回值说明:

|字段|描述|
|-|-|
|USD2CNY |美元对人民币的汇率|


11.币种提币参数接口


请求参数:


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|assetCode|String|否|币种编号|

请求方式：GET


请求示例:

```javascript
# Request
GET https://www.lbkex.net/v1/withdrawConfigs.do

# Response
[{'assetCode': 'eth', 'min': '0.01', 'canWithDraw': True, 'fee': '0.01'}]
```
返回值说明:

|字段|描述|
|-|-|
|assetCode |币种编号|
|min |最小提币数量|
|canWithDraw |该币种是否可提|
|fee |提币手续费（数量）|


12.提币接口 (需要绑定IP,可以在lbank网页端api提现页面申请)

请求参数:


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|sign|String|是|参数签名|
|account|String|是|提币地址|
|assetCode|String|是|提币币种|
|amount|String|是|提币数量（对于neo，必须是整数）|
|memo|String|否|对于bts、dct可能需要|
|mark|String|否|用户备注(长度小于255)|
|fee|String|否|提币手续费（单位：数量）|

请求方式：POST


请求示例:

```javascript
# Request
POST https://www.lbkex.net/v1/withdraw.do

# Response
{'result': 'true', 'withdrawId': 90082, 'fee':0.001}
```
返回值说明:

|字段|描述|
|-|-|
|result |true：成功，false：失败|
|withdrawId |当前提币记录编号|
|fee |提币手续费（数量）|


13.撤销提币接口 (需要绑定IP,可以在lbank网页端api提现页面申请)

请求参数:


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|sign|String|是|参数签名|
|withdrawId|String|是|提币记录编号|

请求方式：POST


请求示例:

```javascript
# Request
POST https://www.lbkex.net/v1/withdrawCancel.do

# Response
{'result': 'true', 'withdrawId': '90083'}
```
返回值说明:

|字段|描述|
|-|-|
|result |true：成功，false：失败|
|withdrawId |当前提币记录编号|


14.提币记录接口 (需要绑定IP,可以在lbank网页端api提现页面申请)

请求参数:


|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|sign|String|是|参数签名|
|assetCode|String|否|币种编号|
|status|String|是|提币状态（0：全部，1：申请中，2：已撤销，3：提现失败，4：提现完成）|
|pageNo|String|否|当前分页页码（默认：1）|
|pageSize|String|否|每页大小（默认：20，最大100条）|


请求方式：POST


请求示例:

```javascript
# Request
POST https://www.lbkex.net/v1/withdraws.do

# Response
{
	'totalPages': 1,
	'pageSize': 20,
	'pageNo': 1
	'list': [{
		'amount': 20.0,
		'assetCode': 'btc',
		'address': 'erfwergertghrehyrhethyryuj',
		'fee': 0.0,
		'id': 89686,
		'time': 1525750028000,
		'txHash': '',
		'status': '2'
	}, {
		'amount': 335.0,
		'assetCode': 'btc',
		'address': 'erfwergertghrehyrhethyryuj',
		'fee': 0.0,
		'id': 89687,
		'time': 1525767979000,
		'txHash': '',
		'status': '2'
	}
	....]
}
```
返回值说明:

|字段|描述|
|-|-|
|totalPage |总的页数|
|pageSize |每页大小|
|pageNo|当前页码|
|list |列表数据（id:提币记录编号，assetCode:币种编号，address: 提币地址，amount：提币数量，fee：提币手续费，time：提币时间，txHash：提币hash，status：提币状态）|



