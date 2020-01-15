# <span>ASP.NET</span> MVC Bad Practices: Model With Optional and Required Properties

### RiskLevel : High

![ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties_1](/Fortify/High/ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties/ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties_1.png "ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties")
![ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties_2](/Fortify/High/ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties/ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties_2.png "ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties")
![ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties_3](/Fortify/High/ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties/ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties_3.png "ASP.NET_MVC_Bad_Practices_Model_With_Optional_and_Required_Properties")

## Tips
如果類別包含有 **[Required] Attribute** 與沒有的屬性，在攻擊者傳送的要求**所含資料多於預期時**，就會出現意料之外的錯誤。

## 程式碼範例

### 修改前

``` C#
public ActionResult CheckPeriodDate(DocumentReplenishRptReq searchModel)
{
    var startDate = searchModel.ApplyStartDate;
    var endDate = searchModel.ApplyEndDate;
    if (String.IsNullOrEmpty(startDate) || String.IsNullOrEmpty(endDate))
    {
        return Json(new Response<byte[]>(null, "L009", MessageConfigInstance.Current.L009));

    }

    if ((DateTime.Parse(endDate) - DateTime.Parse(startDate)).Days<0)
    {
        return Json(new Response<byte[]>(null, "C005", MessageConfigInstance.Current.C005));
    }

    var dayLimited = VoucherLookupInstance.Current.QueryDaysLimited[0];
    if ((DateTime.Parse(endDate) - DateTime.Parse(startDate)).Days > int.Parse(dayLimited.ToString()))
    {
        var msg = MessageConfigInstance.Current.L003;

        return Json(new Response<byte[]>(null, "L003", msg.Replace("{0}", dayLimited.ToString())));
    }

    return Json(new Response<byte[]>(null, ReturnCodes.CODE_SUCCESS, "00"));
}
```

>  使用 [Bind] 屬性來控制應將哪些參數繫結至要求中的模型物件。 

### 修改後

```C#
public ActionResult CheckPeriodDate([Bind(Include = "ApplyStartDate,ApplyEndDate")] DocumentReplenishRptReq searchModel)
{
    var startDate = searchModel.ApplyStartDate;
    var endDate = searchModel.ApplyEndDate;
    if (String.IsNullOrEmpty(startDate) || String.IsNullOrEmpty(endDate))
    {
        return Json(new Response<byte[]>(null, "L009", MessageConfigInstance.Current.L009));
    }

    if ((DateTime.Parse(endDate) - DateTime.Parse(startDate)).Days<0)
    {
        return Json(new Response<byte[]>(null, "C005", MessageConfigInstance.Current.C005));
    }

    var dayLimited = VoucherLookupInstance.Current.QueryDaysLimited[0];
    if ((DateTime.Parse(endDate) - DateTime.Parse(startDate)).Days > int.Parse(dayLimited.ToString()))
    {
        var msg = MessageConfigInstance.Current.L003;

        return Json(new Response<byte[]>(null, "L003", msg.Replace("{0}", dayLimited.ToString())));
    }

    return Json(new Response<byte[]>(null, ReturnCodes.CODE_SUCCESS, "00"));
}
```

### DocumentReplenishRptReq
```C#
[DataContract]
public class DocumentReplenishRptReq
{
    [DataMember]
    [DisplayName("申請書收件日")]
    [Required(ErrorMessage = "C008-{0} 為必填欄位")]
    public string ApplyStartDate { get; set; }

    [DataMember]
    [Required(ErrorMessage = "C008-{0} 為必填欄位")]
    public string ApplyEndDate { get; set; }

    [DataMember]
    public NameValueCollection InsuranceTypeOption { get; set; }

    ...
}
```