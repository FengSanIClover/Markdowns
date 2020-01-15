# <span>ASP.NET</span> MVC Bad Practices: Controller Action Not Restricted to POST

### RiskLevel : High

![ASP.NET_MVC_Bad_Practices_Controller_Action_Not_Restricted_to_POST_1](/Fortify/High/ASP.NET_MVC_Bad_Practices_Controller_Action_Not_Restricted_to_POST/ASP.NET_MVC_Bad_Practices_Controller_Action_Not_Restricted_to_POST_1.png   "ASP.NET_MVC_Bad_Practices_Controller_Action_Not_Restricted_to_POST")

## Tips
Controller 的 Action 在**預設**的情況下**可接受任何的 HTTP 動詞**，攻擊者可以用極小的代價發動跨網站偽造要求攻擊。

## 程式碼範例

### 修改前

``` C#
public ActionResult Message(Response requestResult)
{
    return new JsonNetResult() { Data = requestResult, JsonRequestBehavior = JsonRequestBehavior.AllowGet };
}
```

> 限制 Action 僅接受 Post 動詞，提高跨網站偽造要求的難度。

### 修改後

```C#
[HttpPost]
public ActionResult Message(Response requestResult)
{
    return new JsonNetResult() { Data = requestResult, JsonRequestBehavior = JsonRequestBehavior.AllowGet };
}
```