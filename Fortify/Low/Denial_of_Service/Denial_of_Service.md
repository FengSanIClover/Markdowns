# Denial of Service

### RiskLevel : Low

![Denial_of_Service_1](/Fortify/Low/Denial_of_Service/Denial_of_Service_1.png "Denial_of_Service")
![Denial_of_Service_2](/Fortify/Low/Denial_of_Service/Denial_of_Service_2.png "Denial_of_Service")

## Tips
攻擊者發送要求**超載應用程式的錯誤**或**消耗大量記憶體**，讓程式花費更多時間來執行記憶體回收或者在一些後續作業中**耗盡記憶體**，必須**驗證使用者輸入**以確保它不會造成不適當地資源利用。

## 程式碼範例

``` C#
private async Task<string> FetchResponseBody(HttpResponse response)
{
    response.Body.Seek(0, SeekOrigin.Begin);
    var bodyAsText = await new StreamReader(response.Body).ReadToEndAsync();
    response.Body.Seek(0, SeekOrigin.Begin);

    return bodyAsText;
}
```

> 限制可讀取各筆檔案內容的長度，IIS 已經有預設的上傳大小限制為 30000000 bytes，約 30MB。

### 參考圖示

![Denial_of_Service_3](/Fortify/Low/Denial_of_Service/Denial_of_Service_3.png "Denial_of_Service")