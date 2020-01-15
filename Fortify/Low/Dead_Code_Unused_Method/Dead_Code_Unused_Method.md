# Dead Code: Unused Method

### RiskLevel : Low

![Dead_Code_Unused_Method_1](/Fortify/Low/Dead_Code_Unused_Method/Dead_Code_Unused_Method_1.png "Dead_Code_Unused_Method")
![Dead_Code_Unused_Method_2](/Fortify/Low/Dead_Code_Unused_Method/Dead_Code_Unused_Method_2.png "Dead_Code_Unused_Method")
![Dead_Code_Unused_Method_3](/Fortify/Low/Dead_Code_Unused_Method/Dead_Code_Unused_Method_3.png "Dead_Code_Unused_Method")

## Tips
程式中存在**從未被呼叫**的方法，此項錯誤可能發生於開發階段因需求或寫法變更，導致原有的方法不再需要，且未將之移除而造成的。

## 程式碼範例

``` C#
private IDictionary<string, string> GetTravelPlans(TravelQuoteCalcResultBM bm)
{
    return bm.PlanCodeResult.OrderBy(x => x.PlanCode)
        .ToDictionary(x => x.PlanCode, y => $"{y.PlanCode}\n{y.PlanName}");
}
```

> 此 GetTravelPlans 有被 ToSignRenderObjAsync 這個私有方法呼叫。

```C#
private async Task<dynamic> ToSignRenderObjAsync(TravelQuoteCalcResultBM bm, string proposalNumber, int trialCalId)
{
    return new TravelSignBM()
    {
        ProposalNumber = proposalNumber,
        TrailCalId = trialCalId.ToString(),
        BasicInfo = this.GetSignBasicInfo(bm, proposalNumber),
        PastSituation = await this.GetPastSituationAsync(proposalNumber),
        TravelQuoteRate = await this.GetQuoteRateAsync(bm, proposalNumber),
        PackageContents = await this.GetPackageContentsAsync(bm, proposalNumber),
        PackageTotalAmounts = await this.GetPackageTotalAmountsAsync(bm),
        PackageTotalPremiums = await this.GetPackageTotalPremiumsAsync(bm),
        Plans = this.GetTravelPlans(bm)
    };
}
```

>ToSignRenderObjAsync 有被 GetSignJObjectAsync 這個公有方法呼叫，因此 GetTravelPlans 並非 Unused Method，此為弱掃系統的誤判範例。

```C#
public async Task<JObject> GetSignJObjectAsync(string proposalNumber)
{
    var trialCalData = this.trialCalDataResultService.GetTrialDataResult(proposalNumber);
    JObject rawResult = JObject.Parse(trialCalData.JsonData);

    ServiceLocator.Current.GetInstance<ICalculator>(ProposalReportKeys.TravelQuote).Calculate(ref rawResult);
    dynamic calcResult = this.ToObject(rawResult);
    var result = await this.ToSignRenderObjAsync(calcResult, proposalNumber, trialCalData.TrialCalId);

    return JObject.FromObject(result);
}
```