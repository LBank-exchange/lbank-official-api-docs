# WebSocket API (Market Data) 

## Start

WebSocket  protocol which achieves full-duplex communications over a  single TCP connection between client and server is based on a new network protocol of the TCP. Server sends information to client, reducing unnecessary overhead such as frequent authentication. It greatest advantage includes that：

> - Request header is small in size (2 Bytes) .
> - The server no longer passively responses after receiving the client's request, but actively pushes the new data to the client.
> - There is no need to establish and close TCP connection repeatedly, so it saves bandwidth and server resources.



## Request & subscription instruction

* URL: `wss://www.lbkex.net/ws/V2/`

* Hearbeat（ping/pong）

    To prevent botch links and reduce server load, server will send a "Ping" message periodically to client connections. When client recieves the "Ping" message, it should response immediately. If a client responds nothing to macth the "Ping" message in one minute,  server will close the connection to the client. Meanwhile client can also send a "Ping" message to server to check whether the connection is working. After server recieves the "Ping" message, it should response with a "Pong" message to match the "Ping".

    **Eg:**

    ```javascript
    
    # ping
    {
        "action":"ping",
        "ping":"0ca8f854-7ba7-4341-9d86-d3327e52804e"
    }
    # pong
    {
        "action":"pong",
        "pong":"0ca8f854-7ba7-4341-9d86-d3327e52804e"
    }
    ```

    where the value of the key "pong" in the "Pong" message should equal to the value of the key "ping" in correspond "Ping" message.



* Subscription/Unsubscription

    Each subscribed data should include one subscribed field at least which specifies the data type of the subscription. Data types that can be subscribed now includes：kbar, tick, depth, trade. Each subscription needs a pair field to specify the trading pair subscribed, which is concatenated with a underline (_). After successful subscription Websocket client receives the updated message sent from server as soon as the subscribed data is updated. 
    
1. Subscription of K-line Data
  

**Parameter:**
    
|Parameter|Parameter Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|action|String|Y|Action requested: `subscribe` or `unsubscribe`|
|subscribe|String|Y|`kbar`|
|kbar|String|Y|To subscribe to k-line types<br>`1min`: 1 minute<br>`5min`: 5 minutes<br>`15min`: 15 minutes<br>`30min`: 30 minutes<br>`1hr`: 1 hour<br>`4hr`: 4 hours<br>`day`: 1 day<br>`week`: 1 week<br>`month`: 1 month<br>`year`: 1 year|
|pair|String|Y|Trading pair:`eth_btc`|

**Eg:**
    
```javascript
    
    # Subscription request
    {
        "action":"subscribe",
        "subscribe":"kbar",
        "kbar":"5min",
        "pair":"eth_btc"
    }
    # Push data
    {
        "kbar":{
            "a":64.32991311,
            "c":0.02590293,
            "t":"2019-06-28T17:45:00.000",
            "v":2481.1912,
            "h":0.02601247,
            "slot":"5min",
            "l":0.02587925,
            "n":272,
            "o":0.02595196
        },
        "type":"kbar",
        "pair":"eth_btc",
        "SERVER":"V2",
        "TS":"2019-06-28T17:49:22.722"
    }
```

**Return value specification:**
    
|Parameters|Parameters Type|Description|
| :-----    | :-----  | :-----   |
|t|Long|K-line updates the timestamp|
|o|BigDecimal|Open price|
|h|BigDecimal|Highest price|
|l|BigDecimal|Lowest price|
|c|BigDecimal|Close price|
|v|BigDecimal|Trading volume|
|a|BigDecimal|Aggregated turnover (summation of price times volume for each trade)|
|n|BigDecimal|Number of trades|
|slot|String|K-line type|



2. Market Depth
       

**Parameter:**
    
|Parameters|Parameters Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|action|String|Y|Type of action requested:`subscribe`,`unsubscribe`|
|subscribe|String|Y|`depth`|
|depth|String|Y|Pro-choise:10/50/100|
|pair|String|Y|Trading pair:`eth_btc`|

**Eg:**

```javascript
    # Subscription request
    {
        "action":"subscribe",
        "subscribe":"depth",
        "depth":"100",
        "pair":"eth_btc"
    }
    # Push data
    {
        "depth":{
            "asks":[
                [
                    0.0252,
                    0.5833
                ],
                [
                    0.025215,
                    4.377
                ],
                ...
            ],
            "bids":[
                [
                    0.025135,
                    3.962
                ],
                [
                    0.025134,
                    3.46
                ],
                ...
            ]
        },
        "count":100,
        "type":"depth",
        "pair":"eth_btc",
        "SERVER":"V2",
        "TS":"2019-06-28T17:49:22.722"
    }
```

**Return value specification:**
|Parameters|Parameters Type|Description|
| :-----    | :-----  | :-----   |
|asks|List|Selling side: list.get(0): delegated price, list.get(1): delegated quantity|
|bids|List|Buying side|


3. Trade record
  

**Parameter:**
    
|Parameters|Parameters Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|action|String|Y|Action requested: `subscribe` or `unsubscribe`|
|subscribe|String|Y|`trade`|
|pair|String|Y|Trading pair:`eth_btc`|


**Eg:**
    
```javascript
    
    # Subscription request
    {
        "action":"subscribe",
        "subscribe":"trade",
        "pair":"eth_btc"
    }
    # 推送数据
    {
        "trade":{
            "volume":6.3607,
            "amount":77148.9303,
            "price":12129,
            "direction":"sell",
            "TS":"2019-06-28T19:55:49.460"
        },
        "type":"trade",
        "pair":"btc_usdt",
        "SERVER":"V2",
        "TS":"2019-06-28T19:55:49.466"
    }
```

**Return value specification:**
    

|Parameters|Parameters Type|Description|
| :-----    | :-----  | :-----   |
|amount|String|Recent trading volume|
|price|Integer|Trade price|
|volumePrice|String|Aggregated turnover|
|direction|String|`sell`,`buy`|
|TS|String|Deal time|

4. Market


**Parameter:**
    
|Parameters|Parameters Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|action|String|Y|Action requested:`subscribe`,`unsubscribe`|
|subscribe|String|Y|`tick`|
|pair|String|Y|Trading pair:`eth_btc`|


**Eg:**
    
```javascript
    
    # Subscription request
    {
        "action":"subscribe",
        "subscribe":"tick",
        "pair":"eth_btc"
    }
    # ush data
    {
        "tick":{
            "to_cny":76643.5,
            "high":0.02719761,
            "vol":497529.7686,
            "low":0.02603071,
            "change":2.54,
            "usd":299.12,
            "to_usd":11083.66,
            "dir":"sell",
            "turnover":13224.0186,
            "latest":0.02698749,
            "cny":2068.41
        },
        "type":"tick",
        "pair":"eth_btc",
        "SERVER":"V2",
        "TS":"2019-07-01T11:33:55.188"
    }
```

**Return value specification:**
    
|Parameters|Parameters Type|Description|
| :-----    | :-----  | :-----   |
|high|BigDecimal|Highest price in last 24 hours|
|low|BigDecimal|Lowest price in last 24 hours|
|latest|BigDecimal|Lastest traded price|
|vol|BigDecimal|Trading volume|
|turnover|BigDecimal|Aggregated turnover (summation of price times volume for each trade)|
|to_cny|BigDecimal|Such as `eth_btc`, convert btc into cny|
|to_usd|BigDecimal|Such as `eth_btc`, convert btc into usd|
|cny|BigDecimal|Such as `eth_btc`, convert eth into cny|
|usd|BigDecimal|Such as `eth_btc`, convert eth into usd|
|dir|String|`sell`, `buy`|
|change|BigDecimal|Price limit in last 24 hours|



**Unsubscribe example:**
    
```javascript
    
    #Unsubscribe from k-line
    {
        "action":"unsubscribe",
        "subscribe":"kbar",
        "kbar":"5min",
        "pair":"eth_btc"
    }
    #Unsubscribe from deep subscription
    {
        "action":"unsubscribe",
        "subscribe":"depth",
        "depth":"100",
        "pair":"eth_btc"
    }
```


* Request data

    One-time request data is supported by Websocket server.

    1. One-time request to get k-line needs extra parameters defined in following：

**Parameter:**

|Parameters|Parameters Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|start|String|fasle|Start time. Accept 2 formats, such as `2018-08-03T17:32:00` (beijing time), another timestamp, such as `1533288720` (Accurate to second)|
|end|String|false|Deadline|
|size|String|false|Number of kbars|

2. One-time request to get trade records needs extra parameters defined in following：
       
   **Parameter:**
       

|Parameters|Parameters Type|Required|Description|
| :-----    | :-----   | :-----    | :-----   |
|size|String|Y|Number of trades|


**Eg:**
    
```javascript
    
    # Get k-line data Request
    {
        "action":"request",
        "request":"kbar",
        "kbar":"5min",
        "pair":"eth_btc"
        "start":"2018-08-03T17:32:00"
        "end":"2018-08-05T17:32:00"
        "size":"576"
    }
    # Get depth data Request
    {
        "action":"request",
        "request":"depth",
        "depth":"100",
        "pair":"eth_btc"
    }
    # Get transaction data Request
    {
        "action":"request",
        "request":"trade",
        "pair":"eth_btc"
        "size":"100",
    }
    # Get market data Request
    {
        "action":"request",
        "request":"tick",
        "pair":"eth_btc"
    }
```


