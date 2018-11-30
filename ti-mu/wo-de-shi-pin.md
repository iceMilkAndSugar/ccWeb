# 9.我的文档

## `main.jsp`

```markup
<a class="list-group-item " id="type" href="category.action?type=2"><span
							class="glyphicon glyphicon-file left-icon"> </span>&nbsp;&nbsp;我的文档</a>
```

## `StorageController.java`

```java
/**
 * 取得分类文件
 */
@RequestMapping("/category")
public ModelAndView category(HttpServletRequest request,
		HttpServletResponse response, int type) {
	ModelAndView view = new ModelAndView();
	User user = getSessionUser(request);
	List list = swiftStorageService.getCategoryList(user.getUsername(),type);
	view.addObject("search", "false");
	view.addObject("list", list);
	view.setViewName("/category");
	view.addObject("type", type);
	return view;
}
```

## `StorageController.java`

```java
//获得要求分类的文件列表
public List getCategoryList(String username, int type) {
	SwiftDFS swiftdfs = new SwiftDFS();
	List list =swiftdfs.getCategoryStoredList(username, type);
	ComparatorFile comparator = new ComparatorFile();  
        if (!list.isEmpty()) {  
            synchronized (list) {  
                Collections.sort(list, comparator);  
            }  
        }  
	return list;
}
```

