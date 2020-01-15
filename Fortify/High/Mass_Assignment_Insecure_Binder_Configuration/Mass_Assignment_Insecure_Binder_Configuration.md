# Mass Assignment: Insecure Binder Configuration

### RiskLevel : High

![Mass_Assignment_Insecure_Binder_Configuration_1](/Fortify/High/Mass_Assignment_Insecure_Binder_Configuration/Mass_Assignment_Insecure_Binder_Configuration_1.png "Mass_Assignment_Insecure_Binder_Configuration")
![Mass_Assignment_Insecure_Binder_Configuration_2](/Fortify/High/Mass_Assignment_Insecure_Binder_Configuration/Mass_Assignment_Insecure_Binder_Configuration_2.png "Mass_Assignment_Insecure_Binder_Configuration")
![Mass_Assignment_Insecure_Binder_Configuration_3](/Fortify/High/Mass_Assignment_Insecure_Binder_Configuration/Mass_Assignment_Insecure_Binder_Configuration_3.png "Mass_Assignment_Insecure_Binder_Configuration")
![Mass_Assignment_Insecure_Binder_Configuration_4](/Fortify/High/Mass_Assignment_Insecure_Binder_Configuration/Mass_Assignment_Insecure_Binder_Configuration_4.png "Mass_Assignment_Insecure_Binder_Configuration")
![Mass_Assignment_Insecure_Binder_Configuration_5](/Fortify/High/Mass_Assignment_Insecure_Binder_Configuration/Mass_Assignment_Insecure_Binder_Configuration_5.png "Mass_Assignment_Insecure_Binder_Configuration")
![Mass_Assignment_Insecure_Binder_Configuration_6](/Fortify/High/Mass_Assignment_Insecure_Binder_Configuration/Mass_Assignment_Insecure_Binder_Configuration_6.png "Mass_Assignment_Insecure_Binder_Configuration")
![Mass_Assignment_Insecure_Binder_Configuration_7](/Fortify/High/Mass_Assignment_Insecure_Binder_Configuration/Mass_Assignment_Insecure_Binder_Configuration_7.png "Mass_Assignment_Insecure_Binder_Configuration")
![Mass_Assignment_Insecure_Binder_Configuration_8](/Fortify/High/Mass_Assignment_Insecure_Binder_Configuration/Mass_Assignment_Insecure_Binder_Configuration_8.png "Mass_Assignment_Insecure_Binder_Configuration")
![Mass_Assignment_Insecure_Binder_Configuration_9](/Fortify/High/Mass_Assignment_Insecure_Binder_Configuration/Mass_Assignment_Insecure_Binder_Configuration_9.png "Mass_Assignment_Insecure_Binder_Configuration")

## 程式碼範例

### 修改前

``` C#
public class order
{
    public int column { get; set; }

    public string dir { get; set; }

    public string columnName { get; set; }
}
```

> 加入 ASP<span>.</span>NET MVC 模型繫結程序，以 [DataContract] Attribute 裝飾類別，繫結時就會忽略沒有 [DataMember] Attribute 的屬性。

### 修改後

```C#
[DataContract]
public class order
{
    [DataMember]
    public int column { get; set; }

    [DataMember]
    public string dir { get; set; }

    [DataMember]
    public string columnName { get; set; }
}
```