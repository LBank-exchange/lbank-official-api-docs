 # 币币交易REST API(行情)

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

### 币币行情 API 

1.获取LBank币币行情数据  

> URL: `http://api.lbank.info/v1/ticker.do`

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|币对 <br>如: `eth_btc`、`zec_btc`、 `all`|


> `symbol` 取 `all` 时，则获取所有行情


请求示例1:	

```javascript
# Request
GET http://api.lbank.info/v1/ticker.do
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
请求示例2：

```javascript
# RequestGET http://api.lbank.info/v1/ticker.do
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

返回值说明	


|字段|描述|
|-|-|
|vol|累计成交量|
|high|最高价|
|low|最低价|
|change|涨跌幅|
|turnover|累计成交额|
|latest|最新成交价|
|timestamp|服务器时间戳|



2.获取LBank可用交易对接口

> URL: `http://api.lbank.info/v1/currencyPairs.do`	

请求参数: `无`

请求示例：

```javascript
# Request
GET http://api.lbank.info/v1/currencyPairs.do

# Response[
  "bcc_eth","etc_btc","dbc_neo","eth_btc",
  "zec_btc","qtum_btc","sc_btc","ven_btc",
  "ven_eth","sc_eth","zec_eth"
]
```

返回值说明
> 所有可用交易对


3.获取LBank市场深度

URL: `http://api.lbank.info/v1/depth.do`	

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|币对如 `eth_btc`|
|size|Integer|否(默认60)|返回的条数(1-60)|
|merge|Integer|否(默认0)|深度: 0,1|

请求示例
```javascript
# Request
GET http://api.lbank.info/v1/depth.do
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

返回值说明:
```
asks :卖方深度
bids :买方深度
```
4.获取LBank历史交易信息

URL: `http://api.lbank.info/v1/trades.do`	

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|币对 <br>如 `eth_btc`|
|size|Integer|否(默认600)|返回的条数(1-600)|
|time|String|否|返回时间戳之后 `size` 条数据，为空则返回最新 `size` 条数据|

请求示例
```javascript
# Request
GET http://api.lbank.info/v1/trades.do
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

返回值说明:

|字段|描述|
|-|-|
|date_ms|交易时间|
|amount|交易数量|
|price|交易价格|
|type|buy/sell|
|tid|交易ID|


5.获取K线数据

URL: `http://api.lbank.info/v1/kline.do`	

请求参数	

|参数名|	参数类型|	必填|	描述|
| :-----    | :-----   | :-----    | :-----   |
|symbol|String|是|币对如 `eth_btc`|
|size|Integer|是|返回的条数(1-2880)|
|type|String|是|`minute1`：1分钟<br>`minute5`：5分钟<br>`minute15`：15分钟<br>`minute30`：30分钟<br>`hour1`：1小时<br>`hour4`：4小时<br>`hour8`：8小时<br>`hour12`：12小时<br>`day1`：1日<br>`week1`：1周<br>|
|time|String|是|时间戳 (精确到秒)

请求示例
```javascript
# Request
GET http://api.lbank.info/v1/kline.do
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

返回值说明:

|字段|描述|
|-|-|
|1482311500|时间戳|
|5423.23|开|
|5472.80|高|
|5516.09|低|
|5462|收|
|234.3250|交易量|
