# HTML Cookie

cookie使得无状态的HTTP能够记录状态

## Cookie大致流程

服务器收到HTTP请求后，会添加Set-Cookie选项，浏览器收到响应后会保存下Cookie而后在后面请求中放在HTTP Cookie标头内。

服务器使用Set-Cookie向浏览器发送Cookie信息

```http
Set-Cookie: <cookie-name>=<cookie-value>
```

服务器为客户端设置cookie

```http
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[页面内容]
```

之后客户端发送请求

```http
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

## Cookie的生命周期

- 会话期Cookie

  会话结束之后删除（HTTP持久性链接），如果考虑**浏览器会话恢复**功能可能生命周期无限延长

- 持久性Cookie

  设定的之后删除

如果站点对用户身份验证，每当用户身份验证时都应该**重新生成**并发送Cookie从而防止**会话固定攻击**

> 用户身份验证即为使用cookie时候，每次使用cookie后都要重新生成因此可以理解为在这种模式下cookie为一次性对象
>
> ### 会话固定攻击
>
> 如果cookie非一次性那么就可能存在第三方抓包获取cookie从而伪装进行会话的可能性

## Cookie的限制访问

确保Cookie安全发送不会被意外的参与者或脚本访问：Secure属性以及HttpOnly属性

- Secure

  只能使用HTTPS协议发送， http:开头的站点无法使用

- HttpOnly

  js document.cookie API无法访问带有此属性的cookie，只有服务器有用

