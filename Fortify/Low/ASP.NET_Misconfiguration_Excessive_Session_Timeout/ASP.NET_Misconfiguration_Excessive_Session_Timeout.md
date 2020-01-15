# <span>ASP.NET</span> Misconfiguration: Excessive Session Timeout

### RiskLevel : Low

![ASP.NET_Misconfiguration_Excessive_Session_Timeout_1](/Fortify/Low/ASP.NET_Misconfiguration_Excessive_Session_Timeout/ASP.NET_Misconfiguration_Excessive_Session_Timeout_1.png "ASP.NET_Misconfiguration_Excessive_Session_Timeout")
![ASP.NET_Misconfiguration_Excessive_Session_Timeout_2](/Fortify/Low/ASP.NET_Misconfiguration_Excessive_Session_Timeout/ASP.NET_Misconfiguration_Excessive_Session_Timeout_2.png "ASP.NET_Misconfiguration_Excessive_Session_Timeout")

## Tips
Authentication **逾時時間過長**會讓攻擊者獲得較多的時間**破解使用者密碼、無線加密金鑰**，如果足夠大量的階段作業正在執行，也會導致無法釋放記憶體，最後造成**阻斷服務**，系統預設逾時時間為 **30** 分鐘。

## 程式碼範例

### 修改前

``` HTML
<authentication mode="Forms">
    <forms loginUrl="~/Login" timeout="20" defaultUrl="~/Home/Index" />
</authentication>
```

> 修改設定 Authentication 逾時時間為 15 分鐘。

### 修改後

```HTML
<authentication mode="Forms">
    <forms loginUrl="~/Login" timeout="15" defaultUrl="~/Home/Index" />
</authentication>
```