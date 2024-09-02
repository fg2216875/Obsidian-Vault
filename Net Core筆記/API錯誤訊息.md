## An unhandled exception occurred while processing the request:

SQL連線字串需要多加上"TrustServerCertificate=true"這段才不會出現此錯誤頁面
![[Pasted image 20220601152915.png]]

=========================================================

## The ConnectionString property has not been initialized:

這次之所以會發生，是因為把連線字串寫到appsettings.Development.json中。而發佈後的API程式是設定讀取appsettings.json檔案的資料，故讀不到寫在appsettings.Development.json的連線字串，所以把連線字串寫到appsettings.json中就解決問題了