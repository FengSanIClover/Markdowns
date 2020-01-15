# <span>ASP.NET</span> MVC Bad Practices: Controller Action Without AntiForgery Validation

### RiskLevel : Low

![ASP.NET_MVC_Bad_Practices_Controller_Action_Without_AntiForgery_Validation_1](/Fortify/Low/ASP.NET_MVC_Bad_Practices_Controller_Action_Without_AntiForgery_Validation/ASP.NET_MVC_Bad_Practices_Controller_Action_Without_AntiForgery_Validation_1.png "ASP.NET_MVC_Bad_Practices_Controller_Action_Without_AntiForgery_Validation")
![ASP.NET_MVC_Bad_Practices_Controller_Action_Without_AntiForgery_Validation_2](/Fortify/Low/ASP.NET_MVC_Bad_Practices_Controller_Action_Without_AntiForgery_Validation/ASP.NET_MVC_Bad_Practices_Controller_Action_Without_AntiForgery_Validation_2.png "ASP.NET_MVC_Bad_Practices_Controller_Action_Without_AntiForgery_Validation")

## Tips
<span>ASP.NET</span> MVC 在表單中**新增防偽造權杖**，並在控制項中對該**防偽造權杖進行驗證**，從而更好地保護應用程式**抵禦跨網站偽造要求攻擊**，若應用程式使用表單傳送從使用者資訊，請考慮使用內建的防偽造保護機制。

## 程式碼範例

### 修改前(前端)

``` HTML
<script>
$(document).ready(function () {
    pageSetUp();
});
</script>

@if (@Model is HelperResult)
{
    @Model
}
```

> 加入對 HtmlHelper.AntiForgeryToken() 的呼叫，以新增防偽造權杖。

### 修改後(前端)

```HTML
<script>
$(document).ready(function () {
    pageSetUp();
    SetAntiForgeryToken();
});
</script>

@if (@Model is HelperResult)
{
    @Model
}

<div id="AntiForgeryToken">@Html.AntiForgeryToken()</div>
```

### SetAntiForgeryToken()

```JavaScript
function SetAntiForgeryToken() {
    $(document).ajaxSend(function (event, request, opt) {
        request.setRequestHeader("RequestVerificationToken", $('#AntiForgeryToken').children().val()); 
        opt.data += '&__RequestVerificationToken=' + GetAntiForgeryToken();
    });
}
```

### GetAntiForgeryToken()
```JavaScript
function GetAntiForgeryToken() {
    return $('#AntiForgeryToken').children().val();
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