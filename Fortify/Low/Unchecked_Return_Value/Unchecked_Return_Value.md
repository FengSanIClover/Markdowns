# Unchecked Return Value

### RiskLevel : Low

![Unchecked_Return_Value_1](/Fortify/Low/Unchecked_Return_Value/Unchecked_Return_Value_1.png "Unchecked_Return_Value")
![Unchecked_Return_Value_2](/Fortify/Low/Unchecked_Return_Value/Unchecked_Return_Value_2.png "Unchecked_Return_Value")
![Unchecked_Return_Value_3](/Fortify/Low/Unchecked_Return_Value/Unchecked_Return_Value_3.png "Unchecked_Return_Value")

## 程式碼範例

### 修改前

``` C#
public byte[] DownloadSample()
{
    var webApiHostPath = this.hostingEnvironment.ContentRootPath;
    var filePath = Path.Combine(webApiHostPath, @"ReportTemplates\被保險人匯入匯出格式.xls");

    if (File.Exists(filePath))
    {
        using (var fileStream = new FileStream(filePath, FileMode.Open, FileAccess.Read))
        {
            var fileLength = (int)fileStream.Length;
            var result = new byte[fileLength];

            fileStream.Read(result, 0, fileLength);

            return result;
        }
    }

    return new byte[0];
}
```

> 加入對檔案串流的大小判斷。

### 修改後

```C#
public byte[] DownloadSample()
{
    var webApiHostPath = this.hostingEnvironment.ContentRootPath;
    var filePath = Path.Combine(webApiHostPath, @"ReportTemplates\被保險人匯入匯出格式.xls");

    if (File.Exists(filePath))
    {
        using (var fileStream = new FileStream(filePath, FileMode.Open, FileAccess.Read))
        {
            var fileLength = (int)fileStream.Length;
            if (fileLength > 0)
            {
                var result = new byte[fileLength];

                fileStream.Read(result, 0, fileLength);

                return result;
            }
        }
    }

    return new byte[0];
}
```