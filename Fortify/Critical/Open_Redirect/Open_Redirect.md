# Open Redirect

### RiskLevel : Critical

![Open_Redirect_1](/Fortify/Critical/Open_Redirect/Open_Redirect_1.png "Open_Redirect")
![Open_Redirect_2](/Fortify/Critical/Open_Redirect/Open_Redirect_2.png "Open_Redirect")
![Open_Redirect_3](/Fortify/Critical/Open_Redirect/Open_Redirect_3.png "Open_Redirect")


## 程式碼範例

### 修改前

``` C#
public ActionResult Login(string ssoToken, string returnUrl)
{
    ViewBag.ReturnUrl = returnUrl;

    string clientIP = EnvWebHelper.GetClientIP();

    if (systemSessionService.SessionExists(clientIP, ssoToken))
    {
        SignInByToken(clientIP, ssoToken);

        return Redirect(returnUrl);
    }

    SignOut();

    return View();
}
```

> 加入 Url.IsLocalUrl(returnUrl) 來判斷是否為本機路由。

### 修改後

```C#
public ActionResult Login(string ssoToken, string returnUrl)
{
    ViewBag.ReturnUrl = returnUrl;

    string clientIP = EnvWebHelper.GetClientIP();

    if (this.systemSessionService.SessionExists(clientIP, ssoToken))
    {
        SignInByToken(clientIP, ssoToken);

        if (Url.IsLocalUrl(returnUrl))
        {
            return Redirect(returnUrl);
        }
    }

    SignOut();

    this.Dispose();

    return View();
}
```