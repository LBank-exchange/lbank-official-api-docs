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


请求参数	

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
|symbol|String|Yes|交易对<br>`eth_btc`:以太坊； `zec_btc`:零币 |
|type|String|Yes|trade type: `buy/sell`|
|price|String|Yes|下单价格<br>买单和卖单：`大于等于0`|
|amount|String|Yes|交易数量 <br>卖单和卖单：`BTC` 数量大于等于 `0.001`|
|sign|String|Yes|signature of the request|
