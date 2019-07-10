# WebSocket API（资产及订单）

## 开始使用 

* 本篇所列出`REST`接口访问根URL：`https://www.lbkex.net/` 或 `https://api.lbkex.com/`
* 本篇所列出的`Websocket`访问地址: `wss://www.lbkex.net/ws/V2/`

## 与Websocket接口相关的REST接口

1.生成subscribeKey
```
POST /v1/subscribe/get_key.do
```
从创建时刻起有效期为60分钟
	
**请求参数:**

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|sign|String|是|请求参数的签名|

**请求示例:**
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
	
**返回值说明:**

|字段|参数类型|描述|
|-|-|-|
|result|String|true：成功，false：失败|
|subscribeKey|String|`subscribeKey`| 


2.延长subscribeKey有效期
```
POST /v1/subscribe/refresh_key.do
```
有效期延长至本次调用后60分钟

**请求参数:**

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|sign|String|是|请求参数的签名|
|subscribeKey|String|是|`subscribeKey`|


**请求示例:**

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

**返回值说明:**

|字段|参数类型|描述|
|-|-|-|
|result|String|true：成功，false：失败|


3.关闭subscribeKey
```
POST /v1/subscribe/destroy_key.do
```
关闭某账户数据流


**请求参数:**

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|api_key|String|是|用户申请的 `api_key`|
|sign|String|是|请求参数的签名|
|subscribeKey|String|是|`subscribeKey`|

**示例:**

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

**返回值说明:**

|字段|参数类型|描述|
|-|-|-|
|result|String|true：成功，false：失败|



## Websocket订阅/退订数据（subscribe/unsubscribe）

1.订阅订单更新

当账户下有新订单创建、订单有新成交或者新的状态变化时会推送此类事件

**参数:**

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|action|String|是|请求的动作类型:`subscribe`|
|subscribe|String|是|`orderUpdate`|
|subscribeKey|String|是|通过`REST`接口`/v1/subscribe/refresh_key.do`获取|
|pair|String|是|交易对如:`eth_btc`,支持匹配全部:`all`|

**示例:**

```javascript
# 订阅请求
{
  "action": "subscribe",
  "subscribe": "orderUpdate",
  "subscribeKey": "24d87a4xxxxxd04b78713f42643xxxxf4b6f6378xxxxx35836260",
  "pair": "all",
}
# 推送数据
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

**返回值说明:**

|参数名|	参数类型|	描述|
| :-----    | :-----  | :-----   |
|uuid|String|订单Id|
|txUuid|String|成交记录Id|
|amount|String|最近成交数量|
|volumePrice|String|最近成交数额|
|role|Long|`maker`,`taker`|
|price|Integer|最新价(当订单状态为`1`或者`2`时为成交价格，其他为委托价格)|
|orderStatus|Integer|订单状态<br>`-1`：已撤销 <br>`0`：未成交 <br>`1`： 部分成交<br> `2`：完全成交 <br>`4`：撤单处理中|
|updateTime|Long|订单更新时间|


2.取消订阅订单更新

关闭订单数据的推送

**参数:**

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|action|String|是|请求的动作类型:`unsubscribe`|
|subscribe|String|是|订阅数据:`orderUpdate`|
|subscribeKey|String|是|通过`REST`接口`/v1/subscribe/refresh_key.do`获取|
|pair|String|是|交易对如:`eth_btc`,支持匹配全部:`all`|

**示例:**

```javascript
{
  "action": "unsubscribe",
  "subscribe": "orderUpdate",
  "subscribeKey": "24d87a4xxxxxd04b78713f42643xxxxf4b6f6378xxxxx35836260",
  "pair": "all",
}
```
