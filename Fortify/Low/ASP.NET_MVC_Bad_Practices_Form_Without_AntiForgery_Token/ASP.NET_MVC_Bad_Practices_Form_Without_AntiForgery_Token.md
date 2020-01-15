# <span>ASP.NET</span> MVC Bad Practices: Form Without AntiForgery Token

### RiskLevel : Low

![ASP.NET_MVC_Bad_Practices_Form_Without_AntiForgery_Token_1](/Fortify/Low/ASP.NET_MVC_Bad_Practices_Form_Without_AntiForgery_Token/ASP.NET_MVC_Bad_Practices_Form_Without_AntiForgery_Token_1.png "ASP.NET_MVC_Bad_Practices_Form_Without_AntiForgery_Token")
![ASP.NET_MVC_Bad_Practices_Form_Without_AntiForgery_Token_1](/Fortify/Low/ASP.NET_MVC_Bad_Practices_Form_Without_AntiForgery_Token/ASP.NET_MVC_Bad_Practices_Form_Without_AntiForgery_Token_1.png "ASP.NET_MVC_Bad_Practices_Form_Without_AntiForgery_Token")

## Tips
<span>ASP.NET</span> MVC 在表單中**新增防偽造權杖**，並在控制項中對該**防偽造權杖進行驗證**，從而更好地保護應用程式**抵禦跨網站偽造要求攻擊**，若應用程式使用表單傳送從使用者資訊，請考慮使用內建的防偽造保護機制。

## 程式碼範例

### 修改前(前端)

``` C#
@using (Html.BeginForm("Create", "VoucherChangeEdit", new { area = "ELicense" }, FormMethod.Post, new { @class = "form-horizontal", id = "form_voucherChange_Create" }))
{
    <div class="form-group">
        @Html.LabelFor(x => x.ID, new { @class = "control-label col-md-2" })
        <div class="col-md-2">
            @Html.TextBoxFor(x => x.ID, new { @class = "form-control" })
        </div>
        <div class="col-md-2">
            <a href="javascript:moveToPersonnelDetail()"><p class="form-control-static">人員資料異動</p></a>
        </div>
        @Html.LabelFor(x => x.PayType, new { @class = "control-label col-md-2" })
    </div>

    ...
}
```

> 在 @using 區塊內新增 @Html.AntiForgeryToken()，以新增防偽造權杖。

### 修改後(前端)

```HTML
@using (Html.BeginForm("Create", "VoucherChangeEdit", new { area = "ELicense" }, FormMethod.Post, new { @class = "form-horizontal", id = "form_voucherChange_Create" }))
{                
    @Html.AntiForgeryToken()
    <div class="form-group">
        @Html.LabelFor(x => x.ID, new { @class = "control-label col-md-2" })
        <div class="col-md-2">
            @Html.TextBoxFor(x => x.ID, new { @class = "form-control" })
        </div>
        <div class="col-md-2">
            <a href="javascript:moveToPersonnelDetail()"><p class="form-control-static">人員資料異動</p></a>
        </div>
        @Html.LabelFor(x => x.PayType, new { @class = "control-label col-md-2" })
    </div>

    ...
}
```

### 修改前(後端)
```C#
[HttpPost]
public override ActionResult Index_Read(DataTablesRequest dtRequest, SystemParameterReq searchModel)
{
    SearchModelSessionHelper.SetCurrentDataAndPage<SystemParameterReq>(searchModel, (int)dtRequest.start);

    var result = ((ISystemParameterService)service).GetAll(searchModel);
    var grid = new DataTablesResponse<SystemParameterResp>(dtRequest, result);

    return Json(grid);
}
```

>接收提交表單的 Controller Action 需要使用 [ValidateAntiForgeryToken] Attribute 驗證防偽造權杖。


### 修改後(後端)
```C#
[HttpPost]
[ValidateAntiForgeryToken]
public override ActionResult Index_Read(DataTablesRequest dtRequest, SystemParameterReq searchModel)
{
    SearchModelSessionHelper.SetCurrentDataAndPage<SystemParameterReq>(searchModel, (int)dtRequest.start);

    var result = ((ISystemParameterService)service).GetAll(searchModel);
    var grid = new DataTablesResponse<SystemParameterResp>(dtRequest, result);

    return Json(grid);
}
```