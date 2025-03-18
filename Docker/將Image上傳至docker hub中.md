1.到[docker hub](https://hub.docker.com/)網站並登入帳號

================================================
2.開啟cmd，輸入`docker images`查看目前所擁有的images，可以看到目前有一個image
![[Pasted image 20240301154922.png]]

===================================================
3.使用 docker tag 給一個新標籤，在cmd輸入`docker tag coretest ayaowu/coretest`，將image的repository名稱改為 ayaowu/coretest

指令格式:
`docker tag {Image Name} {DockerHub帳號}/{Image Name}`
![[Pasted image 20240301165956.png]]

===================================================
4.輸入`docker push ayaowu/coretest`，將指定的image上傳至docker hub
![[Pasted image 20240301170134.png]]

===================================================
5.到docker hub查看Repositories中是否有剛剛上傳的image，如果有就是有成功上傳
![[Pasted image 20240301170431.png]]

參考:
把 Docker Image Push 到 Docker Hub
https://ithelp.ithome.com.tw/articles/10191139  
將我的 Image 推到 Docker Hub 儲存庫
https://ithelp.ithome.com.tw/articles/10333562