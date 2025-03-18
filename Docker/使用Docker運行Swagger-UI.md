1.開啟cmd視窗，在裡面輸入下方指令，下載image檔案
```
docker pull docker.swagger.io/swaggerapi/swagger-ui
```

2.當已經下載了 `swagger-ui` 的 Docker image後，接著要使用`docker run`運行一個container
```
docker run -d -p 8080:8080 docker.swagger.io/swaggerapi/swagger-ui
```
- `-d`：讓 container 在背景執行（不顯示容器運行的輸出日誌）。
- `-p 8080:8080`：將本機的 `8080` 端口對應到 container 的 `8080` 端口。

3.執行完後，可以在瀏覽器開啟 `http://localhost:8080` 來查看 Swagger UI

---
### 使用自訂的OpenAPI Json檔案

1.運行 Swagger UI 並載入本機的 `swaggerExample.json`檔案
```
docker run -d -p 8080:8080 -e SWAGGER_JSON=/mnt/swaggerExample.json -v /你的本機路徑/swaggerExample.json:/mnt/swaggerExample.json docker.swagger.io/swaggerapi/swagger-ui
```
- `-e SWAGGER_JSON=/mnt/swaggerExample.json`：告訴 Swagger UI 要載入 `/mnt/swaggerExample.json`當作 API 文件。

- `-v /你的本機路徑/swaggerExample.json:/mnt/swaggerExample.json`：
	- `你的本機路徑/swaggerExample.json`：本機 `swaggerExample.json` 檔案的完整路徑。
	- `/mnt/swaggerExample.json`：在 container 內的對應掛載位置。

2.注意，在 Windows 上運行 Docker 時，路徑要使用 **Linux 格式**，假如檔案放在`C:\Users\user\Desktop\swaggerExample.json`，那路徑就要改成`/c/Users/user/Desktop`

上述範例實際例子會是
```
docker run -d -p 8080:8080 -e SWAGGER_JSON=/mnt/swaggerExample.json -v /c/Users/user/Desktop/swaggerExample.json:/mnt/swaggerExample.json docker.swagger.io/swaggerapi/swagger-ui
```

3.瀏覽 `http://localhost:8080`，查看是否有出現畫面