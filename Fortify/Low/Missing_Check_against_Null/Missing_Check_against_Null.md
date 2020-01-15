# Missing Check against Null

### RiskLevel : Low

![Missing_Check_against_Null_1](/Fortify/Low/Missing_Check_against_Null/Missing_Check_against_Null_1.png "Missing_Check_against_Null")
![Missing_Check_against_Null_2](/Fortify/Low/Missing_Check_against_Null/Missing_Check_against_Null_2.png "Missing_Check_against_Null")

## Tips
當方法參數或方法回傳值為物件型別時，如果**使用該物件前**沒有做**是否為 Null 值**的檢查，那麼在使用時就有可能發生意外的錯誤。

## 程式碼範例

### 修改前

``` C#
public string[] GetCsvTitle(IFormFile fileData,int titleCount = 1)
{
    using (var streamReader =
        new StreamReader(fileData.OpenReadStream(), Encoding.GetEncoding(950), true))
    {
        int count = 1;
        while (count != titleCount)
        {
            streamReader.ReadLine();
            count += 1;
        }

        return streamReader.ReadLine().Split(',');
    }
}
```

> 在使用 Split() 方法分割字串前，加入是否為 Null 值的判斷。

### 修改後

```C#
public string[] GetCsvTitle(IFormFile fileData,int titleCount = 1)
{
    using (var streamReader =
        new StreamReader(fileData.OpenReadStream(), Encoding.GetEncoding(950), true))
    {
        int count = 1;
        while (count != titleCount)
        {
            streamReader.ReadLine();
            count += 1;
        }

        var title = streamReader.ReadLine();

        if (string.IsNullOrEmpty(title))
            throw new BusinessException("", "無法取得檔案標頭");

        return title.Split(',');
    }
}
```