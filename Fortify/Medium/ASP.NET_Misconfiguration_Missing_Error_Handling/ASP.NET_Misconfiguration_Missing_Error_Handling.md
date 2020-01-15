# <span>ASP.NET</span> Misconfiguration: Missing Error Handling

### RiskLevel : Medium

![ASP.NET_Misconfiguration_Missing_Error_Handling_1](/Fortify/Medium/ASP.NET_Misconfiguration_Missing_Error_Handling/ASP.NET_Misconfiguration_Missing_Error_Handling_1.png "ASP.NET_Misconfiguration_Missing_Error_Handling")

## Tips
<span>ASP.NET</span> 應用程式**預設**的錯誤頁面，會對所發生的錯誤**提供詳細資訊**，不應在生產的環境中使用，攻擊者可能利用錯誤頁面所提供的額外資訊來發動攻擊。 

## 程式碼範例

### 修改前

``` HTML
<customErrors mode="On" />
```

> 根據狀態碼 404 導頁至指定頁面。

### 修改後

```HTML
<customErrors mode="On">
    <error statusCode="404" redirect="~/home/notfound" />
</customErrors>
```