# Often Misused: File Upload

### RiskLevel : Medium

![Often_Misused_File_Upload_1](/Fortify/Medium/Often_Misused_File_Upload/Often_Misused_File_Upload_1.png "Often_Misused_File_Upload")

## Tips
若程式功能為可接受**上傳檔案**進行資料處理，必須進行**檔案名稱、副檔名及檔案內容**的檢查，以確保上傳的檔案符合預期並可供應用程式使用。

## 程式碼範例

### 前端檢核

### 修改前

```JavaScript
getFileName(event) {
if (!event.target.value) return;

this.fileName = (<HTMLInputElement>event.target).files[0].name;
}
```

> 加入檢核副檔名。

### 修改後

```JavaScript
getFileName(event) {
    if (!event.target.value) return;

    var validExts = new Array(".xls");

    var fileExt = event.target.value;
    fileExt = fileExt.substring(fileExt.lastIndexOf("."));

    if (validExts.indexOf(fileExt) < 0) {
        SweetAlertService.error(
        `檔案類型錯誤，可接受的副檔名：${validExts.toString()}`
        );
        this.fileInput.nativeElement.value = null;
    } else {
        this.fileName = (<HTMLInputElement>event.target).files[0].name;
    }
}
```

### 後端檢核

> 檢核上傳的檔案內容符合預期的 ProposalInsurantImportBM 。

```C#
public string ImportExcelToProposalInsurant(
    string proposalNo, IFormFile fileData, out List<ProposalInsurantImportBM> importVals)
{
    var excelHelper = new ExcelHelper();
    importVals = excelHelper.ReadExcel<ProposalInsurantImportBM>(fileData).Result.ToList();
}
```