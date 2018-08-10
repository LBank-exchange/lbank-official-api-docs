
WebSocket API (V2.0 beta) 
===========================

WebSocket协议是基于TCP的一种新的网络协议。它实现了
客户端与服务器之间在单个 tcp 连接上的全双工通信，由
服务器主动发送信息给客户端，减少了频繁的身份验证等不
必要的开销。其最大优点有两个：

. 两方请求的 header 数据很小，大概只有2 Bytes。
. 服务器不再是被动的接到客户端的请求后才返回数据，而是有了新数据后主动推送给客户端。
. 不需要多次创建TCP请求和销毁，节约宽带和服务器的资源。



请求与订阅说明
==================

1. 访问地址
    ws://47.52.110.164/ws/V2/
    ws://api.lbank.info/ws/V2/


2. 数据格式：
    所有发送/接受的数据都是标准多JSON格式。每个标准的请求都有一个action字段，
    表明请求的动作类型。现有的action字段是如下几种动作之一：
    ping：心跳信息
    pong：回应心跳
    request：请求数据
    subscribe：订阅数据
    unsubscribe：推订数据
    不同的动作需要不同的参数，在下面章节仔细说明。
    两次动作之间至少间隔0.1s，过于频繁对请求会被切断连接。


3. 心跳（ping/pong）
    为了防止僵尸链接，减少服务器负荷，服务器端会定时向客户连接发送心跳PING信息。
    客户端收到PING消息后，应立即回应。如果超过一分钟没有回应任何PING消息，连接
    会被自动关闭。

    同时，客户也可以向服务器短发送PING消息，用于检查连接是否正常。服务器端收到
    PING消息后，也会回应对应端PONG消息。

    一条标准的ping消息格式如下：
    {'action': 'ping', 'ping': '0ca8f854-7ba7-4341-9d86-d3327e52804e'}

    收到该消息后，用户应该立刻回应对pong消息：
    {'action': 'pong', 'pong': '0ca8f854-7ba7-4341-9d86-d3327e52804e'}
    其中，pong字段必需和收到对ping消息字段完全一致。

    ** 如果超过一分钟没有回应任何PING消息，连接会被自动关闭。 **


4. 订阅/退订数据（subscribe/unsubscribe）
    每个订阅数据应该至少包括一个subscribe字段，用于指定订阅的数据类型。现在可以订阅
    的数据包括：kbar，tick，depth，trade四种。每个订阅数据都需要一个pair字段，用
    来指定订阅的交易对，交易对以下划线（_)、连接。匿名用户单个连接不应该订阅超过10种
    数据，用户登陆后可以同时订阅更多数据。
    * kbar:  K线数据（蜡烛图）。除了上面的两个字段，还需要一个额外字段：kbar，
      用于指定K线的时间间隔，当前该参数接受的选项包括1min, 5min, 15min, 30min, 
      1hr, 2hr, 3hr, 4hr, 6hr, 8hr, 12hr, day, week, month, year; 
    * depth：当前交易的深度数据。除了上面的两个字段，还需要一个额外字段：depth，用于
      指定深度，现在有 10/50/100 三个深度可以选择
    * tick： 当前交易的实时快照
    * trade：逐条交易记录

    以下是一些订阅数据的例子：
    订阅eth/btc交易对的5min K线：
    {'action': 'subscribe', 'subscribe': 'kbar', 'kbar': '5min', 'pair': 'eth_btc'}

    订阅dax/eth交易对的深度数据（深度10）：
    {'action': 'subscribe', 'subscribe': 'depth', 'depth': 10, 'pair': 'dax_eth'}

    订阅btc/usdt交易对的交易记录：
    {'action': 'subscribe', 'subscribe': 'trade', 'pair': 'btc_usdt'}

    订阅vtho/eth交易对的快照记录：
    {'action': 'subscribe', 'subscribe': 'tick', 'pair': 'vtho_eth'}


5. 请求数据（request）
    每个订阅数据应该至少包括一个request字段，用于指定订阅的数据类型。现在可以订阅
    的数据包括：kbar，tick，depth，depth四种。除kbar外，其它三种数据暂时不支持
    历史数据的查询。
    * kbar:  K线数据（蜡烛图）。除了上面的两个字段，还需要一个额外字段：kbar，
      用于指定K线的时间间隔，当前该参数接受的选项包括1min, 5min, 15min, 30min, 
      1hr, 2hr, 3hr, 4hr, 6hr, 8hr, 12hr, day, week, month, year。如果
      需要查询历史数据，需要增加两个字段start和size。start是开始时间，接受两种格
      式，一种是ISO格式，精确到秒，如'2018-08-03T17:32:00'（北京时间），另一种
      是UNIX时间戳，暨1970年1月1日0时（UTC/GMT）开始的秒数，如1514736000.0代
      表北京时间2018年1月1日0点。size是需要获取的kbar的条数。最大不超过1440条。
      若没有start和size这两个参数，默认获取最新的一条kbar数据。
    * depth：当前交易的深度数据。除了上面的两个字段，还需要一个额外字段：depth，
      用于指定深度，现在有 10/50/100 三个深度可以选择    
    * tick： 当前交易的实时快照
    * trade：获取最近的交易记录。除了上面的两个字段，还需要一个额外字段：size，用
      于指定获取的交易数目。最多不超过100条。
