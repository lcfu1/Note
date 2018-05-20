<!-- 用于二维码 -->
<link rel="stylesheet" type="text/css" href="/{{ site.title }}/css/qrcode.css">
<script type="text/javascript" src="/{{ site.title }}/js/awesome-qr.js"></script>

<div id="qrcode">
	<div class="qrcode_container">
		<div class="qrcode_fl">
			<textarea class="qrcode_qr" id="qrtext" placeholder="请输入内容"></textarea>
			<div>
				<button class="qrcode_btn1" style="width: 260px;" onclick="generateQrcode()">生成二维码</button>
			</div>
		</div>
		<div class="qrcode_fr">
			<div class="qrcode_img">
				<img class="qrcode_img1" id="qrcodeIMG">
			</div>
			<button id="downloadqr" class="qrcode_btn1 download" style="width: 260px;display: none;">下载二维码</button>
		</div>
	</div>
</div>	

<script type="text/javascript">
$(document).ready(function(){
	$("#headerNAV a[href$='qrcode']").parent().addClass("active");
});
function generateQrcode(){
	new AwesomeQRCode().create({
	    text: document.getElementById("qrtext").value,
	    
	    dotScale: 1,
	    callback: function(dataURI){
	    		$(".download").show();
	        console.log(dataURI);
	    },
	    bindElement: 'qrcodeIMG'
	});
}

$("#downloadqr").click(function(){
	
	 var isChrome = window.navigator.userAgent.indexOf("Chrome") !== -1;
	 var src = $("#qrcodeIMG").attr("src");
	 if(isChrome){
		downloadImage(src);
	 }else{
		 post("/utils/download",{"image":src});
	 }
});


function post(path, params, method) {
    method = method || "post";
    var form = document.createElement("form");
    form.setAttribute("method", method);
    form.setAttribute("action", path);

    for(var key in params) {
        if(params.hasOwnProperty(key)) {
            var hiddenField = document.createElement("input");
            hiddenField.setAttribute("type", "hidden");
            hiddenField.setAttribute("name", key);
            hiddenField.setAttribute("value", params[key]);

            form.appendChild(hiddenField);
         }
    }

    document.body.appendChild(form);
    form.submit();
}


function downloadImage(src) {
    var a = $("<a></a>").attr("href", src).attr("download", "qrcode-wanandroid.png").appendTo("body");
    a[0].click();
    a.remove();
}

</script>