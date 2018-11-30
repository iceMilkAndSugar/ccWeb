# 1.新建文件夹

## `main.jsp` 

把路径和要创建的名字传过去就好

```javascript
//在页面加载成功绑定事件
	$(document).ready(function(){
		$("button#newdir").click(function(){createDir()})
	})
	
	//新建文件夹
	function createDir(){
		var name = prompt("请输入新建文件夹的名称","新建文件夹");
		if(name!==null&name!==''){
			$.ajax({
				url:'createDir.action',
				type:'post',
				data:{
					path:"${path}",
					name:name
				},
				success:function(s){
					if(s.success){
						location.reload();
					}
					alert(s.msg);
				}
			});
		}
	}
```

## `StorageController.java`

```java
/**
 * 新建文件夹
 */
@RequestMapping("/createDir")
@ResponseBody
public Object createDir(HttpServletRequest request,
	HttpServletResponse response,
	String name,String path) {
User user = getSessionUser(request);
boolean success = swiftStorageService.newDir(
		user.getUsername(),path, name);
return new MessageBean(success , 
		success?Constants.SUCCESS_3:Constants.ERROR_4);
}
```



## `SwfitService.java`

```java
public boolean newDir(String username, String path, String name) {
		return  new SwiftDFS().createDir(username, path+name+"/");
}
```

