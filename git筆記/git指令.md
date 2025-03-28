可使用git bash輸入指令
或在visul studio開啟PowerShell:   (快捷鍵' Ctrl ' + ' ~ ')

## 匯出指定版本區間差異檔案

1.開啟git bash後，將路徑切換成要匯出差異檔案的程式 (cd 程式路徑)

2.輸入指令(註'<'、'>'需要省略)，產生的檔案會在剛才指定的路徑中

```
git archive --format=zip --output=files.zip HEAD $(git diff-tree -r --no-commit-id --name-only --diff-filter=ACMRT <指定起始commit-id> <指定最後commit-id>)
```
註:
若方案底下有多個專案，而路徑設在某一個專案下執行指令，就會出現` fatal:pathspec XXXX did not match any files `訊息，故路徑要設定在"方案路徑那層中"

------------------
## 查看現有分支

```
git branch
```

-----------
## 修改本機分支名稱

```
git branch -m <Old Name> <New Name>
```
例: git brach -m develop FixbugDev

------------------
## 取消指定分支中的追蹤分支

路徑要先設定在要取消的本機分支上，再使用下方的指令
```
git branch --unset-upstream <本機分支名稱>
```

移除目前分支的上游
```
git branch --unset-upstream
```

---
## 設定本機分支的上游

```
git branch -u origin/遠端分支名稱
```

--------------------
## 找出特定檔案最後修改的認可紀錄

```
git blame <檔案路徑> -L <開始行號>,<結束行號>
```
例: git blame C:\\Working\\XXX\\Controllers\\file.cs -L 50,70

-------------
## 切換分支

``` 
git checkout <branch>
```
例: $ git checkout issue1  //將分支切換成 'issue1'

--------------------------
## 查看git config清單內容

```
git config --list
```
輸入後介面就會顯示列表給您觀看，若要離開則按鍵盤 q 即可離開。

-------------------
## 設定使用者信箱

```
git config user.email <email>
```

-------------------------
## 設定使用者名稱

```
git config user.name <user>
```

-------------------------
## 複製指定的遠端儲存庫到本機上

```
git clone <url>
```

-----------------
## 修改最後一次commit的訊息

使用--amend
```
git commit --amend -m "Change Message"
```

---------------
## 產生指定版本區間差異清單

```
git diff-tree -r --no-commit-id --name-status --text --diff-filter=ACDMRT <指定起始commit-id> <指定最後commit-id> > changes.txt
```
 註:
若方案底下有多個專案，而路徑設在某一個專案下執行指令，就會出現` fatal:pathspec XXXX did not match any files `訊息，故路徑要設定在"方案路徑那層中"

--------------
## 查詢遠端儲存庫

```
git remote -v
```

-----------------------------
## 新增遠端儲存庫位置

```
git remote add <name> <url>
```

------------------
## 修改遠端儲存庫位置

```
git remote set-url origin XXXXXXX
```

------------------
## Git初始化

讓 git 開始追蹤資料夾內所有檔案變化的一舉一動(會出現.git隱藏資料夾)
```
git init
```

-----------------
## 在Git Bash中切換路徑

cd <指定路徑>
例: cd /D/HZN/Test

------------------
## 修改已註冊的遠端儲存庫名稱

```
git remote rename <舊名稱> <新名稱>
```
將遠端儲存庫的名稱從"舊名稱" 改為"新名稱"

-------
## 還原所有的檔案，復原成未修改狀態

```
git restore .
```

將單一檔案還原成未修改的狀態
```
git restore <檔案路徑>
```
例:`git restore path/to/your/file.txt`

---
## 更新遠端儲存庫資訊

```
git fetch <遠端儲存庫名> <遠端分支名>
```
註:省略"遠端分支名"會把遠端儲存庫所有更新取回本地

----
## 合併遠端更新到當前分支

```
git merge <遠端儲存庫名>/<遠端分支名>
```

-----
## 直接拉取遠端分支更新並合併

```
git pull <遠端儲存庫名> <遠端分支名>
```
註:`git pull` = `git fetch` + `git merge` 

---
## 推送分支

```
git push <遠端名稱> <本機分支名稱>:<遠端分支名稱>
```

如果遠端分支名稱與本機分支名稱相同，可以省略冒號及後續部分
```
git push <遠端儲存庫名> <遠端分支名>
```

例: git push origin main
 `origin` 如果沒有 `main` ，則自動建立一個同樣叫做 `main` 的分支。如果本來就有 `main` 分支，則會將此分支指向最新進度。
 
-------------------------
## 強制推送分支到遠端

此命令為強制上傳，忽略舊版本覆蓋較新版本的提醒(當遠端分支需要倒退版本時使用)
```
git push -f
```

-------------
## 移除已提交認可

使用reset:
每一個 ^ 符號表示「前一次」的意思，包含e12d8ef這個認可，所以當要取消e12d8ef這個commit的話，就直接寫 $ git reset e12d8ef^
```
 git reset e12d8ef^
```

如果要reset第一個認可的話可用
```
git reset HEAD^
```

---------------
## 取消已追蹤的檔案

1.針對單獨檔案取消追蹤
```
git rm --cached filename    
```

2.針對資料夾取消追蹤
```
git rm -r --cached foldername   
```

參考:https://cynthiachuang.github.io/Ignore-Tracked-Files-in-Git/

----
## Git合併指定commit到另一個分支

比如feature 分支上的commit 82ecb31 非常重要，它含有一個bug的修改，或其他人想訪問的內容。無論什麼原因，

你現在只需要將82ecb31 合併到master，而不合併feature上的其他commits，所以我們用git cherry-pick命令來做：

git checkout master  --選擇哪個分支要被合併

git cherry-pick 82ecb31  --將82ecb31 commit合併到 master分支

參考: [https://www.itread01.com/content/1549285588.html](https://www.itread01.com/content/1549285588.html)

---------------------------------------