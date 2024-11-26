`Formatting.None` 是 `Newtonsoft.Json`（也稱為 `Json.NET`）中的一個選項，用於格式化 JSON 字串的方式。
在 `ToString` 方法中指定 `Formatting.None` 時，它會產生**沒有縮排或空白**的緊湊格式 JSON 字串。

在序列化 JSON 時，`Formatting` 有以下兩種選項：
1. **Formatting.None**：產生緊湊格式的 JSON，沒有任何縮排或額外空白。適合需要節省空間或傳輸速度的情境，例如在 API 回應中。
2. **Formatting.Indented**：產生帶有縮排的 JSON，使其更具可讀性，適合除錯或展示時使用。
```C#
var jsonObject = new JObject
{
	{ "name", "John Doe" },
	{ "age", 30 },
	{ "isActive", true }
};

// 緊湊格式
string compactJson = jsonObject.ToString(Formatting.None);
Console.WriteLine("Compact JSON:");
Console.WriteLine(compactJson);

// 縮排格式
string indentedJson = jsonObject.ToString(Formatting.Indented);
Console.WriteLine("\nIndented JSON:");
Console.WriteLine(indentedJson);
```

輸出結果
```json
//Compact JSON（Formatting.None）
{ "name": "John Doe", "age": 30, "isActive": true }

//Indented JSON（Formatting.Indented）
{
  "name": "John Doe",
  "age": 30,
  "isActive": true
}

```
