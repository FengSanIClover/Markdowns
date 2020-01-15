# <span>ASP.NET</span> Misconfiguration: Debug Information

### RiskLevel : Medium

![ASP.NET_Misconfiguration_Debug_Information_1](/Fortify/Medium/ASP.NET_Misconfiguration_Debug_Information/ASP.NET_Misconfiguration_Debug_Information_1.png "ASP.NET_Misconfiguration_Debug_Information")

## Tips
**除錯訊息**只能用在**開發或測試**的環境中，如果將它們部署在生產的環境中，攻擊者可能利用這些訊息得到的額外資訊，來發動攻擊。 

## 程式碼範例

### 修改前

``` HTML
<compilation debug="true" targetFramework="4.7.2">
    <assemblies>
        <add assembly="netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51" />
    </assemblies>
</compilation>
```

> 調整 Webconfig 內 compilation 的 debug 屬性為 false，停用除錯功能。

### 修改後

```HTML
<compilation debug="false" targetFramework="4.7.2">
    <assemblies>
        <add assembly="netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51" />
    </assemblies>
</compilation>
```