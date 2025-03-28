```html
<label>
	<div> <input type="file" @change="handleFileUpload" /> 
		<button @click="uploadFile">上傳 Excel</button> 
	</div>
</label>
```

```javascript
<script>
import axios from "axios";

export default {
  data() {
    return {
      file: null,
    };
  },
  methods: {
    handleFileUpload(event) {
      this.file = event.target.files[0];
    },
    uploadFile() {
      if (!this.file) {
        alert("請選擇檔案");
        return;
      }

      let formData = new FormData();
      formData.append("file", this.file);

      try {
        const response = await axios.post("/api/upload", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        });
        alert("上傳成功: " + response.data.message);
      } catch (error) {
        alert("上傳失敗: " + error.response?.data?.message || error.message);
      }
    },
  },
};
</script>
```

```csharp
using Microsoft.AspNetCore.Mvc;
using System.Data.SqlClient;
using Dapper;
using NPOI.SS.UserModel;
using NPOI.XSSF.UserModel;
using System.IO;
using System.Collections.Generic;

[Route("api/[controller]")]
[ApiController]
public class UploadController : ControllerBase
{
    private readonly string _connectionString = "YourConnectionString";

    [HttpPost]
    [Route("upload")]
    public IActionResult UploadExcelFile([FromForm] IFormFile file)
    {
        if (file == null || file.Length == 0)
        {
            return BadRequest(new { message = "無效的檔案" });
        }

        try
        {
            var dataList = new List<YourModel>();
            
            using (var stream = new MemoryStream())
            {
                file.CopyTo(stream);
                stream.Position = 0;
                IWorkbook workbook = new XSSFWorkbook(stream);
                ISheet sheet = workbook.GetSheetAt(0);
                
                for (int rowIndex = 1; rowIndex <= sheet.LastRowNum; rowIndex++) // Skip header row
                {
                    IRow row = sheet.GetRow(rowIndex);
                    if (row == null) continue;
                    
                    var data = new YourModel
                    {
                        Column1 = row.GetCell(0)?.ToString(),
                        Column2 = row.GetCell(1)?.ToString(),
                        Column3 = row.GetCell(2)?.ToString(),
                    };
                    dataList.Add(data);
                }
            }

            // 使用 Dapper 將資料批次插入 SQL
            var sql = "INSERT INTO YourTable (Column1, Column2, Column3) VALUES (@Column1, @Column2, @Column3)";
            
            using (var conn = new SqlConnection(_connectionString))
            {
                conn.Execute(sql, dataList);
            }

            return Ok(new { message = "資料上傳成功" });
        }
        catch (Exception ex)
        {
            return StatusCode(500, new { message = "伺服器錯誤: " + ex.Message });
        }
    }
}

```

```csharp
public class YourModel
{
    public string Column1 { get; set; }
    public string Column2 { get; set; }
    public string Column3 { get; set; }
}
```