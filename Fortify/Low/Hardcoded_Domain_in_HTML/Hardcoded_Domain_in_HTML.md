# Hardcoded Domain in HTML

### RiskLevel : Low

![Hardcoded_Domain_in_HTML_1](/Fortify/Low/Hardcoded_Domain_in_HTML/Hardcoded_Domain_in_HTML_1.png "Hardcoded_Domain_in_HTML")

## Tips
網站包含**另一個網站中可執行的內容**是有風險的，會導致網站的**安全性**與另一個網站息息相關。

## 程式碼範例

### 修改前

``` C#
@section pagespecific{
    <script src="https://cdnjs.com/libraries/pdf.js"></script>

    <script>
        ...
    </script>

    ...
}
```

> 移除直接引用另外一個網站中可執行的內容。

### 修改後

```C#
@section pagespecific{
    <script>
        ...
    </script>

    ...
}
```