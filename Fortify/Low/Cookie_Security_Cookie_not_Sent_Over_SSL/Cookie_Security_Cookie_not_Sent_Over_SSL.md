# Cookie Security: Cookie not Sent Over SSL

### RiskLevel : Low

![Cookie_Security_Cookie_not_Sent_Over_SSL_1](/Fortify/Low/Cookie_Security_Cookie_not_Sent_Over_SSL/Cookie_Security_Cookie_not_Sent_Over_SSL_1.png "Cookie_Security_Cookie_not_Sent_Over_SSL")

## Tips
若未設定 cookie 的 secure 旗標，在應用程式同時使用 HTTPS 和 HTTP 時，HTTP 發送要求時也會傳送在 HTTPS 要求時所傳送的 Cookie，可能會造成應用程式資料外洩，可以同時參考 **(RiskLevel : High) Cookie Security: Session Cookie not Sent Over SSL** 的設定。

## 程式碼範例

### 修改前

``` C#
public ActionResult Login(string account, string pwd)
{
    var ticket = new FormsAuthenticationTicket(
    1,account,DateTime.Now,DateTime.Now.AddMinutes(20),false,string.Empty,FormsAuthentication.FormsCookiePath);

    var encTicket = FormsAuthentication.Encrypt(ticket);

    Response.Cookies.Add(new HttpCookie(FormsAuthentication.FormsCookieName, encTicket));

    return RedirectToAction("index", "default");
}
```

> 在 cookie 上設定 Secure，以指示瀏覽器不要公開傳送這些 cookie。

### 修改後

```C#
public ActionResult Login(string account, string pwd)
{
    var ticket = new FormsAuthenticationTicket(
        1,account,DateTime.Now,DateTime.Now.AddMinutes(20),false,string.Empty,FormsAuthentication.FormsCookiePath);

    var encTicket = FormsAuthentication.Encrypt(ticket);

    var cookie = new HttpCookie(FormsAuthentication.FormsCookieName, encTicket);
    cookie.Secure = true;
    
    Response.Cookies.Add(cookie);

    return RedirectToAction("index", "default");
}
```