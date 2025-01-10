```csharp
public FileResult ExportExamResult(string keyword)
{
	try
	{
	   var ms = _SubjectScoreManageService.ExportScoreExcel(keyword);
		return File(ms.ToArray(), "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet", "成績紀錄.xlsx");
	}catch(Exception ex)
	{
		_logger.Error(ex, $"匯出Excel失敗！{ex}");
		return null;
	}     
}
```

```csharp
public MemoryStream ExportScoreExcel(string keyword)
{
	List<SubjectScoreViewModels> scoreList = new List<SubjectScoreViewModels>();
	using (var conn = DALFactory.getConnection("GTC"))
	{
		scoreList = _SubjectScoreManageRepo.GetExamScoreByAuthAdminSearch(keyword, conn).ToList();
	}

	IWorkbook wb = new XSSFWorkbook();
	ISheet sheet = wb.CreateSheet("成績列表");
	var headers = new[] { "No.", "備註", "認證碼", "考試日期", "考生", "手機號碼", "科目名稱"};
	var properties = new[] { "no", "Note", "ExamCode", "ExamDateTimeStr", "CertInfoChineseName", "Mobile", "SubjectName"};

	var headerRow = sheet.CreateRow(0);
	for (int i = 0; i < headers.Length; i++)
	{
		headerRow.CreateCell(i).SetCellValue(headers[i]);
	}

	for (var i = 0; i < scoreList.Count; i++)
	{
		var row = sheet.CreateRow(i + 1);
		//No.流水號
		row.CreateCell(0).SetCellValue((i + 1).ToString());
		for (int j = 1; j < properties.Length; j++)
		{
			var cell = row.CreateCell(j);
			var property = scoreList[i].GetType().GetProperty(properties[j]);
			var value = property.GetValue(scoreList[i]);
			cell.SetCellValue(value == null ? string.Empty : value.ToString());
			cell.SetCellType(CellType.String);
		}
	}
	var ms = new MemoryStream();
	wb.Write(ms);

	return ms;
}
```
