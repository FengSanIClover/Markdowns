# Poor Error Handling: Overly Broad Catch

### RiskLevel : Low

![Poor_Error_Handling_Overly_Broad_Catch_1](/Fortify/Low/Poor_Error_Handling_Overly_Broad_Catch/Poor_Error_Handling_Overly_Broad_Catch_1.png "Poor_Error_Handling_Overly_Broad_Catch")
![Poor_Error_Handling_Overly_Broad_Catch_2](/Fortify/Low/Poor_Error_Handling_Overly_Broad_Catch/Poor_Error_Handling_Overly_Broad_Catch_2.png "Poor_Error_Handling_Overly_Broad_Catch")
![Poor_Error_Handling_Overly_Broad_Catch_3](/Fortify/Low/Poor_Error_Handling_Overly_Broad_Catch/Poor_Error_Handling_Overly_Broad_Catch_3.png "Poor_Error_Handling_Overly_Broad_Catch")

## 程式碼範例

> 此段程式是非常高層級的程式，用來判斷 token 狀態，只要有任一 exception 拋出，則代表無法通過驗證，前端接收的程式，僅判斷是否為 null 來得知是否通過驗證。

``` C#
try
{
    claimsPrincipal = handler.ValidateToken(jwtToken, validationParameters, out securityToken);
}
catch
{
    claimsPrincipal = null;
}
```

### 圖示
![Poor_Error_Handling_Overly_Broad_Catch_4](/Fortify/Low/Poor_Error_Handling_Overly_Broad_Catch/Poor_Error_Handling_Overly_Broad_Catch_4.png "Poor_Error_Handling_Overly_Broad_Catch")