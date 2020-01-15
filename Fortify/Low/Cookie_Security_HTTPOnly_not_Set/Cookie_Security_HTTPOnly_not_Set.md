# Cookie Security: HTTPOnly not Set

### RiskLevel : Low

![Cookie_Security_HTTPOnly_not_Set_1](/Fortify/Low/Cookie_Security_HTTPOnly_not_Set/Cookie_Security_HTTPOnly_not_Set_1.png "Cookie_Security_HTTPOnly_not_Set")

## Tips
所有主流瀏覽器均支援防止用戶端 Script 存取 Cookie 的 HttpOnly Cookie 屬性，如果未啟用 HttpOnly，則攻擊者將更容易存取使用者 Cookie，有關於 Cookie 的設定可以同時參考 **(RiskLevel : Low) Cookie Security: Cookie not Sent Over SSL**。


## 程式碼範例

### 修改前

``` C#
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

> 在 cookie 上設定 HttpOnly，防止用戶端 Script 存取 Cookie。

### 修改後

```C#
public ActionResult Login(string account, string pwd)
{
    var ticket = new FormsAuthenticationTicket(
        1,account,DateTime.Now,DateTime.Now.AddMinutes(20),false,string.Empty,FormsAuthentication.FormsCookiePath);

    var encTicket = FormsAuthentication.Encrypt(ticket);

    var cookie = new HttpCookie(FormsAuthentication.FormsCookieName, encTicket);
    cookie.Secure = true;
    cookie.HttpOnly = true;

    Response.Cookies.Add(cookie);

    return RedirectToAction("index", "default");
}
```