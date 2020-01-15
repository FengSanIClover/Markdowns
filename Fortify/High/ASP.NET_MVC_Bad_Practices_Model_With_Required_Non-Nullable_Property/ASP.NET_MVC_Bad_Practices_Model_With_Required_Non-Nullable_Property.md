# <span>ASP.NET</span> MVC Bad Practices: Model With Required Non-Nullable Property

### RiskLevel : High

![ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property_1](/Fortify/High/ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property/ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property_1.png "ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property")
![ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property_2](/Fortify/High/ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property/ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property_2.png "ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property")
![ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property_3](/Fortify/High/ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property/ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property_3.png "ASP.NET_MVC_Bad_Practices_Model_With_Required_Non-Nullable_Property")

## Tips
當類別包含 **[Required] Attribute** 的屬性，它的型別又是**不可為 Null** 的型別，當傳遞來的參數值為 Null 時，將引發意料之外的錯誤。

## 程式碼範例

### 修改前

``` C#
public partial class Policy_MetaData
{
    [Key]
    [Required]
    public System.Guid PolicyId { get; set; }

    [Required]
    [MaxLength(10)]
    public string InsuranceCompanyId { get; set; }

    [Required]
    [MaxLength(20)]
    public string ProposalNumber { get; set; }

    ...
}
```

> 將原本套用 [Required] Attribute 的 PolicyId 屬性，其型別轉為可為 Null 的型別。

### 修改後

```C#
public partial class Policy_MetaData
{
    [Key]
    [Required]
    public System.Guid? PolicyId { get; set; }

    [Required]
    [MaxLength(10)]
    public string InsuranceCompanyId { get; set; }

    [Required]
    [MaxLength(20)]
    public string ProposalNumber { get; set; }

    ...
}
```