# Cross-Site Scripting: Reflected

### RiskLevel : High

![Cross-Site_Scripting_Reflected_1](/Fortify/High/Cross-Site_Scripting_Reflected/Cross-Site_Scripting_Reflected_1.png "Cross-Site_Scripting_Reflected")
![Cross-Site_Scripting_Reflected_2](/Fortify/High/Cross-Site_Scripting_Reflected/Cross-Site_Scripting_Reflected_2.png "Cross-Site_Scripting_Reflected")
![Cross-Site_Scripting_Reflected_3](/Fortify/High/Cross-Site_Scripting_Reflected/Cross-Site_Scripting_Reflected_3.png "Cross-Site_Scripting_Reflected")
![Cross-Site_Scripting_Reflected_4](/Fortify/High/Cross-Site_Scripting_Reflected/Cross-Site_Scripting_Reflected_4.png "Cross-Site_Scripting_Reflected")
![Cross-Site_Scripting_Reflected_5](/Fortify/High/Cross-Site_Scripting_Reflected/Cross-Site_Scripting_Reflected_5.png "Cross-Site_Scripting_Reflected")
![Cross-Site_Scripting_Reflected_6](/Fortify/High/Cross-Site_Scripting_Reflected/Cross-Site_Scripting_Reflected_6.png "Cross-Site_Scripting_Reflected")
![Cross-Site_Scripting_Reflected_7](/Fortify/High/Cross-Site_Scripting_Reflected/Cross-Site_Scripting_Reflected_7.png "Cross-Site_Scripting_Reflected")
![Cross-Site_Scripting_Reflected_8](/Fortify/High/Cross-Site_Scripting_Reflected/Cross-Site_Scripting_Reflected_8.png "Cross-Site_Scripting_Reflected")
![Cross-Site_Scripting_Reflected_9](/Fortify/High/Cross-Site_Scripting_Reflected/Cross-Site_Scripting_Reflected_9.png "Cross-Site_Scripting_Reflected")

## 程式碼範例

### 修改前

``` HTML
<div id="searchArea" class="row">
    <div class="col-xs-12 col-sm-10 col-md-10 col-lg-8">
        <h2>案件檢視 流水編號: @Html.Raw(ViewBag.ProposalNumber)</h2>
        <div class="form-group">
            <span class="pull-right">
                <button class="search btn" onclick="Back()">返回</button>
            </span>
        </div>
    </div>
    <div class="col-xs-12 col-sm-10 col-md-10 col-lg-8">
        @{
            Html.RenderPartial("_GetPDF", (ViewDataDictionary)ViewData);
        }
    </div>
</div>
```

> @Html.Raw() 會輸出未經 HTML 編碼的內容，正常來說，不應該太過頻繁地出現這種用法。

> 修改 @Html.Raw 的用法為 @ViewBag 。

### 修改後

```HTML
<div id="searchArea" class="row">
    <div class="col-xs-12 col-sm-10 col-md-10 col-lg-8">
        <h2>案件檢視 流水編號: @ViewBag.ProposalNumber</h2>
        <div class="form-group">
            <span class="pull-right">
                <button class="search btn" onclick="Back()">返回</button>
            </span>
        </div>
    </div>
    <div class="col-xs-12 col-sm-10 col-md-10 col-lg-8">
        @{
            Html.RenderPartial("_GetPDF", (ViewDataDictionary)ViewData);
        }
    </div>
</div>
```