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

1. Acquiring Users' Asset Information


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


2. Place Order

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


3. Cancel the Order

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




4. Query Order 
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







