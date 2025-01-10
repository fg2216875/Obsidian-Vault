```csharp
try
{
    DoSomething();
}
catch (Exception ex)
{
    Log(ex); // 把當前的例外資訊寫入 log。
    throw;   // 再次拋出同一個例外。
}
```
- 單單寫 `throw` 會保留原本的堆疊追蹤資訊（stack trace）。也就是說，透過堆疊追蹤資訊，便能抓到原始的例外。
- 如果寫 `throw ex`，則會重設堆疊追蹤資訊，這將導致呼叫端無法透過例外物件的 `StackTrace` 屬性得知引發例外的源頭是哪一行程式碼。基於此緣故，通常不建議採用此寫法。

參考:https://www.huanlintalk.com/2022/09/csharp-exception-handling.html