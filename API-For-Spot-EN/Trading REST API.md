# Trading REST API

## Getting Start

REST (Representational State Transfer) is the most popular 
internet software architecture nowadays. It is clear in 
structure, easy to understand, and convenient to expand. 
More and more companies therefore apply the structures 
in their websites. It has following advantages:

>- In a `RESTful` architecture, each `URL` represents a resource
>- Between the client and the server, a certain presentation layer of such resources passed
>- The client uses the four `HTTP` commands to operate on the server-side resources to achieve “state transition of the presentation layer.”

We suggested that user should use the REST API for trading and/or asset operations (such as deposite and withdrawals)

## Requesting Interaction
The base `URL` of REST requests is `https://api.lbkex.com/`

All requests are based on `HTTPS` protocol, and `contentType` in request header should be 
assigned to: `application/x-www-form-urlencoded`

The interaction requests
1.Request parameter: Proceed parameter encapsulation based on the interface request parameter. 
2.Submitting the request parameter: Through `POST` or `GET` method, the encapsulated requested parameter would be submitted to the server. 
3.Server response: Server executes security authentication to the user’s request data, and responded data would be returned to the user, according to the business logic. in JSON format after the authentication.
4.Data processing: process the data as it responds to the server. 


## API Reference

### Trading API

#### 1. Acquiring Users' Asset Information


Parameters	

| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Yes|User's `api_key`|
|sign|String|Yes|signature of the request|

Example:

```javascript
# RequestPOST https://api.lbkex.com/v1/user_info.do
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

Returns

|Field|Note|
|-|-|
|result | `true` means success and `false` means failed. |
|asset |Total balance of the asset |
|freeze |Frozen balance of the asset |
|free |Available balance of the account|


#### 2. Place Order

Parameters:


| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Yes|User's `api_key`|
|symbol|String|Yes|Trading Pair<br>`eth_btc`:Etherum over bitcoin； `zec_btc`:zcoin over bitcoin |
|type|String|Yes|trade type: `buy/sell`|
|price|String|Yes|Price<br> `Greater than 0`|
|amount|String|Yes|Volume <br>More than `0.001` `BTC` |
|sign|String|Yes|signature of the request|

Example

```javascript
# Request
POST https://api.lbkex.com/v1/create_order.do
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


Returns

|Field|Note|
|-|-|
|result|`true` on success or `false` on failure |
|order_id|Order ID| 


#### 3. Cancel the Order

Parameters:

| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Yes|User's `api_key`|
|symbol|String|Yes|Trading Pair<br>`eth_btc`:Etherum over bitcoin； `zec_btc`:zcoin over bitcoin |
|order_id|String|Yes|Order ID<br> Use comma to join the multiply orders. `order_id1,order_id2`, No more than 3 orders in one order.|
|sign|String|Yes|signature of the request|

Example


```javascript
# Request
POST https://api.lbkex.com/v1/cancel_order.do
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

Returns

|Field|Note|
|-|-|
|result|`true` on success or `false` on failure (For single order)|
|order_id|Order ID (For single order)| 
|success|Order IDs cancelled sucessfully. (For multiply orders）| 
|error|Order IDs failed to cancel.（For multiply orders）




#### 4. Query Order 

Parameters:


| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Yes|User's `api_key`|
|symbol|String|Yes|Trading Pair<br>`eth_btc`:Etherum over bitcoin； `zec_btc`:zcoin over bitcoin |
|order_id|String|Yes|Order ID<br> Use comma to join the multiply orders. `order_id1,order_id2`, No more than 3 orders in one order.|
|sign|String|Yes|signature of the request|


Example
```javascript
# Request
POST https://api.lbkex.com/v1/orders_info.do
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

Returns

|Field|Note|
|-|-|
|result|`true` on success or `false` on failure (For single order)|
|order_id|Order ID (For single order)| 
|orders|A list of querying orders| 
|symbol|Trading Pair，`eth_btc`：Etherum， `zec_btc`：ZCash|
|amount|required volume|
|create_time|order time|
|price|price|
|avg_price|Average strike price|
|type|`buy`：Buy<br>`sell`：Sell
|deal_amount|filled volume|
|status|Status<br>`-1`：Cancelled <br>`0`：on trading <br>`1`： filled partially <br> `2`：Filled totally <br>`4`：Cancelling|




#### 5. Query history order (Only the records in recent two days are available)

Parameters:


| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Yes|User's `api_key`|
|symbol|String|Yes|Trading Pair<br>`eth_btc`:Etherum over bitcoin； `zec_btc`:zcoin over bitcoin |
|current_page|String|Yes|Current Page|
|page_length|String|Yes|The records in a page. No more than 200.|
|sign|String|Yes|signature of the request|
|status|String|No|the status of order|

Example

```javascript
# Request
POST https://api.lbkex.com/v1/orders_info_history.do
{
  "api_key"："16702619-0bc8-446d-a3d0-62fb67a8985e",
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

Returns

|Field|Note|
|-|-|
|result|`true` on success or `false` on failure (For single order)|
|symbol|Trading Pair，`eth_btc`：Etherum， `zec_btc`：ZCash|
|order_id|Order ID (For single order)| 
|orders|A list of querying orders| 
|amount|required volume|
|create_time|order time|
|price|price|
|avg_price|Average strike price|
|type|`buy`：Buy<br>`sell`：Sell
|deal_amount|filled volume|
|status|Status<br>`-1`：Cancelled <br>`0`：on trading <br>`1`： filled partially <br> `2`：Filled totally <br> `3`：filled partially and cancelled   <br>`4`：Cancelling|
|current_page|Current Page|
|page_length|The page size|
|total|Total number of records.|


#### 6.Search order transaction details

Parameters:	


| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|sign|String|Yes|signature of the request|
|api_key|String|Yes|User's `api_key`|
|symbol|String|Yes|Trading Pair，`eth_btc`：Etherum， `zec_btc`：ZCash|
|order_id|String|Yes|Order ID|

Example

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
Returns


|Field|Note|
|-|-|
|result|`true` on success or `false` on failure|
|txUuid|Trading ID|
|orderUuid|Order ID| 
|tradeType|`buy`：Buy<br>`sell`：Sell
|dealTime|Trading time| 
|dealPrice|Trading price| 
|dealQuantity|Trading volume|
|dealVolumePrice|Aggregated trading value|
|tradeFee|Transaction fee|
|tradeFeeRate|Transaction fee ratio|


#### 7.Past transaction details

Parameters:


| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|sign|String|Yes|signature of the request|
|api_key|String|Yes|User's `api_key`|
|symbol|String|Yes|Trading Pair，`eth_btc`：Etherum， `zec_btc`：ZCash|
|type|String|No|Order type sell, buy|
|start_date|String|No|Start time yyyy-mm-dd, the maximum is today, the default is yesterday|
|end_date|String|No|Finish time yyyy-mm-dd, the maximum is today, the default is today<br>The start and end date of the query window is up to 2 days|
|from|String|No|Initial transaction number inquiring|
|direct|String|No|inquire direction,The default is the 'next' which  is the positive sequence of dealing time，the 'prev' is inverted order of dealing time|
|size|String|No|Query the number of defaults to 100|

Example

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
Returns	


|Field|Note|
|-|-|
|result|`true` on success or `false` on failure|
|txUuid|Trading ID|
|orderUuid|Order ID| 
|tradeType|`buy`：Buy<br>`sell`：Sell
|dealTime|Trading time| 
|dealPrice|Trading price| 
|dealQuantity|Trading volume|
|dealVolumePrice|Aggregated trading value|
|tradeFee|Transaction fee|
|tradeFeeRate|Transaction fee ratio|


#### 8. Acquiring the basic information of all trading pairs

Parameters:
None

Example


```javascript
# Request
GET https://api.lbkex.com/v1/accuracy.do

# Response
[
  {
    "priceAccuracy": "2",
    "quantityAccuracy": "4",
    "symbol": "pch_usdt"
  }
]
```

Returns

|Field|Note|
|-|-|
|symbol|Trading Pair，`eth_btc`：Etherum， `zec_btc`：ZCash|
|priceAccuracy|Price Accuracy|
|quantityAccuracy|Quantity Accuracy|

#### 9. Acquiring openning orders

Parameters:


| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Yes|User's `api_key`|
|symbol|String|Yes|Trading Pair<br>`eth_btc`:Etherum over bitcoin； `zec_btc`:zcoin over bitcoin |
|current_page|String|Yes|Current Page|
|page_length|String|Yes|The records in a page. No more than 200.|
|sign|String|Yes|signature of the request|

Example


```javascript
# Request
POST https://api.lbkex.com/v1/orders_info_no_deal.do
{
  "api_key": "sijnvsvodnvow928492fh2938fh92348f",
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

Returns

|Field|Note|
|-|-|
|result|`true` on success or `false` on failure (For single order)|
|symbol|Trading Pair，`eth_btc`：Etherum， `zec_btc`：ZCash|
|order_id|Order ID (For single order)| 
|orders|A list of querying orders| 
|amount|required volume|
|create_time|order time|
|price|price|
|avg_price|Average strike price|
|type|`buy`：Buy<br>`sell`：Sell
|deal_amount|filled volume|
|status|Status<br>`0`：on trading <br>`1`： filled partially|
|current_page|Current Page|
|page_length|The page size|
|total|Total number of records.|


#### 10. Exchange rate of USD/RMB (Update on 00:00 everyday)

Parameters

None

```javascript
# Request
GET https://api.lbkex.com/v1/usdToCny.do

# Response
{"USD2CNY":"6.4801"}
```

Returns

|Field|Note|
|-|-|
|USD2CNY |Exchange rate of USD/RMB|

#### 11. Withdraw configurations

Parameters

| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|assetCode|String|No|Code of the asset|

Example
```javascript
# Request
GET https://api.lbkex.com/v1/withdrawConfigs.do

# Response
[{'assetCode': 'eth', 'min': '0.01', 'canWithDraw': True, 'fee': '0.01'}]
```

|Field|Note|
|-|-|
|USD2CNY |Exchange rate of USD/RMB|
|assetCode |Code of token|
|min |Minimum amount to withdraw|
|canWithDraw |Whether the currency can be withdrawn|
|fee |Charged fee for withdrawal（amount）|


#### 12. Withdraw (IP binding is required.)

Parameters

| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Yes|User's `api_key`|
|account|String|Yes|Address,if internal,this is email or mobile account|
|assetCode|String|Yes|Asset Code|
|amount|String|Yes| Amount（Must be integer for NEO）|
|memo|String|No|Required for BTS and/or DCT|
|mark|String|No|User's memo.(length <= 255)|
|sign|String|Yes|signature of the request|
|type|String|No|withdraw type，1:internal，2：normal|

Example

```javascript
# Request
POST https://api.lbkex.com/v1/withdraw.do

# Response
{'result': 'true', 'withdrawId': 90082, 'fee':0.001}
```

Returns

|Field|Note|
|-|-|
|result |true：success，false：failed|
|withdrawId |Current withdrawal ID|
|fee |Withdrawal fee（amount）|



#### 13. Revoke Withdraw (IP binding is required.)

Parameters

| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Yes|User's `api_key`|
|withdrawId |Current withdrawal ID|
|sign|String|Yes|signature of the request|


Example

```javascript
# Request
POST https://api.lbkex.com/v1/withdrawCancel.do

# Response
{'result': 'true', 'withdrawId': '90083'}
```

Returns

|Field|Note|
|-|-|
|result |true：success，false：failed|
|withdrawId |Current withdrawal ID|


#### 14. Withdrawal record (IP binding is required.)


| Parameter|	Type|	Required|	Note|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Yes|User's `api_key`|
|assetCode|String|Yes| |
|status|String|Yes|Status: <br> 0: All, 1: applying. 2. Revoked. 3. Failed. 4. Completed )|
|pageNo|String|Yes|Current Page. Default 1.|
|pageSize|String|Yes|The records in a page. No more than 100. Default 20|
|sign|String|Yes|signature of the request|

Example

```javascript
# Request
POST https://api.lbkex.com/v1/withdraws.do

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

Return

|Field|Note|
|-|-|
|totalPage |Total Number of Pages|
|pageNo|Current Page. |
|pageSize|The records in a page. |
|list| List data（id: coin withdrawal ID，assetCode: Asset Code，address: coin withdrawal address，amount：withdrawal amount fee：withdrawal fee，time：wtihdrawal time，txHash：withdrawal hash， status：Coin withdrawal status）|





