# Poor Logging Practice: Use of a System Output Stream

### RiskLevel : Low

![Poor_Logging_Practice_Use_of_a_System_Output_Stream_1](/Fortify/Low/Poor_Logging_Practice_Use_of_a_System_Output_Stream/Poor_Logging_Practice_Use_of_a_System_Output_Stream_1.png "Poor_Logging_Practice_Use_of_a_System_Output_Stream")
![Poor_Logging_Practice_Use_of_a_System_Output_Stream_2](/Fortify/Low/Poor_Logging_Practice_Use_of_a_System_Output_Stream/Poor_Logging_Practice_Use_of_a_System_Output_Stream_2.png "Poor_Logging_Practice_Use_of_a_System_Output_Stream")

## Tips
如果需要紀錄系統資訊，建議使用有結構化的紀錄工具，避免使用**Console.WriteLine 、 Console.Error** 等，將來可能會在轉向結構化記錄時發生錯誤。

## 程式碼範例

### 修改前

``` C#
public void AnalysisGroupCalcEngine()
{  
    var calcDefines = this.calcGroupRepo.Queryable()
        .Where(g => g.GroupId == "Group_All")
        .Join(this.calcDefineRepo.Queryable(),
        g => g.CalcGid,
        d => d.CalcGid,
        (g, d) =>
        new CalcDefine { GroupId = g.GroupId, CalculatorName = d.CalculatorName, Order = g.CalcOrder })
        .ToList();

    var groupCalcBase = new GroupCalcBase();
    groupCalcBase.ProposalNumber = "G20190724007";
    groupCalcBase.TotalSalary = 300000;
    groupCalcBase.TotalInsuranceSalary = 150000;

    var stopWatch = new System.Diagnostics.Stopwatch();

    stopWatch.Reset();
    stopWatch.Start();

    var proposalInsurantDatas =
        this.groupCalcService.PrepareGroupCalcData(groupCalcBase);

    stopWatch.Stop();

    Console.WriteLine($"Prepare:{stopWatch.ElapsedMilliseconds}" );

    ...
}
```

> 移除使用 Console.WriteLine() 顯示系統資訊。

### 修改後

```C#
public void AnalysisGroupCalcEngine()
{  
    var calcDefines = this.calcGroupRepo.Queryable()
        .Where(g => g.GroupId == "Group_All")
        .Join(this.calcDefineRepo.Queryable(),
        g => g.CalcGid,
        d => d.CalcGid,
        (g, d) =>
        new CalcDefine { GroupId = g.GroupId, CalculatorName = d.CalculatorName, Order = g.CalcOrder })
        .ToList();

    var groupCalcBase = new GroupCalcBase();
    groupCalcBase.ProposalNumber = "G20190724007";
    groupCalcBase.TotalSalary = 300000;
    groupCalcBase.TotalInsuranceSalary = 150000;

    var stopWatch = new System.Diagnostics.Stopwatch();

    stopWatch.Reset();
    stopWatch.Start();

    var proposalInsurantDatas =
        this.groupCalcService.PrepareGroupCalcData(groupCalcBase);

    stopWatch.Stop();

    ...
}
```