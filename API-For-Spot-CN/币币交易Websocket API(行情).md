# 币币交易Websocket API(行情)

## 开始使用    

WebSocket是HTML5一种新的协议(Protocol)。它实现了客户端与服务器全双工通信，使得数据可以快速地双向传播。通过一次简单的握手就可以建立客户端和服务器连接，服务器根据业务规则可以主动推送信息给客户端。其优点如下：   
>- 客户端和服务器进行数据传输时，请求头信息比较小，大概2个字节；   
>- 客户端和服务器皆可以主动地发送数据给对方；   
>- 不需要多次创建TCP请求和销毁，节约宽带和服务器的资源。    

强烈建议开发者使用WebSocket API获取市场行情和买卖深度等信息。    

**_PS：当前LBank WebSocket API提供的行情服务接口无需进行安全认证。_**

## 请求交互    

访问的根URL：`ws://api.lbank.info/ws`，访问时需要科学上网

### 行情 API

1. lh_sub_spot_X_btc_trades    订阅行情数据

```javascript
// 'X' 值为币对，如 'eth_btc'
websocket.send("{ 'event':'addChannel', 'channel':'lh_sub_spot_X_btc_trades' }"); 
```



示例	

```javascript
# Request
{
  "event":'addChannel',
  "channel":'lh_sub_spot_eth_btc_trades'
}
# Response
{
  "data":[
    [
      "694481a4e8954c31a36c90b114d8dcd8",
      "7043.57",
      "1.0524",
      "1483410480021",
      "buy"
    ],
    [
      "5258c4d6d5e7494fbcba467a5614a9c6",
      "6955.6914",
      "1.0869",
      "1483409918273",
      "sell"
    ]
  ],
  "channel":"lh_sub_spot_eth_btc_trades"
}
```

返回值说明	


|字段|描述|
|-|-|
|694481a4e8954c31a36c90b114d8dcd8|交易流水号|
|7043.57|价格|
|1.0524|成交量|
|1483410480021|时间|
|Buy/sell|买卖类型|


2. lh_sub_spot_X_btc_depth_Y    订阅市场深度

```javascript
// 'X' 值为：btc, zec
// 'Y' 值为：20, 60
websocket.send("{' event':'addChannel', 'channel':'lh_sub_spot_X_btc_depth_Y' }"); 
```

示例	

```javascript
# Request
{
  "event":'addChannel',
  "channel":'lh_sub_spot_eth_btc_depth_20'
}
# Response
{
  "data":{
  "asks":[
    [
      6515.85,
      3.2306
    ],
    [
      6515.89,
      0.2822
    ]
  ],
  "bids":[
    [
      6515.05,
      3.2919
    ],
    [
      6515.01,
      2.1435
    ]
  ],
  "timestamp":1483521112301
  },
  "channel":"lh_sub_spot_eth_btc_depth_20"
}
```

返回值说明	


|字段|描述|
|-|-|
|asks|卖方深度 |
|bids|买方深度|
|timestamp|服务器时间戳|

3. lh_sub_spot_X_btc_trades    订阅行情数据

```javascript
// X值为：btc, zec
wwebsocket.send("{ 'event':'addChannel', 'channel':'lh_sub_spot_X_btc_trades' }"); 
```

示例	

```javascript
# Request
{
  "event":'addChannel',
  "channel":'lh_sub_spot_eth_btc_trades'
}
# Response
{
  "data":[
    [
      "694481a4e8954c31a36c90b114d8dcd8",
      "7043.57",
      "1.0524",
      "1483410480021",
      "buy"
    ],
    [
      "5258c4d6d5e7494fbcba467a5614a9c6",
      "6955.6914",
      "1.0869",
      "1483409918273",
      "sell"
    ]
  ],
  "channel":"lh_sub_spot_eth_btc_trades"
}
```

返回值说明	


|字段|描述|
|-|-|
|694481a4e8954c31a36c90b114d8dcd8|交易流水号|
|7043.57|价格|
|1.0524|成交量|
|1483410480021|时间|
|Buy/sell|买卖类型|


4. lh_sub_spot_X_btc_kline_Y    订阅行情数据

```javascript
/*
X值为：btc, zec
X值为：Y值为：1minute, 5minute, 10minute, 15minute, 30minute, 1hour, 2hour, 4hour, 6hour, 8hour,12hour,1day, 1week, 1month, 1year
*/
wwebsocket.send("{ 'event':'addChannel', 'channel':'lh_sub_spot_X_btc_trades' }"); 
```

示例	

```javascript
# Request
{
  "event":'addChannel',
  "channel":'lh_sub_spot_eth_btc_kline_1day'
}
# Response
{
  "data":[
    [
      1483459200000,
      7377.82,
      7722.88,
      7348.30,
      7675.15,
      1339.5527
    ]
  ],
  "data":"lh_sub_spot_eth_btc_kline_1day"
}
```

返回值说明	


|字段|描述|
|-|-|
|1483459200000|时间戳|
|7377.82|开|
|7722.88|高|
|7348.30|低|
|7675.15|收|
|1339.5527|交易量|