## 無法clone專案:
可能是因為某些因素所造成，步驟如下
1.當第一次clone專案時，當電腦沒有相對應的憑證時，通常會跳出輸入git帳密的視窗，輸入完後就會記錄在電腦中。假如git雲端有更換網域的話，但在clone專案時沒出現輸入視窗時，可能是使用到前人遺留下來的憑證，會造成沒有權限操作雲端資料庫，需要將舊憑證刪除後，待有跳出輸入帳密視窗即為成功

刪除憑證方式:
開啟控制台 => 選擇"認證管理員" => 選右邊"Windows認證" => 找出舊憑證並刪除
![[Pasted image 20240617145412.png]]
![[Pasted image 20240617145502.png]]

----------------------------------
## 使用git push時，出現"Everything up-to-date" 的訊息:

通常代表本地分支已經與遠端分支同步，沒有需要更新或提交的變更。
**1.先確認本地有無未提交的變更:**
可以使用 `git status` 查看是否有未追蹤或尚未提交的文件。
```
git status
```
如果有變更，則需要將這些變更提交，才能夠推送至遠端儲存庫。
```
git add .
git commit -m "你的提交訊息"
git push origin your-branch-name
```

**2.確認你所在的分支:**
確認是否在正確的分支上執行了操作。可以使用 `git branch` 檢查當前分支，並使用 `git checkout branch-name` 切換到正確的分支。
```
git branch
git checkout your-branch-name
```

**3.檢查遠端分支版本:**
遠端儲存庫可能有更新但未被檢測到，可以使用 `git fetch` 來手動從遠端儲存庫，並抓取最新的版本。
```
git fetch
```

---
## 推送分支可能出現的問題

fatal: The current branch master has no upstream branch. To push the current branch and set the remote as upstream, use 
`git push --set-upstream origin master`

意思是當嘗試推送分支時，Git 通知該分支尚未設定上游（upstream）分支。需要告訴 Git，當前的本地分支（如 `master`）應該對應到遠端的哪個分支。**可使用上述訊息中的指令設定上游分支**

備註:
- `--set-upstream`: 設定本地分支與遠端分支的對應關係（上游分支）。
- `origin`: 預設遠端儲存庫的名稱。
- `master`: 指定的遠端分支名稱。


! [rejected] master -> master (fetch first) error: failed to push some refs to 'http://XXX.XXX.1.1/App/ABC.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing hint: to the same ref. You may want to first integrate the remote changes 
hint: (e.g., 'git pull ...') before pushing again.

這個錯誤的意思是：遠端儲存庫的 `master` 分支已經有變更，而你的本地分支缺少這些變更。Git 為了保護遠端儲存庫，阻止了這次的推送。

### 1.拉取遠端分支的變更
執行以下指令，將遠端的 `master` 分支拉取到本地：
`git pull origin master --rebase`

#### **`--rebase` 的作用**：
1. 會把本地的變更暫時移開，先應用遠端的變更，然後再將本地的提交依序應用回來。
2. 確保提交歷史保持乾淨。

### 2.強制覆蓋遠端變更

----
## 拉取遠端分支時出現"git did not exit cleanly (exit code 128)"錯誤

狀況:
由於在gitea修改完密碼後，用tortoisegit去拉取分支，因為tortoisegit並沒有更新帳號狀態，所以登入者的名稱和密碼皆為舊版。

解決方式:
可以先用git bash查看當前git使用者名稱和Email是否正確，若與gitea的帳號不同時可以先修改
```
git config --global user.email
git config --global user.name
```

1.先在windows搜尋"認證管理員"，然後選擇"windows認證"
2.下方一般認證中，找到該git的url，然後按下"編輯"，修改使用者名稱和密碼

註:剛剛有更新過，但依舊會出現錯誤訊息，但不知何故，該認證突然消失。
然後再去git時，就會出現要求輸入使用者名稱和密碼，接著就成功拉取專案了

參考:https://stackoverflow.com/questions/20195304/how-do-i-update-the-password-for-git