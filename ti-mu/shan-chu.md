# 4.删除

## `main.jsp`

包括了多个删除文件和文件夹

```javascript
//删除文件
	function deletefiles(){
		if(confirm("确定要删除这些文件吗？")){
			var filepath = getSelectedPath()
			var data = $.param({path:filepath},true)
			$.ajax({
				url:'deleteFile.action',
				type:'post',
				data:data,
				success:function(s){
					if(s.success){
						location.reload()
					}
					alert(s.msg)
				}
			});
		}
	}
```

{% hint style="info" %}
 值得注意的是上面用了$.param\(\)这个序列化函数 方便后面的ajax的请求
{% endhint %}

## `StorageController.java`

```java
/**
 * 删除文件
 */
@RequestMapping("/deleteFile")
@ResponseBody
public Object deleteFile(HttpServletRequest request,
	HttpServletResponse response,
	String[] path) {
	User user = getSessionUser(request);
	boolean success = swiftStorageService.delFile(user.getUsername(),path);
	return new MessageBean(success , 
			success?Constants.SUCCESS_5:Constants.ERROR_6);
}
```

## `SwfitService.java`

```java
public boolean delFile(String username, String[] paths) {
	SwiftDFS swiftDFS = new SwiftDFS();
	for(String path : paths){
		String[] temp = path.split("/");
		String filename = temp[temp.length-1];
		boolean isDir = path.endsWith("/");
		System.out.println(path+"\n"+filename+"\n"+isDir);
		swiftDFS.deleteFile(username, path, filename, isDir);	
	}
	return true;
}
```

