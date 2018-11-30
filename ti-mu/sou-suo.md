---
description: 搜索的jsp直接套main就行了
---

# 5.搜索

## `main.jsp`



```java
/**
 * 搜索
 * 
 * @param request
 * @param response
 * @param path
 * @return
 */
@RequestMapping("/search")
public ModelAndView search(HttpServletRequest request,
		HttpServletResponse response, String key) {
	ModelAndView view = new ModelAndView();
	User user = getSessionUser(request);
	List list = swiftStorageService.getSearchList(user.getUsername(), key);
	view.addObject("search", true);
	view.addObject("list", list);
	view.setViewName("/main");
	view.addObject("type", 0);
	return view;
}
```

## `StorageController.java`

```java
/**
 * 搜索
 * 
 * @param request
 * @param response
 * @param path
 * @return
 */
@RequestMapping("/search")
public ModelAndView search(HttpServletRequest request,
		HttpServletResponse response, String key) {
	ModelAndView view = new ModelAndView();
	User user = getSessionUser(request);
	List list = swiftStorageService.getSearchList(user.getUsername(), key);
	view.addObject("search", true);
	view.addObject("list", list);
	view.setViewName("/main");
	view.addObject("type", 0);
	return view;
}
```

## `SwfitService.java`

```java
//获得搜索文件列表
public List getSearchList(String username, String key) {
SwiftDFS swiftdfs = new SwiftDFS();
List list =swiftdfs.searchFile(username, key);
ComparatorFile comparator = new ComparatorFile();  
        if (!list.isEmpty()) {  
            synchronized (list) {  
                Collections.sort(list, comparator);  
            }  
        }  
return list;
}
```

