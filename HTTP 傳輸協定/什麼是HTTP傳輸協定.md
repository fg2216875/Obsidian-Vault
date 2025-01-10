http全名為超文本傳輸協定，是一種用來在網路上傳輸資料的協定。簡單來說，就是在網路上用來傳送 HTML、CSS、JavaScript 等網頁資源的協定。

#### 工作流程大概可以概括為以下幾個步驟：  
- Client 向 Server 發送一個 HTTP 請求  
- Server 收到這個請求後，根據請求的內容進行對應的處理  
- Server 向Client 發送一個 HTTP 回應，內容包括請求的結果和對應的資源

補充:
因http為明文的傳輸協定，等於把傳輸的資料暴露在網路中，因此一般會使用https超文字安全傳輸通訊協定，來加密保護我們資料

### **HTTP 的組成結構**

 HTTP 的組成結構，分別是 HTTP Request 與 HTTP Response 這兩個大項目![[Pasted image 20250106171620.png]]
HTTP 的組成結構(左側為 Request，右側為 Response)

#### http method:
當在網路上使用RESTful API時，我們會使用不同的HTTP Method，來告訴伺服器我們想要做什麼操作，而最常見的為以下幾種:
- GET：讀取資料
- POST：新增資料
- PATCH：修改資料
- PUT：修改資料（若該筆資料不存在，則自動新增資料）
- DELETE：刪除資料

#### Http status code 狀態碼
- 2XX 成功類 (操作被成功接受並處理)，例如：200 成功回應
- 3XX 重定向類 (需進一步操作才能完成)，例如：301 成功轉向
- 4XX 客戶端錯誤類 (請求語法錯誤或無法完成請求)，例如：404 找不到資源
- 5XX 伺服器錯誤類 (後端的問題)，例如：500 伺服器錯誤

參考:
[資訊科普系列(1) — 超文本傳輸協定(HTTP)](https://medium.com/moda-it/http%E4%BB%8B%E7%B4%B9-b2f2abc26def)
[何謂HTTP 傳輸協定](https://medium.com/pierceshih/%E7%AD%86%E8%A8%98-%E4%BD%95%E8%AC%82-http-%E5%82%B3%E8%BC%B8%E5%8D%94%E5%AE%9A-1d9b5be3fd24)