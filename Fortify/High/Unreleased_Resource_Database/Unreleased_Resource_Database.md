# Unreleased Resource: Database

### RiskLevel : High

![Unreleased_Resource_Database_1](/Fortify/High/Unreleased_Resource_Database/Unreleased_Resource_Database_1.png "Unreleased_Resource_Database")
![Unreleased_Resource_Database_2](/Fortify/High/Unreleased_Resource_Database/Unreleased_Resource_Database_2.png "Unreleased_Resource_Database")

## Tips
資源洩漏有兩種常見原因:
1. 錯誤條件以及其他異常情況
2. 不確定程式的哪一部份負責釋放資源

攻擊者可能會藉由耗盡資源集區的方式來發動阻斷服務攻擊。

_**使用 C# 關鍵字 using 進行資源管理，即使發生 Exception，也可以確保執行 Dispose**_。

## 程式碼範例

> 使用 using 進行資料庫連線資源的管理。

``` C#
public IEnumerable<PldfDTO> GetPldfMaxVersions()
{
    using (var con = new SqlConnection(ConfigHelper.GetSKGGPCon()))
    {
        var sqlCommand = @"
SELECT*
FROM [dbo].[PLDF] AS PLDF
INNER JOIN 
(
SELECT PlanCode, MAX(Version) AS Version
FROM [dbo].[PLDF]
GROUP BY PlanCode
) AS MV
on PLDF.PlanCode=MV.PlanCode AND PLDF.Version=MV.Version
";
        return con.Query<PldfDTO>(sqlCommand);
    }
}
```