# Password Management: Password in Configuration File

### RiskLevel : High

![Password_Management_Password_in_Configuration_File_1](/Fortify/High/Password_Management_Password_in_Configuration_File/Password_Management_Password_in_Configuration_File_1.png "Password_Management_Password_in_Configuration_File")

## 程式碼範例

### 修改前

``` C#
<connectionStrings>
<add name="XXXEntities" connectionString="metadata=res://*/Models.XXX.csdl|res://*/Models.XXX.ssdl|res://*/Models.XXX.msl;provider=System.Data.SqlClient;provider connection string='data source=&quot;資料庫來源&quot;;initial catalog=XXX;persist security info=True;user id=使用者Id;password=使用者密碼;MultipleActiveResultSets=True;App=EntityFramework'" providerName="System.Data.EntityClient" />
</connectionStrings>
```

> 將設定檔中**純文字**的連線字串修改為經過**加密後**的連線字串。

### 修改後

```C#
<connectionStrings>
<add name="XXXEntities" connectionString="dllad4YJGbrTrnzz4pa89zwmRr2Glx7kEPEo+mdYEtkU2ETS2FwS8BAjIsLW6QqIC2nhUSdHgFILUAwHfl0uQEw4PtXOUyOKaCVoly/r+d5MbxNjG/peoLmAMHqI+ACkpKRTJB0E4P1XajfDmvWyXAjT1p08+85TCkkuJJbqLS3mf+/caTj57lZex2J0V/AdYBpOS2dkbQa7K2fPr6P/0rPhBU1wmPAy9g+Kq/AdIyuF4B8dmR6xesktl3qT5Svl9YxBaEVkz9+EjQIK/OkfO/HzfQKJSRhj4F6xt6j06eSqEtRAjkVrOzPq9i0Y3/JxiReNURdU2XVO4wb0fH8KCghsV64nAcYwc7G378qtD5j5D0MiFXzfy4vIB1RZIgyffvl9f8VmyBg4usLUVc3RCE1lnBRKIx5KFYznoY53sTw=" providerName="System.Data.EntityClient" />
</connectionStrings>
```