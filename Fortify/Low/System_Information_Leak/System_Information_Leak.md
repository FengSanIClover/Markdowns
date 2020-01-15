# System Information Leak

### RiskLevel : Low

![System_Information_Leak_1](/Fortify/Low/System_Information_Leak/System_Information_Leak_1.png "System_Information_Leak")
![System_Information_Leak_2](/Fortify/Low/System_Information_Leak/System_Information_Leak_2.png "System_Information_Leak")

## 程式碼範例

### 修改前

``` C#
try
{
    ...
}
catch (Exception ex)
{
    byte[] data = Encoding.UTF8.GetBytes("Unhandled Error occured. Please, try again in a while.Exception Message:" + ex.ToString());
    originalResponseBody.Write(data, 0, data.Length);
}
finally
{
    ...
}
```

> 因使用 ex.ToString() 來讀取錯誤訊息，造成系統判斷有顯示除錯資訊的問題，改以其他方式記錄錯誤訊息。

### 修改後

```C#
try
{
    ...
}
catch (Exception ex)
{
    this._logger.LogError(ex, "ApiLogMiddleware Exception");

    var baseResponse = new Response();
    baseResponse.ReturnCode = ReturnCodes.CODE_FAILURE;
    baseResponse.Message = MsgCodes.Msg_99;
    baseResponse.ExceptionData = ex;

    var json = JsonConvert.SerializeObject(baseResponse);
    byte[] data = Encoding.UTF8.GetBytes(json);
    await originalBodyStream.WriteAsync(data, 0, data.Length);
}
finally
{
    ...
}
```