# Hidden Field

### RiskLevel : Low

![Hidden_Field_1](/Fortify/Low/Hidden_Field/Hidden_Field_1.png "Hidden_Field")

## Tips
**避免於隱藏欄位存放使用者資訊**，並預期使用者無法檢視或竄改內容，攻擊者會檢查寫入隱藏欄位的值並加以修改，或以攻擊資料取代這些內容。 

## 程式碼範例

### 修改前

``` HTML
<input name="@fieldName" type="hidden" value="@falseString">
```

> 使用 @Html.Hidden 取代 \<input type="hidden"> 來隱藏欄位。


### 修改後

```HTML
@Html.Hidden(fieldName, falseString)
```