### 使用 v-cloak 解決 Vue Instance 完成編譯前顯示變數的問題
```css
<style>
	[v-cloak] {
	  display: none;
	}
</style>
```

```html
<div v-cloak>
  ${{item.spec_name }}
</div>
```
使用`v-cloak`加上樣式隱藏，編譯完成後，使用者才會看到這個`div`區塊。

----
### 確保JSON字符串不會被HTML編碼，避免資料會出現"&quot"字串

```C#
	public class QuestionEdit { 
		public List<AuditsGroupInfo> AuditsGroups { get; set; } 
		public string AuditsGroupsJson { get { return JsonConvert.SerializeObject(this.AuditsGroups); } } 
	} 
```

使用`@Html.Raw`方法會將內容直接插入到HTML中，因此要確保內容是安全的，以防止XSS攻擊。
```javascript
	var vm = new Vue({ 
		el: "#app", 
		data: function() { return { 
		auditsGroups: JSON.parse(@Html.Raw(Model.AuditsGroupsJson))}          }, 
	})
```

---
### 將使用base64加密的字串進行解碼

```javascript
decodeBase64(encodedString) { 
	// 將 Base64 字串轉成 
	byte array const binaryString = atob(encodedString); 
	const len = binaryString.length; 
	const bytes = new Uint8Array(len); 
	for (let i = 0; i < len; i++) { 
		bytes[i] = binaryString.charCodeAt(i); 
	} 
	// 使用 TextDecoder 來正確地解析 UTF-8 字串 
	const decoder = new TextDecoder('utf-8'); 
	return decoder.decode(bytes); } 
}
```

----
### 避免vue實例初始化時，modal的DOM尚未存在

```javascript
openModal() { 
	// 等待 Vue 完成數據更新後再顯示 
	modal showImageVm.$nextTick(() => {   
		$("#showImage").modal('show'); }); 
	}
```

----
### 使用全局事件總線 (EventBus) 傳遞資料

要在專案中使用 eventBus 其實就是在 Vue 裡面再註冊一個 Vue 實例，eventBus 主要會使用到 Vue 實例中的四種方法：
`$on` ：註冊監聽
`$once`：只監聽一次
`$off`：取消監聽
`$emit`：送出事件
```javascript
//建立事件總線
const EventBus = new Vue();

var vm1 = new Vue({ 
	el: '#app', 
	data() { 
		return { memberid: '' } 
	}, 
	created() { 
		ajax({ 
			success: () => { 
				this.memberid = 'A';
				EventBus.$emit('updateQuestionId', 'B'); 
		   } 
		}) 
	}
});

var vm2 = new Vue({
    el: '#app2',
    data() {
 	   return {
 		   questionId: ''
 	   }
    },
    created() {
 	   EventBus.$on('updateQuestionId', (id) => {
 		   this.questionId = id;
 	   });
    }
});
```

備註:
1.`EventBus.$on('updateQuestionId', ...)`：  
使用 `EventBus` 監聽名稱為 `'updateQuestionId'` 的事件。當此事件被觸發時，會執行後面的CallBack函式 `(id) => { this.questionId = id; }`。

**事件名稱 `'updateQuestionId'`**：  
`'updateQuestionId'` 是自訂的事件名稱。可以將它視為一個識別碼，用於辨別觸發的事件。當 `EventBus` 上有同名的事件觸發（例如使用 `EventBus.$emit('updateQuestionId', 'B')`），所有監聽 `'updateQuestionId'` 事件的函式都會被執行

**參數 `id`**： 當事件被觸發時，可以透過 `EventBus.$emit('updateQuestionId', 'B')` 傳遞一個參數 (`'B'`)，該參數在監聽時接收到，並賦予 `id` 變數。此處的 `(id) => { this.questionId = id; }` 將接收到的 `id` 值賦給 `vm2` 中的 `questionId`。

----
