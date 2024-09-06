1.將members陣列中指定的member物件取出後，並push到checkPerson陣列中，此時若將checkPerson內的member資料做修改時，會連同影響到members有對應的原生資料
```javascript
let checkPerson = [];
let members = [{NO:1,Name:"Kity"},{NO:2,Name:"Kai"},{NO:1,Name:"Ko"}];
let member = members.find(x=>x.No == 1);
checkPerson.push(member);
```

2.List A新增指定物件，同時將List B刪除剛剛新增的指定物件
A:
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

3.將輸入的字串日期增加一天
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