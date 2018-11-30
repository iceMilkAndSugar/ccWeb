# 7.重命名

## `main.jsp`

```javascript
//在页面加载成功绑定事件
$(document).ready(function(){
	$('button#rename').click(function(){rename()})
	
})
//重命名相关事件
function rename(){
	var list = getSelected()
	if(list.size()==1){
		list.parents("tr").find('div.edit-name').show()
		list.parents("tr").find('td>span').hide()
	}
}
function sure(){
	var path = $("div.edit-name:visible").parents('tr').find('td.table-path').text()
	var name = $("div.edit-name:visible>input").val()
	$.ajax({
		url:"rename.action",
		type:"post",
		data:{
			path:path,
			newname:name
		},
		success:function(s){
			if(s.success){
				location.reload()
			}
			alert(s.msg)
		}
	})
}
function cancel(){
	$("div.edit-name:visible").parents('td').find('span:hidden').show()
	$("div.edit-name:visible").hide()
}
```

{% hint style="info" %}
这里有个`sure()`和`cancel()`是为了如下的效果
{% endhint %}

![](../.gitbook/assets/rename.gif)



![](https://github.com/iceMilkAndSugar/ccWeb/raw/master/.gitbook/assets/rename.gif)

## `StorageController.java`

```java
/**
 * 重命名文件
 */
@RequestMapping("/rename")
@ResponseBody
public Object rename(HttpServletRequest request,
	HttpServletResponse response,
	String path,String newname) {
	User user = getSessionUser(request);
	boolean success = swiftStorageService.rename(user.getUsername(),path,newname);
	return new MessageBean(success , 
			success?Constants.SUCCESS_7:"重命名失败");
}
```

## `StorageController.java`

```java
//重命名
public boolean rename(String username, String path, String newname) {
	SwiftDFS swiftDFS = new SwiftDFS();
	String rpath = username + "/" + path;
	if(path.endsWith("/")){
		return swiftDFS.renameDir(rpath, newname);
	}else{
		return swiftDFS.renameFile(rpath, newname);
	}
}
```

