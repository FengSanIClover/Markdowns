# Null Dereference

### RiskLevel : High

![Null Dereference](/Fortify/High/Null_Dereference/Null_Dereference.png "Null Dereference")

## Tips
當方法參數或方法回傳值為物件型別時，如果**使用該物件前**沒有做**是否為 Null 值**的檢查，那麼在使用時就有可能發生意外的錯誤。

## 程式碼範例

### 修改前

``` C#
public async Task<byte[]> ExportErrorReportAsync(IFormFile file, List<GrvfCSVBM> grvfReport)
{
    var csvHelper = new CSVHelper();
    var titles = csvHelper.GetCsvTitle(file);
    var excelHelper = new ExcelHelper(1, "團險費率檔");

    for (int i = 0; i < titles.Length; i += 1)
    {
        excelHelper.CreateCellAndSetValue(0, i, titles[i], false);
    }

    excelHelper.CreateCellAndSetValue(0, titles.Length, "錯誤原因", true);
    excelHelper.WriteData(this.mapper.Map<List<GrvfExcelBM>>(grvfReport), 0, 0, true);

    return await excelHelper.SaveExcel();
}
```

> 在使用 file 與 grvfReport 物件參數前加入 Null 值的檢查。

### 修改後

```C#
public async Task<byte[]> ExportErrorReportAsync(IFormFile file, List<GrvfCSVBM> grvfReport)
{

    if (file == null)
    {
        throw new BusinessException("", "csv檔案為空值(Null)。");
    }

    if (grvfReport == null)
    {
        throw new BusinessException("", "grvf報告為空值(Null)。");
    }

    var csvHelper = new CSVHelper();
    var titles = csvHelper.GetCsvTitle(file);
    var excelHelper = new ExcelHelper(1, "團險費率檔");

    for (int i = 0; i < titles.Length; i += 1)
    {
        excelHelper.CreateCellAndSetValue(0, i, titles[i], false);
    }

    excelHelper.CreateCellAndSetValue(0, titles.Length, "錯誤原因", true);
    excelHelper.WriteData(this.mapper.Map<List<GrvfExcelBM>>(grvfReport), 0, 0, true);

    return await excelHelper.SaveExcel();
}
```