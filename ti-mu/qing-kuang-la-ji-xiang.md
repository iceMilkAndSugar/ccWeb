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

## `garbage.jsp`



