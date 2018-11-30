# 19.移动

## `main.jsp`

```javascript
$(document).ready(function() {
    $('a#move').click(function(){move()})
})
function move(){
		var newpath = prompt("请输入移动的目标路径:\n(空白为根目录)");
		if(newpath!==null){
			var filepath = getSelectedPath();
			//var data = $.param({path : filepath,newpath:newpath},true);
			$.ajax({
				url:'move.action',
				type:'post',
				data:{
					path : filepath[0],
					newpath:newpath
				},
				success:function(s){
					alert(s.msg);
					if(s.success){
						location.reload();
					}
				}
			});
		}
	}
```

## `StorageController.java`

```java
/**
 * 移动
 */
@RequestMapping("/move")
@ResponseBody
public Object move(HttpServletRequest request,
		HttpServletResponse response,
		String newpath,String path) {
		User user = getSessionUser(request);
//		path = UtilTools.converStr(path);
//		newpath = UtilTools.converStr(newpath);
		System.out.println(path+" "+newpath);
		boolean success = swiftStorageService.move(
				user.getUsername(),path, newpath);
		return new MessageBean(success , 
				success?Constants.SUCCESS_9:Constants.ERROR_8);
}
```

## `StorageController.java`

```java
	public boolean move(String username, String rpath, String rnewpath) {
		SwiftDFS swiftDFS = new SwiftDFS();
		boolean isDir = rpath.endsWith("/");
		rpath = username +"/" + rpath;
		rnewpath = username + "/" + rnewpath;
		String[] temp = rpath.split("/");
		String filename = temp[temp.length-1];
		if(isDir){
			return swiftDFS.moveDir(rpath, rnewpath);
		}else {
			return swiftDFS.moveFile(rpath, rnewpath+filename);
		}
	}
```

