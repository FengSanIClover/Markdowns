# Dead Code: Unused Field

### RiskLevel : Low

![Dead_Code_Unused_Field_1](/Fortify/Low/Dead_Code_Unused_Field/Dead_Code_Unused_Field_1.png "Dead_Code_Unused_Field")

## Tips
程式中存在**從未使用到**的變數或類別欄位，此項錯誤可能發生於開發階段因需求或寫法變更，導致原有的變數或類別欄位不再需要，且未將之移除而造成的。

## 程式碼範例

> 變數 ageRegex 有在下方的方法中使用到，並非 Unused Field，此為弱掃系統誤判的範例。

```C#
if (errorMsgAgeFormat == string.Empty)
{
    var ageRegex = new Regex(@"^[\d]{0,2}$");

    importResult.ErrorMessage += this.ValidRegx(
        ageRegex, importResult.InsurantAge, "年齡為正整數且長度不可超過 2 bytes;");
}
```
