# Getting Started

## API overview

Lbank provides users with a set of simple and powerful development 
tools designed to help users quickly and efficiently integrate Lbank 
trading functions into their applications.


The following functions can be quickly implemented through the `API`:
>- Market updates
>- Trading information
>- Account Information 
>- Match Depth 
>- Order Information
>- Place Order
>- Cancel Order

All the requests are based on the `https` protocol, and 
`contentType should be set as `application/x-www-form-urlencoded` 
in the request header. The API root URL is `https://www.lbkex.net/`

For example:
```javascript
contentType:'application/x-www-form-urlencoded'
```


## Getting Started
LBank provides three types of \nterfaces to users, the developer can 
choose the most suitable way to look for market information either 
through proceeding the trading, withdrawing the deposit based on 
the users’ preferences. 

#### REST API

REST (Representational State Transfer) is the most popular internet 
software architecture to date. It has a clear structure, conforms to 
standards, is easy to understand, and is convenient to expand. 
It has become increasingly adapted on most websites. 
The advantages of it including:

>- In RESTful architecture, each URL represents a certain kind of resource
>- Between the client and the server, a certain presentation layer of such resources is passed. 
>- The Client uses the four HTTP commands to operate on the server-side resources to achieve “state transition of the presentation layer”

We suggested to use the REST API for currency exchange transactions or asset withdrawals. 

#### WebSocket API
LBank provides an efficient WebSocket Market Interface as well. 
WebSocket is a new protocol for HTML5. It implements full-duplex communication between 
the client and the server, allowing data to be transmitted in both directions quickly. 
A simple handshake can establish a connection between the client and the server. 
The server can push information to the client according to the business rules. 
The advantages of it including:
>- When the client and server perform data transmission, the request header information is relatively small, about 2 bytes;
>- Both client and server can send data to the other party actively;
>- No need to create TCP requests and destroy multiple times, saving bandwidth and server resources.

**_ Developers are strongly advised to use the WebSocket API to obtain information such as market conditions and trading depth. _**


## Get API Permissions

The user's API permissions are obtained from the website's 
`basic settings -> My API`. Click Apply API to get, where 
apiKey is the access key that LBank provides to API users 
and secretKey is the private key used to sign request parameters.

**_ Never reveal these two parameters to anyone. These two are highly relevant to your account security. _**

## Generating Parameter Signature String(to be signed)

User submitted parameters must be signed in addition to sign. 
The string to be signed is ordered according to the parameter 
name (first compares the first letter of all parameter names, 
in alphabet order, if you encounter the same first letter, 
then look at the second letter, and so on).

For example, sign the following parameters

```java
string[] parameters={
  "api_key=c821db84-6fbd-11e4-a9e3-c86000d26d7c"，
  "symbol=eth_btc",
  "type=buy",
  "price=680",
  "amount=1.0"
}; 

```

Generated a string is:
```json
amount=1.0&api_key=c821db84-6fbd-11e4-a9e3-c86000d26d7c&price=680&symbol=eth_btc&type=buy.
```

## MD5 Digest

MD5 algorithm is used to perform a signature operation on the final string to be signed, 
thereby obtaining a signature hex result string (this string is assigned to the parameter 
sign). In the MD5 calculation result, letters are all capitalized.

## RSA Signature

Users could use their private key to perform a signature operation (Base64 coded) to a MD5 digest
by implementing SHA256 algorithm through RSA, after users getting the signature result string 
which is signed by MD5 algorithm. 

# 入门指引    

## API概述    

用户提供简单而又强大的API接口服务，旨在帮助用户快速高效的将 LBank（lbank.info）交易功能整合到自己应用当中。

通过 `API` 您可以快速实现如下功能   
> - 市场最新行情    
> - 交易信息    
> - 账户信息  
> - 买方卖方深度信息    
> - 订单信息    
> - 快速买进卖出    
> - 撤销订单    

所有请求基于 `https` 协议，请求头信息中 `contentType` 需要统一设置为: `'application/x-www-form-urlencoded'`，访问的根URL：`https://www.lbkex.net/` 

例如：
```javascript
contentType:'application/x-www-form-urlencoded'
```
    
## Getting Started  

LBank为用户提供了三种调用接口的方式，开发者可根据自己的使用场景和偏好选择适合自己的方式来查询行情、进行交易或提现。    

#### REST API    

REST，即Representational State Transfer的缩写，是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，正得到越来越多网站的采用。其优点如下：    
> - 在RESTful架构中，每一个URL代表一种资源；    
> - 客户端和服务器之间，传递这种资源的某种表现层；    
> - 客户端通过四个HTTPS指令，对服务器端资源进行操作，实现“表现层状态转化”。    

建议开发者使用REST API进行币币交易或者资产提现等操作。 

#### WebSocket API    
LBank同样为用户提供了快速高效的WebSocket行情接口，

WebSocket是HTML5一种新的协议(Protocol)。它实现了客户端与服务器全双工通信，使得数据可以快速地双向传播。通过一次简单的握手就可以建立客户端和服务器连接，服务器根据业务规则可以主动推送信息给客户端。其优点如下：

>- 客户端和服务器进行数据传输时，请求头信息比较小，大概2个字节;    
>- 客户端和服务器皆可以主动地发送数据给对方；    
>- 不需要多次创建TCP请求和销毁，节约宽带和服务器的资源。    

**_强烈建议开发者使用WebSocket API获取市场行情和买卖深度等信息。_**

## 开启API权限    

用户的API权限在网站的`安全->API`内获取。点击`申请API`即可获得，其中`apiKey`是LBank提供给API用户的访问密钥，`secretKey`用于对请求参数签名的私钥。 

**_注意： 请勿向任何人泄露这两个参数，这两个参数关乎您账号的安全。_**    
     
## 生成待签名字符串

用户提交的参数除`sign`外，都要参与签名。待签名字符串要求按照参数名进行排序（首先比较所有参数名的第一个字母，按abcd顺序排列，若遇到相同首字母,则看第二个字母, 以此类推。）

例如：对于如下的参数进行签名 

```java
string[] parameters={
  "api_key=c821db84-6fbd-11e4-a9e3-c86000d26d7c"，
  "symbol=eth_btc",
  "type=buy",
  "price=680",
  "amount=1.0"
}; 

```
生成待签名字符串为：
```json
amount=1.0&api_key=c821db84-6fbd-11e4-a9e3-c86000d26d7c&price=680&symbol=eth_btc&type=buy.
```

## MD5摘要

MD5算法 对最终待签名字符串进行签名运算,从而得到签名结果字符串(该字符串赋值于参数`sign`)，MD5计算结果中字母全部大写。

## RSA签名

用户得到MD5签名过后的结果字符串后，在使用自己的私钥，通过RSA使用SHA256算法对MD5摘要做一次签名(用Base64编码)，并将最终的签名结果赋值于参数'sign'。
