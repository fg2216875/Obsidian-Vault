1.安裝套件:
```
pip install <套件名稱>
```
例: `pip install requests`
pip會從Python Package Index (PyPI) 自動下載並安裝套件。

2.查看已安裝的套件:
```
pip list
```

3.移除套件:
```
pip uninstall <套件名稱>
```

4.安裝套件清單:
開源專案中通常會有一個 `requirements.txt` 文件列出所有所需的套件，可以透過以下方式安裝所有相關套件
```
pip install -r requirements.txt
```