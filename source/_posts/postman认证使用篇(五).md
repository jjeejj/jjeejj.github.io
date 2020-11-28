---
title: postman认证使用篇(五)
date: 2017-11-17 20:18:36
tags: [postman]
---
-----------------------------------------------------------------------

`postman` **认证**使用篇(五)

-----------------------------------------------------------------------
<!--more-->

#### Authorization

尽管请求编辑器已经足够强大去构造各种各样的请求，但是有的时候你的请求可能是需要认证，那么就可以尝试使用下面的认证功能了（由于认证的参数信息属于敏感数据，为了保证在协作环境中工作时数据的安全，建议使用变量）

![alt](/images/postman/auth_view.png)

下面分别说明下拉选项中的认证方式：

##### No Auth

不需要认证，这是默认选中的

##### Bearer Auth

![alt](/images/postman/bearer_auth.png)

填写**Token**进行验证，**JWT**中有使用

##### Basic Auth 基础身份验证

![alt](/images/postman/base_auth.png)

输入用户名和密码，直接发送明文数据，在点击**Preview Request**按钮就会自动在**Headers**中生成**authorization** header.

或者直接发送请求，也会自动把**authorization**header 添加到 **Headers** 中
![alt](/images/postman/base_auth_gen.png)

##### Digest Auth 摘要认证

消息摘要式身份认证是在基本身份认证上面扩展了安全性，服务器为每一个连接生成一个唯一的随机数，客户端用这个随机数对密码进行**MD5**加密，然后返回服务器，服务器也用这个随机数对密码进行加密，然后和客户端传送过来的加密数据进行比较,如果一致就返回结果
客户端请求资源->服务器返回认证标示->客户端发送认证信息->服务器查验认证
![alt](/images/postman/digest_auth.png)

过程：
1. 发送一个请求
    ```
        GET /auth/basic/ HTTP/1.1
        HOST: target
    ```
2. 服务器返回401响应头,要求输入用户凭据
    ```
        HTTP/1.1 401 Unauthorized
        WWW-Authenticate: Digest realm="Digest Encrypt",nonce="nmeEHKLeBAA=aa6ac7ab3cae8f1b73b04e1e3048179777a174b3", opaque="0000000000000000",stale=false, algorithm=MD5, qop="auth"
    ```
3. 输入凭据后再发送请求
    ```
        GET /auth/digest/ HTTP/1.1
        Accept: */*
        Authorization:  Digest username="LengWa", realm="Digest Encrypt",  qop="auth", algorithm="MD5", uri="/auth/digest/", nonce="nmeEHKLeBAA=aa6ac7ab3cae8f1b73b04e1e3048179777a174b3", nc=00000001, cnonce="6092d3a53e37bb44b3a6e0159974108b", opaque="0000000000000000", response="652b2f336aeb085d8dd9d887848c3314"
    ```
4. 服务端验证通过后返回数据


返回401请求头时，对于WWW-Authenticate 各个域的作用：

* realm: 是一个简单的字符串，一般是是邮件格式
* qop: 是认证的(校验)方式，这个比较重要，对后面md5的加密过程有影响
* nonce: 是一个字符串，唯一的、不重复的(还可以包含一些有用的信息，进行验证)
* opaque: 是个字符串，它只是透传而已，即客户端还会原样返回过来
* username: 用户认证的用户名
* uri: 本次资源的位置
* cnonce: 客户端随机参数的GUID
* nc: 是认证的次数，因为如果认证失败，则仍然可以重新发送认证信息继续认证
* response: 这个值很重要，是根据以上信息加上密码根据一定的顺序加密生成的MD5值，服务端在收到这个信息后，也根据相同的方式计算出这个值，而密码保存在服务端。服务端根据用户名去找密码，计算出MD5值，如果和客户端传过来一致，就认证通过，否则不通过

##### OAuth 1.0 / OAuth 2.0

现在大多数授权登录都是基于 **OAuth 2.0**

这两种认证方式，就不具体介绍了。网上讲解的比较多

不了解的可以看看这个文档:[理解OAuth](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)

##### Hawk authentication

![alt](/images/postman/hawk_authentication.png)

**hawk**是一个**HTTP**认证方案，使用**MAC**(Message Authentication Code，消息认证码算法)算法，它提供了对请求进行部分加密验证的认证HTTP请求的方法，包括HTTP方法、请求URI和主机

**hawk**方案要求提供一个共享对称密匙在服务器与客户端之间，通常这个共享的凭证在初始TLS保护阶段建立的，或者是从客户端和服务器都可用的其他一些共享机密信息中获得的

举例过程：
1. 发起一个资源请求
    ```
    GET /resource/1?b=1&a=2 HTTP/1.1
    Host: 127.0.0.1:8000
    ```
2. 服务器返回401响应头
    ```
    HTTP/1.1 401 Unauthorized
    WWW-Authenticate: Hawk
    ```
3. 客户端之前已经获取一组`Hawk`访问资源的资格证书在对应的服务器上
资格证书包含以下的属性：
    ```
    Key identifier(Hawk Auth ID): dh37fgj492je
    Key(Hawk Auth Key): werxhqb98rpaxn39848xrunpaw3489ruxnpa98w4rxn
    Algorithm: hmac-sha-256
    ```
4. 客户端通过计算时间戳来生成认证报头，并构造标准的请求字符串
    ```
    1353832234
    GET
    /resource/1?b=1&a=2
    127.0.0.1
    8000
    some-app-data
    ```
5. 使用`Hawk`凭证中的`Algorithm`指定的加密算法加上`Key(Hawk Auth Key)`指定的`key`进行加密，之后把结果在经过`bae64`转码
`/uYWR6W5vTbY3WKUAN6fa+7p1t+1Yl6hFxKeMLfR6kk=`
6. 客户单在发送请求，在`Authorization`头字段包括`Key identifier(Hawk Auth ID)`指定的id，时间戳，以及加密生成的`MAC`
    ```
    GET /resource/1?b=1&a=2 HTTP/1.1
    Host: 127.0.0.1:8000
    Authorization: Hawk id="dh37fgj492je", ts="1353832234", ext="some-app-data", mac="/uYWR6W5vTbY3WKUAN6fa+7p1t+1Yl6hFxKeMLfR6kk="
    ```
7. 服务端收到请求后再次按相同的算法计算出`MAC`，并验证`Hawk`凭证的有效性，如果验证通过就返回结果

这个认证的方式 `npm`中有包,里面有案例，可以查看一下[hawk](https://www.npmjs.com/package/hawk)

[官方详细文档](https://alexbilbie.com/2012/11/hawk-a-new-http-authentication-scheme/)

##### AWS Signature

**AWS**的使用者可以使用自定义的**HTTP**方案基于**HMAC**的加密算法去认证

参考文档
    [http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html](http://docs.aws.amazon.com/AmazonS3/latest/dev/RESTAuthentication.html)
    [http://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-use-postman-to-call-api.html](http://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-use-postman-to-call-api.html)

![alt](/images/Wechatcode.jpg)
**扫描关注，查看更多文章，提高编程能力**


