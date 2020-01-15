# Unreleased Resource: Streams

### RiskLevel : High

![Unreleased_Resource_Streams_1](/Fortify/High/Unreleased_Resource_Streams/Unreleased_Resource_Streams_1.png "Unreleased_Resource_Streams")
![Unreleased_Resource_Streams_2](/Fortify/High/Unreleased_Resource_Streams/Unreleased_Resource_Streams_2.png "Unreleased_Resource_Streams")

## Tips
資源洩漏有兩種常見原因:
1. 錯誤條件以及其他異常情況
2. 不確定程式的哪一部份負責釋放資源

攻擊者可能會藉由耗盡資源集區的方式來發動阻斷服務攻擊。

_**使用 C# 關鍵字 using 進行資源管理，即使發生 Exception，也可以確保執行 Dispose**_。

## 程式碼範例

### 修改前

``` C#
public void LoadSheet()
{
    var templateFile = new FileStream(templatePath, FileMode.Open, FileAccess.Read);
    Workbook = new XSSFWorkbook(templateFile);
    Sheet = Workbook.GetSheetAt(0);
}
```

> 使用 using 進行串流資源的管理。

### 修改後

```C#
public void LoadSheet()
{
    using (var templateFile = new FileStream(templatePath, FileMode.Open, FileAccess.Read)) // 開啟讀取樣版檔
    {
        Workbook = new XSSFWorkbook(templateFile);
        Sheet = Workbook.GetSheetAt(0);
    }     
}
```