# HTML5: MIME Sniffing

### RiskLevel : Medium

![HTML5_MIME_Sniffing_1](/Fortify/Medium/HTML5_MIME_Sniffing/HTML5_MIME_Sniffing_1.png "HTML5_MIME_Sniffing")
![HTML5_MIME_Sniffing_2](/Fortify/Medium/HTML5_MIME_Sniffing/HTML5_MIME_Sniffing_2.png "HTML5_MIME_Sniffing")

## Tips
Web.config 檔案**不包括阻擋 MIME 攔截攻擊**所需的表頭，如果**不明確停用** MIME 攔截，瀏覽器可能以非預期的方式解譯資料，**導致出現 Cross-site scripting 攻擊**。 

## 程式碼範例

### 修改前

``` HTML
<customHeaders>
    <add name="Content-Security-Policy" value="default-src https://fonts.gstatic.com/ 'self';script-src 'self' 'unsafe-inline' 'unsafe-eval';style-src http://fonts.googleapis.com 'self' 'unsafe-inline';img-src 'self' data:; frame-src 'self'" />
</customHeaders>
```

> 加入 \<add name="X-Content-Type-Options" value="nosniff"/> 對表頭做全域設定。

### 修改後

```HTML
<customHeaders>
    <add name="Content-Security-Policy" value="default-src https://fonts.gstatic.com/ 'self';script-src 'self' 'unsafe-inline' 'unsafe-eval';style-src http://fonts.googleapis.com 'self' 'unsafe-inline';img-src 'self' data:; frame-src 'self'" />
    <add name="X-Content-Type-Options" value="nosniff"/>
</customHeaders>
```