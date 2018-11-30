# 17.文件下载

## `main.jsp`

```java
function download(){
	var selectedPath = getSelectedPath()
	var path = ""
	if(selectedPath.length==1){
		if(selectedPath[0].lastIndexOf("/")==selectedPath[0].length-1){
			alert("暂不支持文件夹下载")
			return;
		}
		path += "path=" + selectedPath[0]
	}else{
		for(var i=0;i<selectedPath.length;i++){
			if(selectedPath[i].lastIndexOf("/")==selectedPath[i].length-1){
				alert("暂不支持文件夹下载")
				return;
			}
			path += "path=" + selectedPath[i] +(i<selectedPath.length-1?"&":"")
		}
		console.log(path)
	}
	downbyIframe("download.action?"+path)
}

function downbyIframe(url){
var iframe = $("iframe#downdown");
if(iframe.length==0){
	$("body").append("<iframe id='downdown' width=0 height=0 src="+url+"></iframe>")
}else{
	iframe.attr("src",url)
}
}
```

## `StorageController.java`

```java
//下载
@RequestMapping("/download")
@ResponseBody
public Object uploadfile(HttpServletRequest request,
		HttpServletResponse response,
		String[] path) {
	try {
		User user = getSessionUser(request);
		String export = request.getSession().getServletContext().getRealPath("/")+"export";
		if(path.length<1) return new MessageBean(false,"异常操作");
		for (int i = 0; i < path.length; i++) {
			path[i] = UtilTools.converStr(path[i]);
		}
		if (path.length==1) {
			byte[] oneFile = swiftStorageService.download(user.getUsername(), path[0]);
			response.setContentLength(oneFile.length);
			String[] temp = path[0].split("/");
			String filename = temp[temp.length-1];
			response.addHeader("content-disposition", "attachment;filename="+URLEncoder.encode(filename,"utf-8"));
			response.setContentType("application/octet-stream");
			return oneFile;
		}else {
			String tempDir = export + File.separator + "temp" + File.separator;
			File tempFile = new File(tempDir);
			if(tempFile.exists()){
				UtilTools.deletefile(tempDir);
			}
			tempFile.mkdirs();
			for (String p : path) {
				String[] temp = p.split("/");
				String filename = temp[temp.length-1];
				String packDir = tempDir + filename;
				OutputStream out = new FileOutputStream(packDir);
				out.write(swiftStorageService.download(user.getUsername(), p));
				out.flush();
				out.close();
			}
			UtilTools.WriteToTarGzip(export+ File.separator, "temp", "pack.zip");
			InputStream in = new FileInputStream(export+ File.separator+"pack.zip");
			byte[] zip = new byte[in.available()];
			in.read(zip);
			response.addHeader("content-disposition", "attachment;filename=pack.zip");
			response.setContentType("application/octet-stream");
			response.setContentLength(zip.length);
			return zip;
		}
	} catch (Exception e) {
		e.printStackTrace();
	}
	return null;
}
```

## `StorageController.java`

```java
//下载
public byte[] download(String username,String path) {
	SwiftDFS swiftDFS = new SwiftDFS();
	return swiftDFS.downloadFile(username+"/"+path);
}
```

