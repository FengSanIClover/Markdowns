# Cookie Security: Session Cookie not Sent Over SSL

### RiskLevel : High

![Cookie_Security_Session_Cookie_not_Sent_Over_SSL_1](/Fortify/High/Cookie_Security_Session_Cookie_not_Sent_Over_SSL/Cookie_Security_Session_Cookie_not_Sent_Over_SSL_1.png "Cookie_Security_Session_Cookie_not_Sent_Over_SSL")
![Cookie_Security_Session_Cookie_not_Sent_Over_SSL_2](/Fortify/High/Cookie_Security_Session_Cookie_not_Sent_Over_SSL/Cookie_Security_Session_Cookie_not_Sent_Over_SSL_2.png "Cookie_Security_Session_Cookie_not_Sent_Over_SSL")

## Tips
若**未設定 cookie 的 secure 旗標**，在應用程式同時使用 HTTPS 和 HTTP 時，**HTTP 發送要求時也會傳送在 HTTPS 要求時所傳送的 Cookie**，可能會造成應用程式資料外洩。

## 程式碼範例

### 修改前

``` HTML
<authentication mode="Forms">
    <forms loginUrl="~/Login" timeout="15" defaultUrl="~/Home/Index" />
</authentication>
```

> 加入 requireSSL="true" 啟用 cookie 的 secure 旗標，以指示瀏覽器不要公開傳送這些 cookie。

### 修改後

``` HTML
<authentication mode="Forms">
    <forms loginUrl="~/Login" timeout="15" requireSSL="true" defaultUrl="~/Home/Index" />
</authentication>
```