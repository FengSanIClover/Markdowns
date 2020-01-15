# Privacy Violation: Heap Inspection

### RiskLevel : High

![Privacy_Violation_Heap_Inspection_1](/Fortify/High/Privacy_Violation_Heap_Inspection/Privacy_Violation_Heap_Inspection_1.png "Privacy_Violation_Heap_Inspection")
![Privacy_Violation_Heap_Inspection_2](/Fortify/High/Privacy_Violation_Heap_Inspection/Privacy_Violation_Heap_Inspection_2.png "Privacy_Violation_Heap_Inspection")
![Privacy_Violation_Heap_Inspection_3](/Fortify/High/Privacy_Violation_Heap_Inspection/Privacy_Violation_Heap_Inspection_3.png "Privacy_Violation_Heap_Inspection")

## Tips
如果**未明確清除**未管理的記憶體緩衝區，在應用程式意外結束時 ，應用程式的記憶體傾印可能會洩露敏感資料。

## 程式碼範例

### 修改前

``` C#
public virtual IUserInfo SignIn(ILoginRequest loginRequest, string returnUrl = "")
{
    var result = Providers.Where(w => w.Value.Available == true).OrderBy(o => o.Key).Select(s => s.Value).ToList();

    if (result.Any())
    {
        foreach (IUserDataSourceProvider provider in result)
        {
            IUserInfo userInfo = provider.VerifyUser(loginRequest);
            if (userInfo != null && userInfo.VerificationPassed)
            {
                DateTime systemTime = DateTime.UtcNow;
                DateTime sessionEndTime = systemTime.AddMinutes(20);

                var userData = JsonConvert.SerializeObject(userInfo);
                var ticket = new FormsAuthenticationTicket(
                    1,userInfo.UserName,systemTime,sessionEndTime,true,userData,FormsAuthentication.FormsCookiePath);

                var encTicket = FormsAuthentication.Encrypt(ticket);
                var identity = new GenericIdentity(userInfo.UserName);
                var principal = new GenericPrincipal(identity, new string[0]);
                HttpContext.User = principal;
                Thread.CurrentPrincipal = principal;

                Response.Cookies.Add(new HttpCookie(FormsAuthentication.FormsCookieName, encTicket));
                CreateSession(userInfo, encTicket);

                return userInfo;
            }
            else
            {
                return userInfo;
            }
        }
    }

    return default(IUserInfo);
}
```

> 使用 finally 區塊來確保記憶體中的 userInfo 清除。 

### 修改後

```C#
public virtual IUserInfo SignIn(ILoginRequest loginRequest, string returnUrl = "")
{
    var result = Providers.Where(w => w.Value.Available == true).OrderBy(o => o.Key).Select(s => s.Value).ToList();

    if (result.Any())
    {
        foreach (IUserDataSourceProvider provider in result)
        {
            IUserInfo userInfo = provider.VerifyUser(loginRequest);
            try
            {
                if (userInfo != null && userInfo.VerificationPassed)
                {
                    DateTime systemTime = DateTime.UtcNow;
                    DateTime sessionEndTime = systemTime.AddMinutes(20);

                    var userData = JsonConvert.SerializeObject(userInfo);
                    var ticket = new FormsAuthenticationTicket(
                        1,userInfo.UserName,systemTime,sessionEndTime,true,userData,FormsAuthentication.FormsCookiePath);

                    var encTicket = FormsAuthentication.Encrypt(ticket);
                    var identity = new GenericIdentity(userInfo.UserName);
                    var principal = new GenericPrincipal(identity, new string[0]);
                    HttpContext.User = principal;
                    Thread.CurrentPrincipal = principal;

                    Response.Cookies.Add(new HttpCookie(FormsAuthentication.FormsCookieName, encTicket));
                    CreateSession(userInfo, encTicket);

                    return userInfo;
                }
                else
                {
                    return userInfo;
                }
            }
            finally
            {
                userInfo = null;
            }
        }
    }

    return default(IUserInfo);
}
```