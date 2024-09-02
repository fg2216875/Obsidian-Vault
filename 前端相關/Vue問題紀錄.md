1.使用 v-cloak 解決 Vue Instance 完成編譯前顯示變數的問題
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

=========================================================
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
			auditsGroups: JSON.parse(@Html.Raw(Model.AuditsGroupsJson)) } }, 
	})
```