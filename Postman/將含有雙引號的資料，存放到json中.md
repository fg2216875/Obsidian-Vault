1.切換到`Scripts`分頁中，會有兩個頁籤可選擇
"Pre-request": 在呼叫API前要所要執行的javascript程式
"Pre-response":在得到API回應後，所要執行的javascript程式
![[Pasted image 20240902195922.png]]

2.在`Pre-request`中寫下這段程式碼
```JavaScript
let originalHTML = "XXXXXXX";
// 使用 JavaScript 內建的 JSON.stringify 來自動轉義特殊字符
let htmlContent = JSON.stringify(originalHTML);
// 您的原始字串
let originalText = `YYYYYYY`;
// 使用 JavaScript 內建的 JSON.stringify 來自動轉義特殊字符
let textContent = JSON.stringify(originalText);

// 將字串存入環境變數中，以便在 Body 中使用
pm.environment.set("HtmlContent", htmlContent);
pm.environment.set("TextContent", textContent);
```

3.切換到`Body`分頁並選擇`raw`，在撰寫json時，使用`{{ }}`並指定剛才設置的環境變數名稱，就可以取得變數中的資料
```json
{
    "HtmlContent": {{HtmlContent}},
    "TextContent": {{TextContent}}
}
```
![[Pasted image 20240902200556.png]]