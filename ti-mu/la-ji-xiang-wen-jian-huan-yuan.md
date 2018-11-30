# 7.垃圾箱文件还原

## `garbage.jsp`

```javascript
$(document).ready(function() {
		$("button#restore").click(function(){restore()})
	})
//还原文件
function restore(){
	var selected = getSelected();
	var path = [];
	$(selected.parents("tr").find("td.table-path")).each(function(){
		path.push($(this).text());
	});
	console.log(path);
	var data = $.param({paths : path},true);
	$.ajax({
		url:'restore.action',
		traditional :true, 
		type:'post',
		data: data,
		success:function(s){
			if(s.success){
				location.reload();
			}
			alert(s.msg);
		}
	});
}
```

## `StorageController.java`

```java
/**
 * 还原多文件
 */
@RequestMapping("/restore")
@ResponseBody
public Object restore(HttpServletRequest request,
	HttpServletResponse response,String[] paths) {
	User user = getSessionUser(request);
	boolean success = swiftStorageService.restore(user.getUsername(),paths);
	return new MessageBean(success , "文件还原成功");
}
```

## `StorageController.java`

```java
//还原
public boolean restore(String username, String[] paths) {
	SwiftDFS swiftDFS = new SwiftDFS();
	for(String path : paths){
		boolean isDir = path.endsWith("/");
		swiftDFS.backgarbagefile(username, path, isDir);
	}
	return true;
}
```

