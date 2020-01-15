# <span>ASP.NET</span> MVC Bad Practices: Optional Submodel With Required Property

### RiskLevel : High

![ASP.NET_MVC_Bad_Practices_Optional_Submodel_With_Required_Property_1](/Fortify/High/ASP.NET_MVC_Bad_Practices_Optional_Submodel_With_Required_Property/ASP.NET_MVC_Bad_Practices_Optional_Submodel_With_Required_Property_1.png "ASP.NET_MVC_Bad_Practices_Optional_Submodel_With_Required_Property")
![ASP.NET_MVC_Bad_Practices_Optional_Submodel_With_Required_Property_2](/Fortify/High/ASP.NET_MVC_Bad_Practices_Optional_Submodel_With_Required_Property/ASP.NET_MVC_Bad_Practices_Optional_Submodel_With_Required_Property_2.png "ASP.NET_MVC_Bad_Practices_Optional_Submodel_With_Required_Property")

## Tips
若類別的屬性中有 **[Required] 的 Arrtibute** ，且引用這**類別作為屬性之一**的父類別，沒在該屬性加上 [Required] 的 Arrtibute，在使用上就容易引發意外的錯誤。

## 程式碼範例

### 修改前

``` C#
public class ContractBM
{
    public string PreEffectiveDate { get; set; }

    public ProposalCustomerBM ProposalCustomer { get; set; }

    public List<ProposalPackageBM> ProposalPackage { get; set; }

    public List<ProposalPackageAmountBM> ProposalPackageAmount { get; set; }

    public List<PldfList> Pldf { get; set; }

    public List<PldfSettingBM> PldfSetting { get; set; }
}
```

> 因 ProposalCustomerBM 有使用 [Required] Arrtibute 的屬性，在引用的屬性加上 [Required] Arrtibute。

### 修改後

```C#
public class ContractBM
{
    public string PreEffectiveDate { get; set; }

    [Required]
    public ProposalCustomerBM ProposalCustomer { get; set; }

    public List<ProposalPackageBM> ProposalPackage { get; set; }

    public List<ProposalPackageAmountBM> ProposalPackageAmount { get; set; }

    public List<PldfList> Pldf { get; set; }

    public List<PldfSettingBM> PldfSetting { get; set; }
}
```

### ProposalCustomerBM
```C#
public class ProposalCustomerBM
{
    public string CustBan { get; set; }

    public string ServiceItem { get; set; }

    [Required]
    public string CustName { get; set; }

    [Required]
    public int Oiiid { get; set; }

    [Required]
    public string FixedEmployer { get; set; }

    ...
}
```