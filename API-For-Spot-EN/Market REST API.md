 # Marketing REST API

## Getting Start    
REST (Representational State Transfer) is the most popular internet software architecture nowadays. It is clear in structure, easy to understand, and convenient to expand. More and more companies therefore apply the structures in their websites. It has following advantages:

>- In a RESTful architecture, each URL represents a resource
>- Between the client and the server, a certain presentation layer of such resources passed
>- The client uses the four HTTP commands to operate on the server-side resources to achieve “state transition of the presentation layer.”

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
### Marketing API

1. 
1. Query current Market Data  

> URL: `https://api.lbkex.com/v1/ticker.do`

Parameters	

|Name|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|Yes|Pair <br>Such as: `eth_btc`、`zec_btc`、 `all`|


> Set `all` to `symbol` to get data of all trading pairs.


Example 1:	

```javascript
# Request
GET https://api.lbkex.com/v1/ticker.do
{
  "symbol"："all"
}
# Response
[
  {
    "symbol"："eth_btc",
    "timestamp"："1410431279000",
    "ticker"：{
      "change"："4.21",
      "high"："7722.58",
      "latest"："7682.29",
      "low"："7348.30",
      "turnover"："0.00",
      "vol"："1316.3235"
    }
  },
  {
    "symbol"："sc_btc",
    "timestamp"："1410431279000",
    "ticker"：{
      "change"："4.21",
      "high"："7722.58",
      "latest"："7682.29",
      "low"："7348.30",
      "turnover"："0.00",
      "vol"："1316.3235"
    }
  }
]
```
Example 2

```javascript
# RequestGET https://api.lbkex.com/v1/ticker.do
{
  "symbol"："eth_btc"
}
# Response
{
  "timestamp"："1410431279000",
  "ticker"：{
    "change"："4.21",
    "high"："7722.58",
    "latest"："7682.29",
    "low"："7348.30",
    "turnover"："0.00",
    "vol"："1316.3235"
  }
}
```

Returns	


|Field|Description|
|-|-|
|vol|24 hr trading volume|
|high|24 hr highest price|
|low|24 hr lowest price|
|change|Fluctuation (%) in 24 hr|
|turnover|Total Turn over in 24 hr|
|latest|Latest Price|
|timestamp|Timestamp of latest transaction|



2. Available trading pairs

> URL: `https://api.lbkex.com/v1/currencyPairs.do`	

Paramters: `None`

Example

```javascript
# Request
GET https://api.lbkex.com/v1/currencyPairs.do

# Response[
  "bcc_eth","etc_btc","dbc_neo","eth_btc",
  "zec_btc","qtum_btc","sc_btc","ven_btc",
  "ven_eth","sc_eth","zec_eth"
]
```

Returns
> All available trading pairs


3. Market Depth

URL: `https://api.lbkex.com/v1/depth.do`	

Parameters	

|Name|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|Yes|Trading Pair. Such as `eth_btc`|
|size|Integer|No(Default is 60)|The count of returned items.(1-60)|
|merge|Integer|No(Default is 0)|Depth: 0,1|

Example
```javascript
# Request
GET https://api.lbkex.com/v1/depth.do
{
  "symbol"："eth_btc",
  "size"："60",
  "merge"："1"
}
# Response
{
  "asks"：[
    [5370.4, 0.32],
    [5369.5, 0.28],
    [5369.24, 0.05],
    [5368.2, 0.079],
    [5367.9, 0.023]
  ],
  "bids"：[
    [5367.24, 0.32],
    [5367.16, 1.31],
    [5366.18, 0.56],
    [5366.03, 1.42],
    [5365.77, 2.64]
  ]
}
```

Returns:
```
asks :Depth of asks (Sellers')
bids :Depth of bids (Buyers')
```

4. Query historical transactions

URL: `https://api.lbkex.com/v1/trades.do`	

Parameters	

|Name|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|Yes| Trading pair. <br>Such as `eth_btc`|
|size|Integer|No(Default is 600)|The count of returned items.(1-600)|
|time|String|No|Start transaction timestamp of the querying. Return latest records if it is not provided. |

Example
```javascript
# Request
GET https://api.lbkex.com/v1/trades.do
{
  "symbol"："eth_btc",
  "size"："600",
  "time"："1482311600000"
}
# Response[
  {
    "date_ms"：1482311500000,
    "amount"：1.4422,
    "price"：5242.66,
    "type"："buy",
    "tid"："a4aie34"
  },
  {
    "date_ms"：1482311400000,
    "amount"：51.3454,
    "price"：5412.24,
    "type"："sell",
    "tid"："iodsio1934"
  },
  {
    "date_ms"：1482311300000,
    "amount"：4.2355,
    "price"：5124.22,
    "type"："buy",
    "tid"："di124kq"
  }
]
```

Returns:

|Field|Description|
|-|-|
|date_ms|Transaction Time|
|amount|Transaction Volume|
|price|Transaction Price|
|type|buy or sell|
|tid|Transaction ID|


5. Query K Bar Data

URL: `https://api.lbkex.com/v1/kline.do`	

Parameters	

|Name|	Type|	Required|	Description|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|Yes|Trading Pair `eth_btc`|
|size|Integer|Yes|Count of the bars (1-2880)|
|type|String|Yes|`minute1`：1 minute<br>`minute5`：5 minutes<br> `minute15`：15minutes<br>`minute30`：30 minutes<br>`hour1`：1 hour<br>`hour4`：4 hours<br>`hour8`：8 hours<br>`hour12`：12 hours<br>`day1`：1 day<br>`week1`：1 week<br>`month1`：1 month<br> |
|time|String|Yes|Timestamp (of Seconds)|

Example
```javascript
# Request
GET https://api.lbkex.com/v1/kline.do
{
  "symbol"："eth_btc",
  "size"："600",
  "type"："minute1",
  "time"："1482311600"
}
# Response
[
  [
    1482311500,
    5423.23,
    5472.80,
    5516.09,
    5462,
    234.3250
  ],
  [
    1482311400,
    5432.52,
    5459.87,
    5414.30,
    5428.23,
    213.7329
  ]
]
```

Returns:

|Field|Description|
|-|-|
|1482311500|Timestamp|
|5423.23|Open Price|
|5472.80|Highest Price|
|5516.09|Lowest Price|
|5462|Close Price|
|234.3250|Trading Volume|
