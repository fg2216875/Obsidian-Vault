Jquery選擇器參考
https://ithelp.ithome.com.tw/articles/10197416
Jquery 功能
-------------------------------
淡入效果
 $("#projectRecordList").fadeIn(3000);

淡出效果
 $("#projectRecordList").fadeOut(3000);
 -----------------------------------
 每次輸入觸發事件
       $("#MainContent_txtMessageEdit").on('input', function () {
		........................................
        })

-----------------------------------------
下拉選單設定預設值後，要主動觸發onchange事件
var value = $("#WP_RB_NO").val();
$("#WP_RB_NO").val(value).change();

----------------------------------------------
上傳檔案及抓取檔案名稱
<input id="selectFile" type="file" class="" onchange="checkFile(this)">   //此this代表這個input控制項

<script>
	function checkFile(file){     //參數file 相當於 $("#selectFile")[0].files[0]
        var name = file.files[0].name;  //取得檔案名稱(含副檔名)
        console.log(name);
    }
</script>

將檔案傳遞至Controller API
 <form id="jobTransferForm" method="post" action="@Url.Action("FileUpload","Test")" enctype="multipart/form-data">  
 <input id="selectFile" type="file" class="" onchange="checkFile(this)">
 </form>
 
 API:
public ActionResult SaveJobTransfer(HttpPostedFileBase selectFile){   
//在Controller中接收檔案時，要使用HttpPostedFileBase，參數名稱要與input控制項name相同
	.....................
}
//multipart/form-data： 不對字元編碼，在使用包含有上傳檔案控制元件的表單時，必須使用該值。






