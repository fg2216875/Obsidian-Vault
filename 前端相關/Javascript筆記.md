**將members陣列中指定的member物件取出後，並push到checkPerson陣列中**
```javascript
let checkPerson = [];
let members = [{NO:1,Name:"Kity"},{NO:2,Name:"Kai"},{NO:1,Name:"Ko"}];
let member = members.find(x=>x.No == 1);
checkPerson.push(member);
```
註:此時若將checkPerson內的member資料做修改時，會連同影響到members中的原始資料

-------
**在List A新增指定物件，同時將List B刪除剛剛新增的指定物件**
```javascript
let editAudits = [{XXX,XXX},{CCC,CCC}];
var audit = this.editAudits.filter(x => x.gOrder == this.selectedAudit)[0]; 
// 將其添加到審核項目群組 
this.auditsGroup.push(audit); 
// 從editAudits中刪除該審核項目 
var index = this.editAudits.indexOf(audit); 
if (index > -1) { 
	this.editAudits.splice(index, 1); 
}
```

--------
**將輸入的字串日期增加一天**
```javascript
// 假設你的日期字串是 '2023-08-15'
let dateStr = '2023-08-15';
// 創建一個 Date 物件
let date = new Date(dateStr);
// 使用 setDate 方法將日期加1天
date.setDate(date.getDate() + 1);
// 將日期轉換回字串格式
let newDateStr = date.toISOString().split('T')[0];
console.log(newDateStr); // 輸出 '2023-08-16'
```

----------
**在物件陣列中只取出特定屬性**
```javascript
const data = [ 
{ id: 1, name: 'Alice', age: 25, gender: 'Female', country: 'USA' }, 
{ id: 2, name: 'Bob', age: 30, gender: 'Male', country: 'Canada' }, 
{ id: 3, name: 'Charlie', age: 28, gender: 'Male', country: 'UK' }, // ...其他物件 ];
//物件陣列，{ name, country }=>從每個物件中取出`name`和`country`屬性
const result = data.map(({ name, country }) => ({ name, country }));
//純字串陣列
const names = data.map(({ name }) => name);
```
註:
#### `map()` 特色
- 會透過`函式`內所回傳的值組合成一個新的陣列
- 並不會改變原陣列
- 回傳數量會等於原始陣列的長度
- 如果不回傳則是 `undefined`

----
**扣除數字但結果不小於0**
```javascript
function deductExamCount(examInfo) {
let result = Math.max(0, examInfo.examCount - examInfo.validExam); 
return result;
}
```

-----
**監聽瀏覽器離開畫面的事件**
```javascript
//上一頁、重新整理、關閉皆會觸發beforeunload事件(也可使用pagehide)
window.addEventListener('beforeunload', function (event) { 
	// 在此執行你想要的操作，例如保存資料或彈出警告 
	console.log('頁面即將關閉'); 
	// 若需要顯示提示訊息： 
	event.preventDefault(); 
	event.returnValue = ''; // 部分瀏覽器需要這行來顯示確認對話框 
});
```
註: 瀏覽器對顯示 `beforeunload` 對話框的行為有限制，可能不支援自訂訊息。