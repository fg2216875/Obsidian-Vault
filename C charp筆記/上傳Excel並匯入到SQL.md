```html
<label>
	<input type="file" class="d-none" accept=".xlsx, .xls" v-on:change="recognizeFromExcel">
	<div class="btn btn-success btn-sm mx-1 me-0"><i class="fas fa-file-excel me-1"></i>批次認列匯入</div>
</label>
```

```javascript
recognizeFromExcel: function (e) {
//event.target.files[0];
	const formData = new FormData();
	formData.append('file', e.target.files[0]);
	$.ajax({
		url:'@Url.Action(nameof(ManageController.RecognizeFromExcel), "Manage")',
		method: "POST",
		contentType: false,
		processData: false,
		data: formData,
		dataType: "json",
		success: function (obj) {
			console.log(obj);
			let jsonObject = JSON.parse(obj);
			if (jsonObject.resp == "0") {
				info(jsonObject.msg);
			} else {
				alert(jsonObject.msg);
			}
		}, error: function (jqXHR, error) {
			console.log("status: " + jqXHR.status + " error: " + error);
		}
	});
}
```

```C#
public JsonResult RecognizeFromExcel(HttpPostedFileBase file){
	JObject result = new JObject();
	JsonResult jr = new JsonResult();
	if(file == null)
	{
		result["resp"] = "-1";
		result["msg"] = "請上傳檔案";
		jr.Data = result.ToString(Formatting.None);
		return jr;
	}
	
	var dataToUpdate = _ManageService.GetColumnValue(file);
	var flag = _ManageService.MultipleRecognize(dataToUpdate);
	if (flag)
	{
		result["resp"] = "0";
		result["msg"] = "資料更新成功";
	}
	else
	{
		result["resp"] = "-1";
		result["msg"] = "fail";
	}
	
	jr.Data = result.ToString(Formatting.None);
	return jr;
}

public class UpdateInfo
{
	public string Mobile { get; set; }
	public string ExamCode { get; set; }
	public string Result { get; set; }
}
```

```C#
public List<UpdateInfo> GetColumnValue(HttpPostedFileBase file)
{
	IWorkbook workbook;
	workbook = new XSSFWorkbook(file.InputStream);  // 處理 .xlsx
	var sheet = workbook.GetSheetAt(0);  // 取得第一個工作表
	var headers = new Dictionary<string, int>();  // 儲存欄位名稱與索引
	// 取得表頭資料（第一行）
	IRow headerRow = sheet.GetRow(0);
	// 找出目標欄位的索引位置
	for (int i = 0; i < headerRow.LastCellNum; i++)
	{
		string header = headerRow.GetCell(i)?.ToString().Trim();
		headers[header] = i;  // 儲存欄位名稱對應的索引
	}

	var dataToUpdate = new List<UpdateInfo>();
	// 遍歷資料行（從第 2 行開始）
	for (int rowIndex = 1; rowIndex <= sheet.LastRowNum; rowIndex++)
	{
		IRow row = sheet.GetRow(rowIndex);
		if (row == null) continue;

		var info = new UpdateInfo
		{
			Mobile = GetCellValue(row, headers, "手機號碼"),
			ExamCode = GetCellValue(row, headers, "認證碼"),
			Result = GetCellValue(row, headers, "結果")
		};
		dataToUpdate.Add(info);
	}

	return dataToUpdate;
}
        
public bool MultipleRecognize(List<UpdateInfo> updateData)
{
	if(updateData == null)
	{
		return false;
	}

	bool isSuccess = false;
	using(var conn = XXX.getConnection())
	{
		var memberMap = _ManageRepo.GetMemberId(updateData, conn);
		// 將查詢到的會員 ID 整合到要更新資料中
		foreach (var item in updateData)
		{
			item.RecognitionUser = Sess.Member.MemberID;
			if (memberMap.ContainsKey(item.Mobile))
			{
				item.MemberId = memberMap[item.Mobile];
			}
		}

		using(var tran = conn.BeginTransaction())
		{
			var flag = _ManageRepo.MultipleRecognize(updateData, conn, tran);
			if (flag)
			{
				tran.Commit();
				isSuccess = true;
			}
		}
	}   
	return isSuccess;
}
```