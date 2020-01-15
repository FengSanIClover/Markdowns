# JSON Injection

### RiskLevel : Critical

![JSON_Injection_1](/Fortify/Critical/JSON_Injection/JSON_Injection_1.png "JSON_Injection")
![JSON_Injection_2](/Fortify/Critical/JSON_Injection/JSON_Injection_2.png "JSON_Injection")
![JSON_Injection_3](/Fortify/Critical/JSON_Injection/JSON_Injection_3.png "JSON_Injection")
![JSON_Injection_4](/Fortify/Critical/JSON_Injection/JSON_Injection_4.png "JSON_Injection")

## Tips
如果 JSON 文件和訊息的語義遭攻擊者更改，且將**未經驗證的輸入**寫入至 JSON 或**未使用安全的序列化方法**來執行序列化為 JSON，就有可能引發預期之外的結果。

## 程式碼範例

### 修改前

``` C#
public OutputParams_qeruyVerifyResult VerificationLoginResult(string memberNo, string token, string verifyNo, out code returncode)
{
    string HashKey = SCBSettings.TWCA_HASH_KEY; 
    OutputParams_qeruyVerifyResult res = new OutputParams_qeruyVerifyResult();
    code _returncode = new code();

    var req = new verifyInfo()
    {
        basic = GetBasicInfo(verifyNo),
        memberNo = memberNo,
        token = token,
        identifyNo = $"{BasicInfo.businessNo}{BasicInfo.apiVersion}{BasicInfo.hashKeyNo}{BasicInfo.verifyNo}{memberNo}{token}{HashKey}".SHA256CryptoString()
    };

    var maxRetry = int.Parse(SCBSettings.TWCARetry);
    for (int retry = 0; retry < maxRetry; retry++)
    {
        var result = IdpWSClient.qeruyVerifyResult(req);

        _returncode = result.code;

        res = JsonConvert.DeserializeObject<OutputParams_qeruyVerifyResult>(result.outputParams);

        res.SetIdentify(result, result.outputParams);

        if (res.PDFSignature != null && res.PDFSignature.Any())
        {
            break;
        }
        else
        {
            if (retry == maxRetry)
            {
                Logger.Info(nameof(VerificationLoginResult), $"TWCA qeruyVerifyResult retry {retry.ToString()} fail。");
                throw new Exception($"TWCA 驗證結果重覆呼叫{retry.ToString()}次後失敗。");
            }

            Logger.Info(nameof(VerificationLoginResult), $"TWCA 驗證結果 result.outputParams:{result.outputParams}。");

            //延遲n秒，1000 = 1秒
            Thread.Sleep(1000 * int.Parse(SCBSettings.TWCASleep));
        }
    }
    returncode = _returncode;
    return res;
}
```

>調整程式序列化方法 res = JsonConvert.DeserializeObject<OutputParams_qeruyVerifyResult>(result.outputParams);

>為自訂的序列化方法 res = new JsonHelp().Deserialize<OutputParams_qeruyVerifyResult>(result.outputParams);

### 修改後

```C#
public OutputParams_qeruyVerifyResult VerificationLoginResult(string memberNo, string token, string verifyNo, out code returncode)
{
    string HashKey = SCBSettings.TWCA_HASH_KEY; 
    OutputParams_qeruyVerifyResult res = new OutputParams_qeruyVerifyResult();
    code _returncode = new code();

    var req = new verifyInfo()
    {
        basic = GetBasicInfo(verifyNo),
        memberNo = memberNo,
        token = token,
        identifyNo = $"{BasicInfo.businessNo}{BasicInfo.apiVersion}{BasicInfo.hashKeyNo}{BasicInfo.verifyNo}{memberNo}{token}{HashKey}".SHA256CryptoString()
    };

    var maxRetry = int.Parse(SCBSettings.TWCARetry);
    for (int retry = 0; retry < maxRetry; retry++)
    {
        var result = IdpWSClient.qeruyVerifyResult(req);

        _returncode = result.code;

        res = new JsonHelp().Deserialize<OutputParams_qeruyVerifyResult>(result.outputParams);

        res.SetIdentify(result, result.outputParams);

        if (res.PDFSignature != null && res.PDFSignature.Any())
        {
            break;
        }
        else
        {
            if (retry == maxRetry)
            {
                Logger.Info(nameof(VerificationLoginResult), $"TWCA qeruyVerifyResult retry {retry.ToString()} fail。");
                throw new Exception($"TWCA 驗證結果重覆呼叫{retry.ToString()}次後失敗。");
            }

            Logger.Info(nameof(VerificationLoginResult), $"TWCA 驗證結果 result.outputParams:{result.outputParams}。");

            //延遲n秒，1000 = 1秒
            Thread.Sleep(1000 * int.Parse(SCBSettings.TWCASleep));
        }
    }
    returncode = _returncode;
    return res;
}
```

### 自訂的序列化方法
```C#
public TResult Deserialize<TResult>(string strJson)
{
    var json = JsonConvert.SerializeObject(strJson);
    byte[] jsonBytes = System.Text.Encoding.UTF8.GetBytes(json);
    using (var stream = new System.IO.MemoryStream(jsonBytes))
    {
        return this.Deserialize<TResult>(stream);
    }
}
```