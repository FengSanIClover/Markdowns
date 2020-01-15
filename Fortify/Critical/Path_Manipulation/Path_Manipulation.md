# Path Manipulation

### RiskLevel : Critical

![Path_Manipulation_1](/Fortify/Critical/Path_Manipulation/Path_Manipulation_1.png "Path_Manipulation")
![Path_Manipulation_2](/Fortify/Critical/Path_Manipulation/Path_Manipulation_2.png "Path_Manipulation")

## 程式碼範例

### 修改前

``` C#
string file_pdf_ProposalPdfFileName = $"要保書{req.ProposalNumber}{SystemTime.ToString("yyyyMMddHHmmss")}.pdf";

string file_pdf_ProposalPdfFilePath = Path.Combine(pdf_save_path, file_pdf_ProposalPdfFileName);

BinaryWriter file_pdf_ProposalPdfFile_Writer = 
    new BinaryWriter(File.Open(file_pdf_ProposalPdfFilePath, FileMode.Create, FileAccess.Write, FileShare.None));
```

> 加入路徑的白名單檢查機制 !WhitelistHelp.Check(file_pdf_ProposalPdfFilePath, 1) 。

### 修改後

```C#
string file_pdf_ProposalPdfFileName = $"要保書{req.ProposalNumber}{SystemTime.ToString("yyyyMMddHHmmss")}.pdf";
string file_pdf_ProposalPdfFilePath = Path.Combine(pdf_save_path, file_pdf_ProposalPdfFileName);

if (!WhitelistHelp.Check(file_pdf_ProposalPdfFilePath, 1))
{
    return Failure($"要保書PDF檔案存取路徑有誤:{file_pdf_ProposalPdfFilePath}，請聯絡IT人員協助處理。");
}

BinaryWriter file_pdf_ProposalPdfFile_Writer = 
    new BinaryWriter(File.Open(file_pdf_ProposalPdfFilePath, FileMode.Create, FileAccess.Write, FileShare.None));
```

### 白名單檢查機制的方法
```C#
public static bool Check(string path, int type)
{
    bool check = true;

    List<string> whitelist = new List<string>();

    //檔案存取路徑(根目錄)
    whitelist.Add(SCBSettings.File_Root_Path.ToLower());

    //今天日期
    whitelist.Add(DateTime.Now.ToString("yyyyMMdd"));
    
    //副檔名
    whitelist.Add(".pdf");

    //將路徑全轉為小寫，再做比對
    string _path = path.ToLower();

    switch (type)
    {
        case 1:
            {
                if (!_path.Contains("要保書"))
                {
                    check = false;
                }
                break;
            }
        case 2:
            {
                if (!_path.Contains("銀行文件"))
                {
                    check = false;
                }
                break;
            }
    }

    if (check)
    {
        foreach (string item in whitelist)
        {
            if (!_path.Contains(item))
            {
                check = false;
                break;
            }
        }
    }

    return check;
}
```