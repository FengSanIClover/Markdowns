# System Information Leak: Internal

### RiskLevel : Low

![System_Information_Leak_Internal_1](/Fortify/Low/System_Information_Leak_Internal/System_Information_Leak_Internal_1.png "System_Information_Leak_Internal")
![System_Information_Leak_Internal_2](/Fortify/Low/System_Information_Leak_Internal/System_Information_Leak_Internal_2.png "System_Information_Leak_Internal")

## 程式碼範例

### 修改前

```JavaScript
platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.log(err));
```

> 調整加入環境的判斷，於開發環境時在顯示除錯資訊。

### 修改後

```JavaScript
platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => {
    if(environment.production) {
      console.log(err);
    }
  });
```