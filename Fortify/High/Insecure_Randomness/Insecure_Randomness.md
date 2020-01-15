# Insecure Randomness

### RiskLevel : High

![Insecure_Randomness_1](/Fortify/High/Insecure_Randomness/Insecure_Randomness_1.png "Insecure_Randomness")
![Insecure_Randomness_2](/Fortify/High/Insecure_Randomness/Insecure_Randomness_2.png "Insecure_Randomness")

## Tips
虛擬亂數產生器 (PRNG) 區分統計式與加密式，由 Next() 實作的亂數產生器屬於統計式，在安全性要求較高的環境中，會產生 **Insecure Randomness 錯誤**。 

## 程式碼範例

### 修改前

``` C#
public static string GetNewVerifyNo()
{
    Random rd = new Random(unchecked((int)DateTime.Now.Ticks));
    return rd.Next(999999999).ToString("00000000#");
}
```

> 使用運用 RNGCryptoServiceProvider(加密式的 PRNG)的方法產生亂數。

### 修改後

```C#
public static string GetNewVerifyNo()
{
    string verifyNo = RNG.Next().ToString("00000000#").Substring(0, 9);

    return verifyNo;
}
```

### 產生亂數的方法
```C#
/// <summary>
/// 使用 RNGCryptoServiceProvider 產生由密碼編譯服務供應者 (CSP) 提供的亂數產生器。
/// </summary>
public static class RNG
{
    private static RNGCryptoServiceProvider rngp = new RNGCryptoServiceProvider();
    private static byte[] rb = new byte[4];

    public static int Next()
    {
        rngp.GetBytes(rb);
        int value = BitConverter.ToInt32(rb, 0);
        if (value < 0) value = -value;
        return value;
    }
}
```