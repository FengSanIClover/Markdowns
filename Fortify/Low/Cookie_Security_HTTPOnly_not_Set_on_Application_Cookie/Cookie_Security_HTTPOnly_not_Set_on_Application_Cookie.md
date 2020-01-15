# Cookie Security: HTTPOnly not Set on Application Cookie

### RiskLevel : Low

![Cookie_Security_HTTPOnly_not_Set_on_Application_Cookie_1](/Fortify/Low/Cookie_Security_HTTPOnly_not_Set_on_Application_Cookie/Cookie_Security_HTTPOnly_not_Set_on_Application_Cookie_1.png "Cookie_Security_HTTPOnly_not_Set_on_Application_Cookie")

## 程式碼範例

### 修改前

``` HTML
<system.web>
    <sessionState mode="InProc" cookieless="false" timeout="15" />
    <httpCookies requireSSL="true" />
</system.web>
```

> 加入 httpOnlyCookies 屬性並設為 true，表示不可透過用戶端 script 來存取 cookie。

### 修改後

```HTML
<system.web>
    <sessionState mode="InProc" cookieless="false" timeout="15" />
    <httpCookies requireSSL="true" httpOnlyCookies="true"/>
</system.web>
```