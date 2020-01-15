# Code Correctness: Missing [Serializable] Attribute

### RiskLevel : Low

![Code_Correctness_Missing_Serializable_Attribute_1](/Fortify/Low/Code_Correctness_Missing_Serializable_Attribute/Code_Correctness_Missing_Serializable_Attribute_1.png "Code_Correctness_Missing_Serializable_Attribute")

## Tips
.NET 執行期間將允許任何 declare [Serializable] Attribute 屬性的物件進行序列化，若類別**要求自訂序列化方法**，就必須**繼承 ISerializable 介面**，不過類別仍然**必須 declare [Serializable] Attribute**。 

## 程式碼範例

### 修改前

``` C#
public class BusinessException : Exception
{
    public string ErrorMsg { get; set; }
    public string ErrorCode { get; set; }

    public BusinessException(string errorCode = "", string errorMsg = "")
    {
        this.ErrorMsg = errorMsg;
        this.ErrorCode = errorCode;
    }
}
```

> 因 BusinessException 繼承 Exception 而 Exception 又繼承 ISerializable，因此 BusinessException 必須 declare [Serializable] Attribute 才可以進行序列化。

### 修改後

```C#
[Serializable]
public class BusinessException : Exception
{
    public string ErrorMsg { get; set; }
    public string ErrorCode { get; set; }

    public BusinessException(string errorCode = "", string errorMsg = "")
    {
        this.ErrorMsg = errorMsg;
        this.ErrorCode = errorCode;
    }
}
```