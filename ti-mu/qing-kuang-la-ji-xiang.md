# 6.清空垃圾箱

## `garbage.jsp`

```javascript
$(document).ready(function() {
		$("button#clearAll").click(function(){clearAll()})
	});
//清空回收站
function clearAll(){
	$.ajax({
		url:'cleargarbage.action',
		type:'post',
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
 * 清空回收站
 */
@RequestMapping("/cleargarbage")
@ResponseBody
public Object clearGarbage(HttpServletRequest request,
	HttpServletResponse response) {
	User user = getSessionUser(request);
	boolean success = swiftStorageService.clearAll(user.getUsername());
	return new MessageBean(success , Constants.SUCCESS_5);
}
```

## `SwfitService.java`

```java
//清空回收站
function clearAll(){
	$.ajax({
		url:'cleargarbage.action',
		type:'post',
		success:function(s){
			if(s.success){
				location.reload();
			}
			alert(s.msg);
		}
	});	
}
```

