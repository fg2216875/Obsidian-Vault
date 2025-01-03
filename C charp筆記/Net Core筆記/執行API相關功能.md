## 下方函式為呼叫API的寫法:

```C#
public class APIService
{
	public void PostInsertStockData(string json,string url)
	{
		//建立WebRequest，並設定要呼叫的API網址
		HttpWebRequest Request = (HttpWebRequest)WebRequest.Create(url);
		//設定要傳遞資料的格式，此為json
		Request.ContentType = "application/json; charset=utf-8";
		//設定請求為Post
		Request.Method = "POST";
		//將需 post 的資料內容轉為 stream
		using(var streamWriter = new StreamWriter(Request.GetRequestStream()))
		{
			streamWriter.Write(json);
			streamWriter.Flush();
			streamWriter.Close();
			try
			{
		//使用 GetResponse 方法將 request 送出，如果不是用 using 包覆，請記得手動 close WebResponse 物件，避免連線持續被佔用而無法送出新的 request
				var httpResponse = (HttpWebResponse)Request.GetResponse();
		//使用 GetResponseStream 方法從 server 回應中取得資料，stream 必需被關閉
		//使用 stream.close 就可以直接關閉 WebResponse 及 stream，但同時使用 using 或是關閉兩者並不會造成錯誤，養成習慣遇到其他情境時就比較不會出錯
				using (var streamReader = new StreamReader(httpResponse.GetResponseStream()))
				{
					var result = streamReader.ReadToEnd();
				}
				httpResponse.Close();
			}
			catch (Exception ex)
			{
			
			}
		} 
	}
}
```
# 使用 JsonProperty 設定類別屬性的別名

引入 Newtonsoft.Json 命名空間 
```C#
public class CertificateInfo
{
	/// <summary>
	/// 擁有者
	/// </summary>
	[JsonProperty(PropertyName="certificatioinOwner")]
	public string Name { get; set; }
	/// <summary>
	/// 日期
	/// </summary>
	[JsonProperty(PropertyName="Date")]
	public string createtime { get; set; }
}
```
若使用 Postman 觀察 API 串接的情形，可以發現類別序列化成 JSON 格式後的屬性名稱就是 PropertyName 設定的名稱

參考:https://www.tpisoftware.com/tpu/articleDetails/1178