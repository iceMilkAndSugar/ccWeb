# 18.复制

## `main.jsp`

```javascript
$(document).ready(function() {
    $('a#copy').click(function(){copy()})
})

function copy(){
	var newpath = prompt("请输入复制的目标路径:\n(空白为根目录)");
	if(newpath!==null){
		var filepath = getSelectedPath();
		//var data = $.param({path : filepath,newpath:newpath},true);
		$.ajax({
			url:'copy.action',
			type:'post',
			data:{
				path : filepath[0],
				newpath:newpath
			},
			success:function(s){
				alert(s.msg);
			}
		});
	}
}
```

## `StorageController.java`

```java
/**
 * 复制
 */
@RequestMapping("/copy")
@ResponseBody
public Object copy(HttpServletRequest request,
		HttpServletResponse response,
		String newpath,String path) {
		User user = getSessionUser(request);
//		path = UtilTools.converStr(path);
//		newpath = UtilTools.converStr(newpath);
		System.out.println(path+" "+newpath);
		boolean success = swiftStorageService.copy(
				user.getUsername(),path, newpath);
		return new MessageBean(success , 
				success?Constants.SUCCESS_8:Constants.ERROR_9);
}
```

{% hint style="info" %}
有时候出现乱码问题需要转码`UtilTools.converStr()` 或者用其他办法
{% endhint %}

## `StorageController.java`

```java
//复制
public boolean copy(String username,String rpath,String rnewpath) {
	SwiftDFS swiftDFS = new SwiftDFS();
	boolean isDir = rpath.endsWith("/");
	rpath = username +"/" + rpath;
	rnewpath = username + "/" + rnewpath;
	String[] temp = rpath.split("/");
	String filename = temp[temp.length-1];
	
	if(isDir){
		return swiftDFS.copyDir(rpath, rnewpath);
	}else{
		System.out.println(rpath+"\n"+rnewpath+filename);
		return swiftDFS.copyFile(rpath, rnewpath+filename);//文件目标为文件
	}
}
```

