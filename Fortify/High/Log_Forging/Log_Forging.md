# Log Forging

### RiskLevel : High

![Log_Forging_1](/Fortify/High/Log_Forging/Log_Forging_1.png "Log_Forging")
![Log_Forging_2](/Fortify/High/Log_Forging/Log_Forging_2.png "Log_Forging")
![Log_Forging_3](/Fortify/High/Log_Forging/Log_Forging_3.png "Log_Forging")

## 程式碼範例

### 修改前

``` C#
public IHttpActionResult GetSignedDocument(GetSignedDocumentReq req)
{
    try
    {
        logger.Info("GetSignedDocument:" + JsonConvert.SerializeObject(req));

        if (!ModelState.IsValid)
        {
            var modelStateErrors = ModelState.Where(x => x.Value.Errors.Count > 0)
                .ToDictionary(k => k.Key, k => k.Value.Errors.Select(e => e.ErrorMessage)
                .ToArray());

            var msg = "";
            foreach (var val in modelStateErrors)
            {
                msg += string.Join(",", val.Value);
            }

            logger.Info($"ModelState.IsValid == false:{msg}");

            return Failure("要保書編號有誤!");
        }

        var fileIds = PolicyService
            .Query(x => x.ProposalNumber == req.ProposalNumber && x.Status == "Accepted")
            .OrderBy(x => x.OrderByDescending(y => y.UploadedOn))
            .Select(x => x.ProposalFileId);

        if (!fileIds.Any())
        {
            return Failure("查無要保書資料。");
        }

        byte[] bytes = null;
        foreach (var item in fileIds)
        {
            var file = FilesService.Find(item);
            if (file != null)
            {
                var filePath = Path.Combine(fileRootPath, file.Name);
                if (File.Exists(filePath))
                {
                    bytes = File.ReadAllBytes(filePath);
                    break;
                }
            }
        }

        if (bytes == null)
        {
            return Failure("查無要保書檔案。");
        }

        var signedDoc = new GetSignedDocumentResp
        {
            SignedDocument = bytes
        };

        return Success(signedDoc);
    }
    catch (Exception ex)
    {
        logger.Error(ex.ToString());

        return Failure(ex.Message);
    }
}
```

> 寫入 log 前檢核寫入內容 var jsonReq = Regex.Replace(JsonConvert.SerializeObject(req), "[^A-Za-z0-9$]", string.Empty); 

### 修改後

```C#
public IHttpActionResult GetSignedDocument(GetSignedDocumentReq req)
{
    try
    {
        var jsonReq = Regex.Replace(JsonConvert.SerializeObject(req), "[^A-Za-z0-9$]", string.Empty);
        logger.Info("GetSignedDocument:" + jsonReq);      
        
        if (!ModelState.IsValid)
        {
            var modelStateErrors = ModelState.Where(x => x.Value.Errors.Count > 0)
                .ToDictionary(k => k.Key, k => k.Value.Errors.Select(e => e.ErrorMessage)
                .ToArray());

            var msg = "";
            foreach (var val in modelStateErrors)
            {
                msg += string.Join(",", val.Value);
            }

            logger.Info($"ModelState.IsValid == false:{msg}");

            return Failure("要保書編號有誤!");
        }

        var fileIds = PolicyService
            .Query(x => x.ProposalNumber == req.ProposalNumber && x.Status == "Accepted")
            .OrderBy(x => x.OrderByDescending(y => y.UploadedOn))
            .Select(x => x.ProposalFileId);

        if (!fileIds.Any())
        {
            return Failure("查無要保書資料。");
        }

        byte[] bytes = null;
        foreach (var item in fileIds)
        {
            var file = FilesService.Find(item);
            if (file != null)
            {
                var filePath = Path.Combine(fileRootPath, file.Name);
                if (File.Exists(filePath))
                {
                    bytes = File.ReadAllBytes(filePath);
                    break;
                }
            }
        }

        if (bytes == null)
        {
            return Failure("查無要保書檔案。");
        }

        var signedDoc = new GetSignedDocumentResp
        {
            SignedDocument = bytes
        };
        
        return Success(signedDoc);
    }
    catch (Exception ex)
    {
        logger.Error(ex.ToString());

        return Failure(ex.Message);
    }
}
```