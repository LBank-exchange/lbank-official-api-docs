# WebSocket API（Asset & Order）

## Start use 

* The REST interface listed in this article accesses the root URL：`https://www.lbkex.net/` or `https://api.lbkex.com/`
* Websocket access addresses listed in this article: `wss://www.lbkex.net/ws/V2/`

## The REST interface is related to the Websocket interface

1.Create subscribeKey
```
POST /v1/subscribe/get_key.do
```
The key is valid in 60 minutes from creation.
	
**Parameter:**

|Parameters|Parameters Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Y|API key requested by user|
|sign|String|Y|Signature of parameter in request|

**Eg:**

```javascript
# Request
POST https://www.lbkex.net/v1/subscribe/get_key.do
{
  "api_key"："16702619-0xx8-446d-axx0-62xxx7a8985e",
  "sign"："0E0872AD955C0E715B43C78F24B3053A",
}
# Response
{
    "result":"true",
    "subscribeKey":"24d87a4xxxxxd04b78713f42643xxxxf4b6f6378xxxxx35836260"
}
```

**Return value specification:**

|Fields|Parameters Type|Description|
|-|-|-|
|result|String|true: succeed; false: failure|
|subscribeKey|String|`subscribeKey`|


2.Extend the validity of subscribeKey
```
POST /v1/subscribe/refresh_key.do
```
The key is valid in 60 minutes from this call.

**Parameter:**

|Parameters|Parameters Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Y|API key requested by user|
|sign|String|Y|Signature of parameter in request|
|subscribeKey|String|Y|`subscribeKey`|


**Eg:**

```javascript
# Request
POST https://www.lbkex.net/v1/subscribe/refresh_key.do
{
  "api_key"："16702619-0xx8-446d-axx0-62xxx7a8985e",
  "sign"："0E0872AD955C0E715B43C78F24B3053A",
  "subscribeKey"："24d87a4xxxxxd04b78713f42643xxxxf4b6f6378xxxxx35836260",
}
# Response
{"result": "true"}
```

**Return value specification:**

|Fields|Parameters Type|Description|
|-|-|-|
|result|String|true: succeed; false: failure|


3.Close subscribeKey
```
POST /v1/subscribe/destroy_key.do
```
Close the data stream for an account.


**Parameter:**

|Parameters|Parameters Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|Y|API key requested by user|
|sign|String|Y|Signature of parameter in request|
|subscribeKey|String|Y|`subscribeKey`|

**Eg:**

```javascript
# Request
POST https://www.lbkex.net/v1/subscribe/destroy_key.do
{
  "api_key"："16702619-0xx8-446d-axx0-62xxx7a8985e",
  "sign"："0E0872AD955C0E715B43C78F24B3053A",
  "subscribeKey"："24d87a4xxxxxd04b78713f42643xxxxf4b6f6378xxxxx35836260",
}
# Response
{"result": "true"}
```

**Return value specification:**

|Fields|Parameters Type|Description|
|-|-|-|
|result|String|true: succeed; false: failure|



## Websocket（subscribe/unsubscribe）

1.Update subscribed orders

Such events will be pushed when there are new orders created, new orders dealed or new status changes of the account.

**Parameters:**

|Parameters|Parameters Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|action|String|Y|Action requested: `subscribe`|
|subscribe|String|Y|`orderUpdate`|
|subscribeKey|String|Y|Obtained through the REST interface of the `/v1/subscribe/refresh_key.do`|
|pair|String|Y|Trading pair :`eth_btc`. Support matching all: `all`|

**Eg:**

```javascript
# Subscription request
{
  "action": "subscribe",
  "subscribe": "orderUpdate",
  "subscribeKey": "24d87a4xxxxxd04b78713f42643xxxxf4b6f6378xxxxx35836260",
  "pair": "all",
}
# Push data
{
    "orderUpdate":{
        "amount":"0.003",
        "orderStatus":2,
        "price":"0.02455211",
        "role":"maker",
        "updateTime":1561704577786,
        "uuid":"d0db191d-xxxxx-4418-xxxxx-fbb1xxxx2ea9",
	"txUuid":"da88f354d5xxxxxxa12128aa5bdcb3",
        "volumePrice":"0.00007365633"
    },
    "pair":"eth_btc",	
    "type":"orderUpdate",
    "SERVER":"V2",
    "TS":"2019-06-28T14:49:37.816"
}
```

**Return value specification:**

|Parameters|Parameters Type|Description|
| :-----    | :-----  | :-----   |
|uuid|String|Order ID|
|txUuid|String|Trading record ID|
|amount|String|Trading volume|
|volumePrice|String|Aggregated trading value|
|role|Long|`maker`,`taker`|
|price|Integer|Last price(When the order status is' 1 'or' 2 ' ，it is the   transaction price, and the others are the Delegat price)|
|orderStatus|Integer|Order status<br/>`-1`：Withdrawn <br/>`0`：Unsettled <br/>`1`： Partial sale<br/>`2`：Close the deal <br/>`4`：Withdrawing|
|updateTime|Long|Updating time of order|


2.Cancel updating subscribed orders

Close pushing order data.

**Parameter:**

|Parameters|Parameters Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|action|String|Y|Action requested: `unsubscribe`|
|subscribe|String|Y|Subscribe data: `orderUpdate`|
|subscribeKey|String|Y|Obtained through the REST interface of the `/v1/subscribe/refresh_key.do`|
|pair|String|Y|Trading pair :`eth_btc`. Support matching all: `all`|

**Eg:**

```javascript
{
  "action": "unsubscribe",
  "subscribe": "orderUpdate",
  "subscribeKey": "24d87a4xxxxxd04b78713f42643xxxxf4b6f6378xxxxx35836260",
  "pair": "all",
}
```
