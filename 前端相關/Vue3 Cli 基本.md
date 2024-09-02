1.  index.html：網頁的進入點
2.  App.vue：App 組件，類似我們前幾篇文件中提到的Component，透過引用的方式引入到index.html中，文件中僅包含組件的HTML並使用`<templates>`標籤包住。
3.  main.js：Javascript的進入點，透過這個檔案的第1, 2兩行將vue和app這個component載入到index當中，透過第五行渲染出HTML並且透過第六行將他載入到index中ID為app的這個元素中。