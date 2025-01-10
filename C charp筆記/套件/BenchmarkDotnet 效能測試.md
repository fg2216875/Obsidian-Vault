
BenchmarkDotnet 可在Nuget套件管理員中下載，使用時必須在 release 環境下啟動 (此專案範本為「主控台應用程式」)
```csharp
class Program
{
	static void Main(string[] args)
	{
		var summary = BenchmarkRunner.Run<IsEmptyVsEqual>();
	}
}

[MemoryDiagnoser]
public class IsEmptyVsEqual
{
	[Benchmark]
	public void IsEmpty()
	{
		string sut = null;
		var a = string.IsNullOrEmpty(sut);

		sut = "";
		var b = string.IsNullOrEmpty(sut);

		sut = "aaaaaaaaaaaaasss";
		var c = string.IsNullOrEmpty(sut);
	}
	
	[Benchmark]
	public void IsEqual()
	{
		string sut = null;
		var a = sut == null || sut == "";

		sut = "";
		var b = sut == null || sut == "";

		sut = "aaaaaaaaaaaaasss";
		var c = sut == null || sut == "";
	}
}
```
- 在要測試的方法加上`[Benchmark]`屬性，即可加入測試行列
- 想要加上記憶體的測試，只需要在類別加上 `[MemoryDiagnoser]`

### 測試結果欄位說明
### 1. **Method**
- **描述**：測試的具體方法名稱（例如：`IsEmpty`, `IsEqual`, `StrIsEqual`）。
- **意義**：這些方法是您在測試中用來進行基準測試的函數。
### 2. **Mean**
- **描述**：平均執行時間（以奈秒為單位，ns）。
- **意義**：這是測試方法在多次運行中的平均執行時間，通常用來衡量方法的性能快慢。
### 3. **Error**
- **描述**：測量誤差（以奈秒為單位，ns）。
- **意義**：表示測量不確定性的範圍，通常反映方法在多次測試中的波動程度，計算方式與統計學中的置信區間相關。
### 4. **StdDev**
- **描述**：標準差（以奈秒為單位，ns）。
- **意義**：描述測試執行時間的變化程度。標準差越小，說明執行時間越穩定，越大則表示結果有更多的波動。
### 5. **Allocated**
- **描述**：測試方法中分配的記憶體大小（通常是以位元組為單位）。
- **意義**：測量每次執行方法時分配的堆內存量。如果是 `-`，表示此方法在測試過程中未進行任何堆內存分配，這對於效能敏感的程式碼是理想的。

參考:
[BenchmarkDotnet —— 效能測試好簡單](https://igouist.github.io/post/2021/06/benchmarkdotnet/)