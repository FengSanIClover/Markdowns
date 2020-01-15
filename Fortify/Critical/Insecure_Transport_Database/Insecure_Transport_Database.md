# Insecure Transport : Database

### RiskLevel : Critical

![Insecure Transport : Database](/Fortify/Critical/Insecure_Transport_Database/Insecure_Transport_Database.png "Insecure Transport : Database")

## Tips
為了避免 SQL Server 與 AP 伺服器傳輸時的內容被攔截竊取，可以**採用加密的傳輸通道**，可以增加傳輸封包安全性，但 SQL 要先啟用 SSL 憑證。

## 程式碼範例

### 修改前

``` C#
  <connectionStrings>
    <add name="XXXEntities" connectionString="metadata=res://*/Models.XXX.csdl|res://*/Models.XXX.ssdl|res://*/Models.XXX.msl;provider=System.Data.SqlClient;provider connection string='data source=&quot;資料庫來源&quot;;initial catalog=XXX;persist security info=True;user id=使用者帳號;password=使用者密碼;MultipleActiveResultSets=True;App=EntityFramework'" providerName="System.Data.EntityClient" />
  </connectionStrings>
```

> SQL 開啟 SSL 憑證，連線字串加入 Encrypt=yes。

### 修改後

```C#
  <connectionStrings>
    <add name="XXXEntities" connectionString="metadata=res://*/Models.XXX.csdl|res://*/Models.XXX.ssdl|res://*/Models.ESTAMP.msl;provider=System.Data.SqlClient;provider connection string='data source=&quot;資料庫來源&quot;;initial catalog=XXX;persist security info=True;user id=使用者帳號;password=使用者密碼;MultipleActiveResultSets=True;Encrypt=yes;App=EntityFramework'" providerName="System.Data.EntityClient" />
  </connectionStrings>
```