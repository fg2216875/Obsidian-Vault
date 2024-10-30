#### host.docker.internal 的用途：
1. **從容器訪問主機服務**：
	如果你的應用在 Docker 容器中運行，但需要訪問主機上的資料庫、API 服務，或其他網路資源時，你可以使用 `host.docker.internal` 來訪問主機的網路。
	
2. **僅適用於 Mac 和 Windows**：
	 在 **Linux** 上 `host.docker.internal` 可能無法使用。如果需要類似的功能，可能需要自行設定網橋或其他方式。

#### 使用範例:
假設主機上有一個 MySQL 服務跑在 `localhost:3306`，你希望 Docker 容器中的應用連接這個 MySQL。

 Docker 容器內部連接 MySQL：
```
mysql -h host.docker.internal -P 3306 -u root -p
```

或者在 C# 的 `connection string` 中：
```
var connectionString = "Server=host.docker.internal;Port=3306;Database=mydb;User Id=root;Password=mypass;";
```

#### 為什麼不直接用 localhost?
- **`localhost` 在容器中指的是容器本身**，而非主機。
- 使用 `host.docker.internal` 能夠解決這個問題，因為它是專門為容器與主機溝通而設的 DNS。
