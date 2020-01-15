# Privacy Violation

### RiskLevel : Critical

![Privacy_Violation_1](/Fortify/Critical/Privacy_Violation/Privacy_Violation_1.png "Privacy_Violation")
![Privacy_Violation_2](/Fortify/Critical/Privacy_Violation/Privacy_Violation_2.png "Privacy_Violation")
![Privacy_Violation_3](/Fortify/Critical/Privacy_Violation/Privacy_Violation_3.png "Privacy_Violation")

## 程式碼範例

### 修改前

``` C#
public static string AESDeCryptoString(string text, string IV, string Key)
{
    logger.Info($"AESDeCryptoString 解密(text)：{text}");
    string decrypt = "";
    try
    {
        if (string.IsNullOrEmpty(text)) return string.Empty;
        if (Key == null) throw new Exception("未將對象引用設置到對象的實例。");
        if (Key.Length < 16) throw new Exception("指定的密鑰長度不能少於16位。");
        if (Key.Length > 32) throw new Exception("指定的密鑰長度不能多於32位。");
        if (Key.Length != 16 && Key.Length != 24 && Key.Length != 32) throw new Exception("指定的密鑰長度不明確。");
        if (!string.IsNullOrEmpty(IV))
        {
            if (IV.Length < 16) throw new Exception("指定的矢量長度不能少於16位。");
        }

        using (AesCryptoServiceProvider aes = new AesCryptoServiceProvider())
        {
            byte[] key = Encoding.UTF8.GetBytes(Key);
            byte[] iv = Encoding.UTF8.GetBytes(IV);
            aes.Key = key;
            aes.IV = iv;

            byte[] dataByteArray = Convert.FromBase64String(text);
            using (MemoryStream ms = new MemoryStream())
            {
                using (CryptoStream cs = new CryptoStream(ms, aes.CreateDecryptor(), CryptoStreamMode.Write))
                {
                    cs.Write(dataByteArray, 0, dataByteArray.Length);
                    cs.FlushFinalBlock();
                    decrypt = Encoding.UTF8.GetString(ms.ToArray());
                    logger.Info($"AESDeCryptoString 解密(decrypt)：{decrypt}");
                }
            }
        }
    }
    catch (Exception e)
    {
        throw e;
    }
    return decrypt;
}
```

> 移除相關紀錄 log 的程式碼。

### 修改後

```C#
public static string AESDeCryptoString(string text, string IV, string Key)
{            
    string decrypt = "";
    try
    {
        if (string.IsNullOrEmpty(text)) return string.Empty;
        if (Key == null) throw new Exception("未將對象引用設置到對象的實例。");
        if (Key.Length < 16) throw new Exception("指定的密鑰長度不能少於16位。");
        if (Key.Length > 32) throw new Exception("指定的密鑰長度不能多於32位。");
        if (Key.Length != 16 && Key.Length != 24 && Key.Length != 32) throw new Exception("指定的密鑰長度不明確。");
        if (!string.IsNullOrEmpty(IV))
        {
            if (IV.Length < 16) throw new Exception("指定的矢量長度不能少於16位。");
        }

        using (AesCryptoServiceProvider aes = new AesCryptoServiceProvider())
        {
            byte[] key = Encoding.UTF8.GetBytes(Key);
            byte[] iv = Encoding.UTF8.GetBytes(IV);
            aes.Key = key;
            aes.IV = iv;

            byte[] dataByteArray = Convert.FromBase64String(text);
            using (MemoryStream ms = new MemoryStream())
            {
                using (CryptoStream cs = new CryptoStream(ms, aes.CreateDecryptor(), CryptoStreamMode.Write))
                {
                    cs.Write(dataByteArray, 0, dataByteArray.Length);
                    cs.FlushFinalBlock();
                    decrypt = Encoding.UTF8.GetString(ms.ToArray());                            
                }
            }
        }
    }
    catch (Exception e)
    {
        throw e;
    }
    return decrypt;
}
```