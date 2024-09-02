在Tests分頁撰寫程式，以loop方式呼叫API:
1.先將要測試的API路徑、參數填寫好
![[Pasted image 20240530164545.png]]

2.切換到"Tests"分頁並寫下呼叫API的迴圈程式
``` 
const totalRequests = 20;
let currentRequest = pm.environment.get("currentRequest") || 1;
if (currentRequest <= totalRequests) {
    // 增加請求計數器
    pm.environment.set("currentRequest", currentRequest + 1);
    // 重新發送請求
    postman.setNextRequest(pm.info.requestName);
} else {
    // 重置請求計數器
    pm.environment.set("currentRequest", 1);
}
```

3.設定好後按下"Save"按鈕
![[Pasted image 20240530171447.png]]

4.在側邊欄選取"Collections"，並選擇要測試的"檔案"或"集合"，接著點選"RUN"按鈕
![[Pasted image 20240530171824.png]]

5.在Run order中，可以勾選要執行哪些API，"Iterations"是指執行的次數，最後點選"橘色Run"按鈕即可執行剛剛所寫的程式
![[Pasted image 20240530172314.png]]