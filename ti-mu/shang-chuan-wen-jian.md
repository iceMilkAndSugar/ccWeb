---
description: 项目默认帮我们实现了上传到tomcat的目录下 我们要做的就是把tomcat的文件上传到swift上
---

# 2.上传文件

## `main.jsp`

```javascript
$.ajax({
	url:'createFile.action',
	type:'post',
	data:{
		path:"${path}",
		name:fileName,
		filepath:filepath
	},
	success:function(s){
		if(s.success){
			location.reload()
		}
		alert(s.msg)
	}
})
```

## `StorageController.java`

```java
/**
	 * 上传tomcat文件到服务器
	 */
	@RequestMapping("/createFile")
	@ResponseBody
	public Object createFile(HttpServletRequest request,
			HttpServletResponse response,
			String name,String path,String filepath) {
			User user = getSessionUser(request);
			filepath = request.getSession().getServletContext()
					.getRealPath("/")+filepath;
			boolean success = swiftStorageService.createFile(
					user.getUsername(),path, name,filepath);
			return new MessageBean(success , 
					success?Constants.SUCCESS_4:Constants.ERROR_5);
	}
```

## `SwfitService.java`

```java
public boolean createFile(String username, String path, String name, String filepath){
    return new SwiftDFS().createFile(username, path, name, filepath);
}
```

