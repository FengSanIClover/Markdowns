# Password Management: Empty Password

### RiskLevel : High

![Password_Management_Empty_Password_1](/Fortify/High/Password_Management_Empty_Password/Password_Management_Empty_Password_1.png "Password_Management_Empty_Password")
![Password_Management_Empty_Password_2](/Fortify/High/Password_Management_Empty_Password/Password_Management_Empty_Password_2.png "Password_Management_Empty_Password")

## 程式碼範例

### 修改前

``` C#
private string _assignCertPassword = "";

public override string ToString()
{
    var jObj = new JObject();

    jObj.Add("AssignCertPassword", JToken.FromObject(_assignCertPassword));

    return jObj.ToString();
}
```

>因參數名包含 Password 而被系統誤判，修改參數名。

>變數或參數名包含 _**Key**_、_**Password**_ 等字詞容易造成系統誤判，使用上要多注意。

### 修改後

```C#
private string _assignCertVerificationCode = "";

public override string ToString()
{
    var jObj = new JObject();

    jObj.Add("AssignCertPassword", JToken.FromObject(_assignCertVerificationCode));

    return jObj.ToString();
}
```