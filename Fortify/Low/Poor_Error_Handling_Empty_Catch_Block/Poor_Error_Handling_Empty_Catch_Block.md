# Poor Error Handling: Empty Catch Block

### RiskLevel : Low

![Poor_Error_Handling_Empty_Catch_Block_1](/Fortify/Low/Poor_Error_Handling_Empty_Catch_Block/Poor_Error_Handling_Empty_Catch_Block_1.png "Poor_Error_Handling_Empty_Catch_Block")
![Poor_Error_Handling_Empty_Catch_Block_2](/Fortify/Low/Poor_Error_Handling_Empty_Catch_Block/Poor_Error_Handling_Empty_Catch_Block_2.png "Poor_Error_Handling_Empty_Catch_Block")

## 程式碼範例

### 修改前

```C#
private static Assembly CurrentDomain_AssemblyResolve(object sender, ResolveEventArgs args)
{
    var requestedAssembly = new AssemblyName(args.Name);
    Assembly assembly = null;
    AppDomain.CurrentDomain.AssemblyResolve -= CurrentDomain_AssemblyResolve;

    try
    {
        assembly = Assembly.Load(requestedAssembly.Name);
    }
    catch (Exception ex)
    {
    }

    AppDomain.CurrentDomain.AssemblyResolve += CurrentDomain_AssemblyResolve;

    return assembly;
}
```

> 在未處理的 catch 區塊加入 throw 將例外拋出。

### 修改後

```C#
private static Assembly CurrentDomain_AssemblyResolve(object sender, ResolveEventArgs args)
{
    var requestedAssembly = new AssemblyName(args.Name);
    Assembly assembly = null;
    AppDomain.CurrentDomain.AssemblyResolve -= CurrentDomain_AssemblyResolve;

    try
    {
        assembly = Assembly.Load(requestedAssembly.Name);
    }
    catch (Exception ex)
    {
        throw ex;
    }

    AppDomain.CurrentDomain.AssemblyResolve += CurrentDomain_AssemblyResolve;

    return assembly;
}
```